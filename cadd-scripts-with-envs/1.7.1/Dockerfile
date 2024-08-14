FROM visze/cadd-scripts-v1_7:0.1.0

ARG CADD_VERSION=1.7.1

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
    base_image="debian:bookworm"

# Add CADD directory (CADD.sh) and conda to the path
ENV PATH=/opt/CADD-scripts-${CADD_VERSION}:/opt/mambaforge/bin:$PATH

# Install a snakemake compatible with the base env from the upstream Docker container.
# For version 1.7.1 this is in practice snakemake 7.x.
RUN mamba upgrade -y mamba 'python=3.12'
RUN mamba install -y -c conda-forge -c bioconda -n base 'snakemake=8'

# Download and untar CADD from github.
RUN cd /opt && \
    wget -O - https://github.com/kircherlab/CADD-scripts/archive/refs/tags/v${CADD_VERSION}.tar.gz | tar xz && \
    cd CADD-scripts-${CADD_VERSION}
RUN ln -s /conda-envs /opt/CADD-scripts-${CADD_VERSION}/envs/conda

# Add CADD directory (CADD.sh) and conda to the path
ENV PATH=/opt/CADD-scripts-${CADD_VERSION}:/opt/conda/bin:/opt/conda/condabin:$PATH

