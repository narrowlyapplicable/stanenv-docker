FROM continuumio/miniconda3

WORKDIR /workdir

COPY ./myenv.yaml ./
RUN apt-get -y update && \
    apt-get -y install build-essential && \
    conda env update --prune --name base -f myenv.yaml

EXPOSE 8888
ENTRYPOINT ["jupyter-lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", "--NotebookApp.token='hogehoge'"]
CMD ["--notebook-dir=/workdir"]