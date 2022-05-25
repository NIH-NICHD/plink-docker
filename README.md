# picard-docker
[![Docker](https://github.com/adeslatt/picard-docker/actions/workflows/docker-publish.yml/badge.svg)](https://github.com/adeslatt/picard-docker/actions/workflows/docker-publish.yml)[![Docker Image CI](https://github.com/adeslatt/picard-docker/actions/workflows/docker-image.yml/badge.svg)](https://github.com/adeslatt/picard-docker/actions/workflows/docker-image.yml)

Build a Container for bioconda picard

Steps to build this docker container.
1. Look up on [anaconda](https://anaconda.org/) the tool you wish to install
2. create an `environment.yml` file either manually or automatically
3. Use the template `Dockerfile` modifying if necessary (in our case we have no custom files for the `src` directory so we do not use that)
4. Build the Docker Image
5. Set up GitHub Actions

To edit the files:
1. Use a simple editor (Text Edit on Mac is fine) - we want to be cautious about unseen characters
2. Use emacs (can be installed with conda)
```bash
conda install -c conda forge emacs
```

To build your image from the command line:
* Can do this on [Google shell](https://shell.cloud.google.com) - docker is installed and available

```bash
docker build -t picard .
```

To test this tool from the command line 

Set up an environment variable capturing your current command line:
```bash
PWD=$(pwd)
```

Then mount and use your current directory and call the tool now encapsulated within the environment.
```bash
docker run -it -v $PWD:$PWD -w $PWD picard picard
```

## Testing docker with files

Assuming we have the appropriate data files

```bash
docker run -it -v $PWD:$PWD -w $PWD picard \
   picard FilterSamReads \
   REFERENCE_SEQUENCE=data/Homo_sapiens_assembly38.fasta \
   INPUT=data/HTP0003A.cram \
   OUTPUT=HTP0003A_filtered.cram \
   FILTER=includePairedIntervals \
   INTERVAL_LIST=data/test2.interval_list
```