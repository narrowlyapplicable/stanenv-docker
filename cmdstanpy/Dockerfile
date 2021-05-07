FROM continuumio/miniconda3

WORKDIR /workdir

COPY ./myenv.yaml ./
RUN apt -y update && \
    apt -y install build-essential && \
    conda config --append channels conda-forge && \
    conda env update --prune --name base -f myenv.yaml && \
    conda clean -afy && \
    install_cmdstan

EXPOSE 8888
ENTRYPOINT ["jupyter-lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", "--NotebookApp.token='hogehoge'"]
CMD ["--notebook-dir=/workdir"]