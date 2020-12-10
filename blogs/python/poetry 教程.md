# [Poetry](https://python-poetry.org/) 教程

## 安装

### windows powershell

```shell
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -
```

安装程序会将 poetry 工具安装到 poerty 的 bin 目录中。在 Unix 上，它位于 `$HOME/.poetry/bin` 中，而在 Windows 上，它位于 `％USERPROFILE％\.poetry\bin` 中。

最后，打开一个新的 shell 并输入以下内容：

```shell
poetry --version
```

## 更新

```shell
poetry self update
```

## 基本用法

创建新的项目

```shell
poetry new poetry-demo
```

初始化预先存在的项目

```shell
cd pre-existing-project
poetry init
```

指定依赖项

```shell
poetry add pendulum
```

使用虚拟环境

使用 `poetry run` :

```shell
-- 使用 poetry run
poetry run python your_script.py
-- 激活虚拟环境
poetry shell
```

安装依赖项

```shell
poetry install
```
