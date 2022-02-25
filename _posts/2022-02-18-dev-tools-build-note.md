---
title: "开发环境搭建手记"
tags: "tool note vim tmux"
---

### 安装终端应用(可选)

- 下载安装终端 [Alacritty][alacritty]。
- 下载安装字体 [Hack][hack-font]，安装方式参考 [如何在 Mac 上安装和移除字体][install-font-on-mac]。
- 编辑 `~/.config/alacritty/alacritty.yml`, 具体配置参考 [Dotfiles](#Dotfiles)

### Oh My Zsh 安装与配置

- 运行`sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"`

- 编辑 `~/.zshrc`

{% highlight bash %}

# 设置主题
ZSH_THEME="robbyrussell"

# 配置插件
plugins=(git fzf)

{% endhighlight %}

`zsh` 插件的安装使用见[这里](/2022/02/25/oh-my-zsh-setup.html)

### Neovim 与 Nvchad

安装 [Neovim][neovim] `brew install neovim`

安装 [Nvchad][nvchad]

```
git clone https://github.com/NvChad/NvChad ~/.config/nvim --depth 1
nvim +'hi NormalFloat guibg=#1e222a' +PackerSync
```

### Tmux

安装 tmux 配置 

```
$ cd
$ git clone https://github.com/gpakosz/.tmux.git
$ ln -s -f .tmux/.tmux.conf
$ cp .tmux/.tmux.conf.local .
```

### Dotfiles

建议使用的配置备份到Git项目, 例如:

[Dotfiles - Github][dotfiles]

#### 快捷键速查表

#### 参考

[fzf]: https://github.com/junegunn/fzf
[alacritty]: https://alacritty.org 
[dotfiles]: https://github.com/fuzhongqing/dotfiles
[hack-font]: https://github.com/ryanoasis/nerd-fonts/blob/master/patched-fonts/Hack/Regular/complete/Hack%20Regular%20Nerd%20Font%20Complete.ttf?raw=true
[install-font-on-mac]: https://support.apple.com/zh-cn/HT201749
[neovim]: https://neovim.io
[nvchad]: https://nvchad.github.io
