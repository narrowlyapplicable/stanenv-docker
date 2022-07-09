FROM continuumio/miniconda3:latest
SHELL ["/bin/bash", "-c"]
# bashを使う　参照:<https://qiita.com/anagura0000/items/bef08bf129f1bd8529ce>
WORKDIR /workdir

COPY ./myenv.yaml ./
RUN conda config --append channels conda-forge && \
    conda config --remove channels defaults && \
    conda env update --prune --name base -f myenv.yaml && \
    # conda env create -f myenv.yaml && \
    conda clean -afy 
# RUN conda env export -n stan > ./frozen.yaml && \
# ENV CONDA_DEFAULT_ENV stan
# ENV PATH /opt/conda/envs/stan/bin:$PATH
# ENV PATH $CONDA_PREFIX/bin/cmdstan:$PATH
RUN apt -y update && \
    apt -y install build-essential && \
    # eval "$(conda shell.bash hook)" && \
    # 参照：<https://github.com/ContinuumIO/docker-images/issues/89#issuecomment-887935997>
    # conda init bash && \
    # conda deactivate && \
    # conda activate base && \
    python -c 'import cmdstanpy; cmdstanpy.install_cmdstan(overwrite=True)'
    # install_cmdstan && \
    # echo "conda activate stan" >> ~/.bashrc
    # 環境名はmyenv.yamlに合わせること

# GitHubを使う場合は以下も実行
# COPY .ssh /root/.ssh
# RUN chmod 600 /root/.ssh/*
EXPOSE 8888
ENTRYPOINT ["jupyter-lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", "--NotebookApp.token='hogehoge'"]
CMD ["--notebook-dir=/workdir"]