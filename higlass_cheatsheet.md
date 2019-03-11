--- 
title:  'Higlass cheat sheet'

author: 
- Omid Gholamalamdari
...

# Introduction

This cheat sheet is for setting up and managing a local docker container.
This is from [HiGlass](http://higlass.io/docs):
> HiGlass is a viewer for large-scale genomic data. It takes ideas introduced in genome browsers such as the UCSC Genome Browser and combines them with inspirations from more recent HiC data browsers such as Juicebox and implements them in a framework inspired by modern online maps (so-called slippy maps) to form a fast, extensible and responsive viewer for diverse types of genomic data."


## Setting up
docker stop higlass-container;
docker rm higlass-container;

docker pull gehlenborglab/higlass


docker run --detach \
           --publish 8889 \
           --volume ~/hg-data:/data \
           --volume ~/hg-tmp:/tmp \
           --name higlass-container \
         gehlenborglab/higlass


## Adding bedfiles.
1. You need to convert them using clodius. I would login to the higlass docker container and use the installed clodius on that. Had no success installing it from pip.

Loging to higlass-container docker
> docker exec -it higlass-container /bin/bash

> clodius aggregate bedfile -assembly hg19 sample.bed

2. copy the clodius output file (.beddb) to higlass tmp folder (it would be ~/hg-tmp/, if you followed the steps above)
3. add to higlass.

> docker exec higlass-container python higlass-server/manage.py ingest_tileset \
       --filetype beddb --datatype bedlike \
       --coordSystem hg19 --filename /tmp/sample.bed.beddb --project-name alaki


## Adding mcool files

".cool" is a filesystem to store HiC data developed as part of cooler package. ".mcool" files can store HiC datasets in different zoom levels. Here is how to add them to HiGlass.

> docker exec higlass-container python higlass-server/manage.py   ingest_tileset   --filename /tmp/K562-Control_hg38.mcool   --datatype matrix --filetype cooler --project-name HeatShock --name "K562-Control(hg38)" --coordSystem hg38

> docker exec higlass-container python higlass-server/manage.py   ingest_tileset   --filename /tmp/K562-HS_hg38.mcool   --datatype matrix --filetype cooler --project-name HeatShock --name "K562-HeatShock(hg38)" --coordSystem hg38

## Adding bedgraph files

clodius aggregate bedgraph          \
    /tmp/K562_hg38_ins_score_window100KB.bedgraph    \
    --output-file /tmp/K562_hg38_ins_score_window100KB.hitile \
    --chromosome-col 1              \
    --from-pos-col 2                \
    --to-pos-col 3                  \
    --value-col 4                   \
    --assembly hg38               \
    --nan-value NA                  


docker exec higlass-container python \
        higlass-server/manage.py ingest_tileset \
        --filename /tmp/K562_hg38_ins_score_window100KB.hitile \
        --filetype hitile \
        --datatype vector \
        --coordSystem hg38

## Bigwig files
docker exec higlass-container python \
        higlass-server/manage.py ingest_tileset \
        --filetype bigwig \
        --datatype vector \
        --coordSystem hg19 --filename /tmp/cnvs_hw.bigWig 

docker exec higlass-container python higlass-server/manage.py \
ingest_tileset --filetype chromsizes-tsv --datatype chromsizes --coordSystem hg38 --filename /tmp/hg38_2