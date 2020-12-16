# 命令s

## 打开某个文件或目录

```shell
# Windows
explorer[.exe] filename
start filename
```

## 用 VsCode 打开

```shell
code filename  # 可以是文件或目录路径
```

## 创建文件

```shell
# Windows
New-Item -Path 'C:\temp\New Folder' -ItemType Directory
New-Item -Path 'C:\temp\New Folder\file.txt' -ItemType File
```

> **重要**：如果对已存在的文件使用 `New-Item -Force` ，此文件 会被完全覆盖。