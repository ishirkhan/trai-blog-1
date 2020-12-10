# [Poetry](https://python-poetry.org/) 教程

## 安装

### windows powershell

```shell
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -
```

安装程序会将 poetry 工具安装到 poerty 的 bin 目录中。在 Unix 上，它位于 `$HOME/.poetry/bin` 中，而在 Windows 上，它位于 `％USERPROFILE％\.poetry\bin` 中。

最后，打开一个新的 shell 并输入以下内容：

```shell
# 检查版本
poetry --version
# 更新
poetry self update
```

## 基本用法

创建新的项目：

```shell
poetry new poetry-demo
```

初始化预先存在的项目：

```shell
cd pre-existing-project
poetry init
```

指定依赖项：

```shell
poetry add pendulum
```

使用虚拟环境：

```shell
# 使用 poetry run
poetry run python your_script.py
# 激活虚拟环境
poetry shell
# 要获得虚拟环境的路径，请运行 `poenry env info --path`。
```

安装依赖项：

```shell
poetry install
# 默认情况下，当前项目以可编辑模式安装。
# 如果只想安装依赖项，请使用 `--no-root` 标志运行 `install` 命令：
poetry install --no-root
```
