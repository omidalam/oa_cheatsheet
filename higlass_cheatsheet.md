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

## Adding mcool files

".cool" is a filesystem to store HiC data developed as part of cooler package. ".mcool" files can store HiC datasets in different zoom levels. Here is how to add them to HiGlass.

> docker exec higlass-container python higlass-server/manage.py   ingest_tileset   --filename /tmp/K562-Control_hg38.mcool   --datatype matrix --filetype cooler --project-name HeatShock --name "K562-Control(hg38)" --coordSystem hg38

> docker exec higlass-container python higlass-server/manage.py   ingest_tileset   --filename /tmp/K562-HS_hg38.mcool   --datatype matrix --filetype cooler --project-name HeatShock --name "K562-HeatShock(hg38)" --coordSystem hg38

