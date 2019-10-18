# Brew 相关

安装好的 brew 目录为 `/usr/local/Homebrew/`，对应管理工具安装的路径为 `/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core`。

## Brew 回退

未删除早期下载的版本时, 可以参考这篇文章: [brew管理node的版本](https://my.oschina.net/uniquejava/blog/491030).

如果删除了早期的版本, 可以在 GitHub 找到之前的版本对应的提交, 切换到这次提交后执行以下语句进行安装对应的版本

```bash
# 设置不自动 update
export HOMEBREW_NO_AUTO_UPDATE=1
brew install <application>
```

## 使用镜像

为快速更新/下载 Brew 提供的应用, 可以配置国内的镜像加速下载。 以下配置来自于[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/)。

```bash
git -C "$(brew --repo)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git
brew update
```

```bash
git -C "$(brew --repo)" remote set-url origin https://github.com/Homebrew/brew.git
git -C "$(brew --repo homebrew/core)" remote set-url origin https://github.com/Homebrew/homebrew-core.git
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://github.com/Homebrew/homebrew-cask.git
brew update
```
