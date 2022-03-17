FROM jupyter/r-notebook:b6fdd5dae6cb

COPY --chown=${NB_UID}:${NB_GID} \
    etc/future-vars.sh /opt/conda/bin/future-vars.sh

RUN Rscript -e "install.packages(c('future', 'future.apply','doFuture'), repos='http://cran.us.r-project.org')"
