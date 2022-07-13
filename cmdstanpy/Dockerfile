FROM continuumio/miniconda3:latest

WORKDIR /workdir

COPY ./myenv.yaml ./
RUN conda config --append channels conda-forge && \
    conda config --remove channels defaults && \
    conda env update --prune --name base -f myenv.yaml && \
    # 統一性のため、myenv.yamlのchannelsには`nodefaults`を追加
    # 参照<https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#sharing-an-environment>
    conda clean -afy 

# RUN source ~/.bashrc 

EXPOSE 8888
ENTRYPOINT ["/bin/bash", "-l", "-c", "jupyter-lab --ip=0.0.0.0 --port=8888 --no-browser --allow-root --NotebookApp.token='hogehoge'"]
CMD ["--notebook-dir=/workdir"]