# ubuntu20.04-cuda11.2-cudnn8-py3.8.12-jupyterlab3-poetry

## 解説
- https://qiita.com/nokomoro3/items/ca92227fcd191dde5f17
## Setup

- In order to set the password, the following line needs to be changed.
  - [./jupyter_lab_config.py](./jupyter_lab_config.py)
    ```python
    password = "PASSWORD"
    ```

- build
```
docker-compose up -d
```

- Please access the following URL
  - http://localhost:8888/lab

## Package

- Package install uses poetry, so you can install it with the following command in ipynb file.
```ipynb
!poetry add {PACKAGE_NAME}
```
