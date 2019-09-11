# mac oh-my-zsh + iTerm2

## Install

**[iTerm2](https://iterm2.com/downloads/stable/latest)**

**Homebrew**

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

**[Git](https://git-scm.com/download/mac) 或者 `brew install git`**

```shell
brew install git
```

**Oh My Zsh**

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

**powerline 字体**

```shell
git clone https://github.com/powerline/fonts.git --depth=1
cd fonts
./install.sh
```

## Configuration

打开 iTerm2 偏好设置 ( `command + ,` ) 中的 **Profiles** 菜单：

**General** → **Font** : 选择 <u>Meslo LG S for Powerline</u> ；

**Colors** → **Color presets...** : 选择 *Solarized Dark* 。

编辑 *~/.zshrc* 文件：

```shell
ZSH_THEME="agnoster"	# 使用主题 agnoster
DEFAULT_USER="$USER"	# 缩短前缀
```

## Plugins

**语法高亮**

```shell
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

plugins=( [plugins...] zsh-syntax-highlighting)
```

**自动补全 ( 到官网下载 ) ：https://mimosa-pudica.net/zsh-incremental.html**

```shell
# 在 ~/.oh-my-zsh/custom/plugins 文件夹中创建一个名为 incr 的目录（为了方便管理），
# 将下载好的文件拷贝进去，执行：
cd cd .oh-my-zsh/custom/plugins/incr
source incr * .zsh
```

**自动建议**

```shell
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

plugins=(zsh-autosuggestions)
```

> 请打开 iTerm2 偏好设置：Profiles → Colors → ANSI Colors → Bright → Black (405870)，以看清建议文字的颜色。

**z ( 快速访问目录 ) 插件**

```shell
plugins=(z)
```

