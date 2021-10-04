# notebooks

- docker
  - jupyterlab用docker環境

- pytorch-tutorial
  - 勉強会用のpytorchのチュートリアルnotebook

## jovian

- 以下にも公開している。
  - https://jovian.ai/nokomoro3/collections/pytorch-tutorial

## 環境構築方法(Windows)

- pyenv + poetry(global)で構築する方法を示す。
- 参考
  - https://qiita.com/kerobot/items/3f4064d5174676080585

### Pythonインストール

- 以下でインストール。今回は3.9.7を入れた。
  - https://www.python.org/downloads
- Add Python to Pathにはチェックを入れておく。

### poetryインストール

- powershellで以下を実行
```
> (Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -
```

- powershellを再起動し、以下でversionが表示されればOK。
```
> poetry --version
Poetry version 1.1.10
```

- 以下の設定を実行し、globalにpoetryを使うモードにする。
```
> poetry config virtualenvs.create false
```

- 適当なフォルダでpoetry initしておく。
```
> poetry init
```

### pyenvインストール

- python3.8でしかうまく動かない部分があるため、pyenvを使って、3.8を使う。

- pyenvのインストール
```
pip install pyenv-win --target $HOME\.pyenv
```

- powershellで以下を実行
```
> [System.Environment]::SetEnvironmentVariable('PYENV',$env:USERPROFILE + "\.pyenv\pyenv-win\","User")
> [System.Environment]::SetEnvironmentVariable('PATH', $HOME + "\.pyenv\pyenv-win\bin;" + $HOME + "\.pyenv\pyenv-win\shims;" + $env:Path,"Machine")
```

- version確認。
```
> pyenv --version
```

- 一覧を取得する。
```
> pyenv update
> pyenv install -l
```

- Python3.8.10をインストールする。
```
> pyenv install 3.8.10
```

- 3.8.10をグローバルにする。
```
> pyenv global 3.8.10
```

### jupyterlab

- poetryでjupyterlabを追加する。
```
poetry add jupyterlab
```

- 以下で起動できる。
```
poetry run jupyter notebook
```

### poetry add

- その他、tensorflowなどいろいろ入れる。
```
poetry add tensorflow
poetry add pandas
poetry add scikit-learn
```

### CUDA + cuDNNのインストール

- 以下から可能な組み合わせを探す。
  - https://www.tensorflow.org/install/source?hl=ja#gpu_support_2
- 今回は、CUDA11.0とcuDNN8.1を入れる。
- CUDAを入れる。
  - https://developer.nvidia.com/cuda-11.2.0-download-archive?target_os=Windows&target_arch=x86_64&target_version=10&target_type=exelocal
- 以下からcuDNNをダウンロードする。
  - https://developer.nvidia.com/rdp/cudnn-archive
- 解凍した一式を以下のCUDAフォルダに上書き移動する。
  - C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.0
- 以下のテストコードをpythonで動かす。
```python
import tensorflow as tf
print("Num GPUs Available: ", len(tf.config.experimental.list_physical_devices('GPU')))
# > Num GPUs Available: 1 など1以上が出ればOK
```
- dllが足りないなどの場合は、適当に似たファイルを複製すればOK。