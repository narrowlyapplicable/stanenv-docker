# Docker-composeによる CmdStanPy & PyStan2 環境
- docker-composeコマンド一発で、Stan付きJupyterLabを使用可能にする  

## 利用法
1. 使いたい環境のディレクトリに移動し`docker-compose up`
   - cmdstanpy
     >cd simple-cmdstan  
     >docker-compose up  
   - pystan2
     >cd pystan2  
     >docker-compose up  
2. ブラウザからJupyter Labを開く
   - `localhost:8080/lab`

## 詳細
- 共通
  - いずれもdocker-composeから起動している
  - 起動の際、カレントディレクトリの内容をコンテナ内の`workdir/`にマウントしている
    - 試したいJupyter Notebookがあれば、 起動するディレクトリ下に置けばすぐ試行できる
    - docker-compose.yamlからvolumes指定する方法は@komiya_____さんの[記事](https://qiita.com/komiya_____/items/96c14485eb035701e218)を参考にした

- simple-cmdstan
  - Dockerfile内でcmdstanpy + `requirements.txt`にあるパッケージを`pip install`している
    - conda(miniconda)によるインストールを試みたが、cmdstanpyインストール後の`install_cmdstan`が失敗するため断念した
    - そのため@amber_kshzさんの[qiita記事](https://qiita.com/amber_kshz/items/172e88e5feda1e7e3133)を参考に、pipによるインストールに切り替えた
  - 必要なライブラリの変更 or バージョン指定は`requirements.txt`を編集する
  
- pystan2
  - minicondaを使って`myenv.yaml`内のパッケージをインストールしている
    - Dockerfile内での`conda activate`が失敗するため、`conda env create`ではなく `conda env update　--prune`でbase環境に各パッケージをインストールしている
      - この方法を思いつくまでが長かった…
  - ライセンス対策でanacondaを避ける場合は、Dockerfile内に下記を追加すれば良い
    >RUN conda config --add channels conda-forge  
    >RUN conda config --remove channels anaconda  
  - ライブラリの変更・バージョン指定は`myenv.yaml`を編集する

## 速度比較
- 8schoolsを実行し、コンパイルおよびサンプリングの速度比較
  - 各ディレクトリ下`/demo`内の`hello_world.ipynb`参照

- コンパイル
  - 前評判通り、コンパイル速度はCmdStanPyの方がかなり（３倍強）速い
  - CmdStanPy
    >CPU times: user 5.14 ms, sys: 4.22 ms, total: 9.37 ms  
    >Wall time: 17.5 s  
  - PyStan2
    >CPU times: user 1.09 s, sys: 73.1 ms, total: 1.16 s  
    >Wall time: 1min 5s  

- サンプリング
  - サンプリング速度は大差ない (PyStan側にcondaを使用している影響はあるかも)
  - CmdStanPy
    >CPU times: user 48.1 ms, sys: 30.6 ms, total: 78.7 ms  
    >Wall time: 238 ms  
  - PyStan2
    >CPU times: user 52.8 ms, sys: 21.1 ms, total: 73.9 ms  
    >Wall time: 163 ms  

## TODO
- PyStan3が正式リリースされた後、PyStan3用のDockerfileも追加する
