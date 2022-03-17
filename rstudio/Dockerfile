FROM jupyter/r-notebook:b6fdd5dae6cb as base

COPY --chown=${NB_UID}:${NB_GID} \
    etc/future-vars.sh /opt/conda/bin/future-vars.sh

RUN Rscript -e "install.packages(c('future', 'future.apply','doFuture'), repos='http://cran.us.r-project.org')"

FROM base as rstudio

RUN pip install --no-cache-dir \
        jupyter-server-proxy==3.2.0 \
        jupyter-rsession-proxy==2.0.1 && \
    jupyter labextension install @jupyterlab/server-proxy@3.2.0 && \
    jupyter lab build

USER root

RUN curl --silent -L --fail \
        https://download2.rstudio.org/server/bionic/amd64/rstudio-server-2021.09.1-372-amd64.deb > /tmp/rstudio.deb && \
    apt-get update && \
    apt-get install -y --no-install-recommends /tmp/rstudio.deb && \
    rm /tmp/rstudio.deb && \
    apt-get clean

RUN mamba clean -tipsy && \
    jupyter lab clean && \
    jlpm cache clean && \
    npm cache clean --force && \
    find /opt/conda/ -type f,l -name '*.a' -delete && \
    find /opt/conda/ -type f,l -name '*.pyc' -delete && \
    find /opt/conda/ -type f,l -name '*.js.map' -delete && \
    rm -rf /opt/conda/pkgs && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

COPY --chown=${NB_UID}:${NB_GID} \
    scripts/startup.R ${HOME}/work/

USER ${NB_UID}
