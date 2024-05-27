# Container image for CADD-scripts

This container image lets you use the CADD scripts snakemake pipeline to annotate variants.
Documentation for CADD-scripts is here:

https://github.com/kircherlab/CADD-scripts


To use this container image you also need to mount the CADD annotation data. Assume you have
downloaded the reference data to a directory `$REFPATH`, such that it looks something
like this:

    $ ls $REFPATH
    GRCh38_v1.7

Then use the following options to make it available in the container.

Singularity/Apptainer:

    -B $REFPATH:/opt/conda/share/cadd-scripts-1.6-1/data/annotations

Docker:

    -v $REFPATH:/opt/conda/share/cadd-scripts-1.6-1/data/annotations


Technically, the data directory is redirected to the above mount points using a symlink
from /opt/CADD-scripts-${CADD_VERSION}. This keeps the image compatible with the bioconda-based
image "cadd-scripts".
