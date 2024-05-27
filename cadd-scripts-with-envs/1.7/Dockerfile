FROM mambaorg/micromamba:1.5.3

ARG CADD_VERSION=1.7
ARG CADD_CONFIG_VERSION=1.7

MAINTAINER "Marius Bjoernstad" <pmb@fa2k.net>
LABEL software="cadd-scripts-with-envs" \
    software.version="$CADD_VERSION" \
    version="1" \
    about.summary="CADD is a tool for scoring the deleteriousness of single nucleotide variants as well as insertion/deletions variants in the human genome" \
    about.home="https://cadd.gs.washington.edu/" \
    about.documentation="https://github.com/BioContainers/containers/blob/master/cadd-scripts-with-envs/$CADD_VERSION/README.md" \
    about.license="Custom License" \
    about.license_file="https://github.com/kircherlab/CADD-scripts/blob/v$CADD_VERSION/LICENSE" \
    extra.binaries="/opt/conda/bin/cadd.sh" \
    about.tags="Genomics, Human geneitcs" \
    base_image="mambaorg/micromamba:1.5.3"

# Need to be root to install apt packages
USER root

# Install ps command required by some tools
RUN apt-get update && \
        apt-get install -y procps curl && \
        apt-get clean && \
        apt-get purge && \
        rm -rf /var/lib/apt/lists/* /tmp/*

# Install snakemake and mamba. mamba is required by snakemake (as it provides the conda command).
# bgzip (tabix package) is required by the pipeline.
RUN micromamba install -y -c conda-forge -c bioconda -n base snakemake mamba tabix && \
        micromamba clean --all --yes

# Download and untar CADD from github. Create a symlink with lowercase cadd.sh for
# compatibility with conda package.
RUN cd /opt && \
    curl -L https://github.com/kircherlab/CADD-scripts/archive/refs/tags/v${CADD_VERSION}.tar.gz | tar xz && \
    cd CADD-scripts-${CADD_VERSION} && \
    ln -s CADD.sh cadd.sh

# Add CADD.sh and conda to the path
ENV PATH=/opt/CADD-scripts-${CADD_VERSION}:/opt/conda/bin:/opt/conda/condabin:$PATH

# Run snakemake to include the necessary conda environments inside the container
RUN cd /opt/CADD-scripts-${CADD_VERSION} && \
	CADD=/opt/CADD-scripts-${CADD_VERSION} snakemake test/input.tsv.gz \
            --use-conda --conda-create-envs-only --conda-prefix envs/conda \
        	--configfile config/config_GRCh38_v${CADD_CONFIG_VERSION}.yml \
            --snakefile Snakefile

# Make the data directory into a symlink so the image can be used with the same
# bind paths as the bioconda-based image (cadd-scripts).
RUN mkdir -p /usr/local/share/cadd-scripts-${CADD_VERSION} && \
    mv /opt/CADD-scripts-${CADD_VERSION}/data /usr/local/share/cadd-scripts-${CADD_VERSION}/ && \
    ln -s /usr/local/share/cadd-scripts-${CADD_VERSION}/data /opt/CADD-scripts-${CADD_VERSION}/data
