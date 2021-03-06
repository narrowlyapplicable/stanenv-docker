FROM ubuntu:20.04

COPY ./requirements.txt ./
RUN apt-get -y update && \
    apt-get -y install python3 && \
    apt-get -y install python3-pip && \
    pip3 install --upgrade pip && \
    pip3 install cmdstanpy[all] && \
    pip3 install -r requirements.txt && \
    python3 -c 'import cmdstanpy; cmdstanpy.install_cmdstan()'

EXPOSE 8888
ENTRYPOINT ["jupyter-lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", "--NotebookApp.token='hogehoge'"]