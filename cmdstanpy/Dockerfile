FROM continuumio/miniconda3:latest

WORKDIR /workdir

COPY ./myenv.yaml ./
RUN conda config --append channels conda-forge && \
    conda config --remove channels defaults && \
    conda env update --prune --name base -f myenv.yaml && \
    # conda env create -f myenv.yaml && \
    conda clean -afy 
# 本来conda installのみでセットアップされるはずだが、Dockerでは上手くいかなかったためinstall_cmdstanを実行
# 通常の`install_cmdstan`ではImportErrorを発するため、Python経由で実行した
RUN apt -y update && \
    apt -y install build-essential && \
    python -c 'import cmdstanpy; cmdstanpy.install_cmdstan(overwrite=True)'
    # 別に仮想環境を作るパターンならば以下が必要
    # 参照：<https://github.com/ContinuumIO/docker-images/issues/89#issuecomment-887935997>
    # eval "$(conda shell.bash hook)" && \
    # conda init bash && \
    # conda deactivate && \
    # conda activate stan && \
    # echo "conda activate stan" >> ~/.bashrc
# GitHubを使う場合は以下も実行
# COPY .ssh /root/.ssh
# RUN chmod 600 /root/.ssh/*
EXPOSE 8888
ENTRYPOINT ["jupyter-lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", "--NotebookApp.token='hogehoge'"]
CMD ["--notebook-dir=/workdir"]