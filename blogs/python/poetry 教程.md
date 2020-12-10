# [Poetry](https://python-poetry.org/) 教程

## 安装

### windows powershell

```shell
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -

poetry --version
```

安装程序会将 poetry 工具安装到 poerty 的 bin 目录中。在 Unix 上，它位于 `$HOME/.poetry/bin` 中，而在 Windows 上，它位于 `％USERPROFILE％\.poetry\bin` 中。

