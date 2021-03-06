# Docker-composeによる CmdStanPy & PyStan2 環境
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
- docker-compose
  - いずれもdocker-composeから起動している
  - 起動の際、カレントディレクトリの内容をコンテナ内の`workdir/`にマウントしている
    - 試したいJupyter Notebookがあれば、 `workdir/`に置けばすぐ試行できる
- simple-cmdstan
  - Dockerfile内でcmdstanpy + `requirements.txt`にあるパッケージを`pip install`している
    - この方法は@amber_kshzさんの[qiita記事](https://qiita.com/amber_kshz/items/172e88e5feda1e7e3133)を参考にした
    - conda(miniconda)によるインストールを試みたが、cmdstanpyインストール後の`install_cmdstan`が失敗するため断念した
  - 必要なライブラリの変更 or バージョン指定は`requirements.txt`を編集する
- pystan2
  - minicondaを使って`myenv.yaml`内のパッケージをインストールしている
    - Dockerfile内での`conda activate`が失敗するため、`conda env create`ではなく `conda env update　--prune`でbase環境に各パッケージをインストールしている
    - ライセンス対策でanacondaを避ける場合は、Dockerfile内に下記を追加すれば良い
        >conda config --add channels conda-forge  
        >conda config --remove channels anaconda  
