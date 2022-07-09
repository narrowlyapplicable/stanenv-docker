FROM continuumio/miniconda3:latest

WORKDIR /workdir

COPY ./myenv.yaml ./
RUN conda config --append channels conda-forge && \
    conda config --remove channels defaults && \
    conda env update --prune --name base -f myenv.yaml && \
    # 統一性のため、myenv.yamlのchannelsには`nodefaults`を追加
    # 参照<https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#sharing-an-environment>
    conda clean -afy 
# 本来conda installのみでセットアップされるはずだが、Dockerでは上手くいかなかったためinstall_cmdstanを実行
# 通常の`install_cmdstan`ではImportErrorを発するため、Python経由で実行した
RUN apt -y update && \
    apt -y install build-essential && \
    python -c 'import cmdstanpy; cmdstanpy.install_cmdstan(overwrite=True)'

EXPOSE 8888
ENTRYPOINT ["jupyter-lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", "--NotebookApp.token='hogehoge'"]
CMD ["--notebook-dir=/workdir"]