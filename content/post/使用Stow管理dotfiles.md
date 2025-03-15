---
date: 2025-03-15T14:53:51+08:00
title: 使用Stow管理dotfiles
tags:
  - Linux
categories:
  - Linux
keywords:
  - dotfiles
  - Stow
  - Linux
description: Dotfiles是Linux中的配置文件，通常散落在主目录下。Stow通过创建软链接，将Dotfiles集中到~/dotfiles目录中统一管理。本文介绍Stow的基本用法（安装、链接、删除、更新）和高级功能（如.stow-local-ignore、.stowrc），并提供管理Bash、Git、Vim等配置的示例，帮助用户高效组织Dotfiles，避免意外链接。
author: LYJ
slug: manage-dotfiles-with-stow
---
# dotfiles是什么？
在 Linux 系统下，软件的相关配置通常保存在用户的主目录（`$HOME`）下。例如，`bash` Shell 的配置文件就位于 `$HOME/.bashrc`。这些配置文件通常以点（`.`）开头，因此它们可以统称为 `dotfiles`。由于 `dotfiles` 默认是隐藏文件，且在 `$HOME` 目录下往往分散于多个不同的文件或目录中，直接管理它们会显得比较麻烦。常见的解决方法是通过将所有的配置文件放在一个名叫`dotfiles`的目录类，然后通过一些工具来高效管理`dotfiles`。
**常见的`dotfiles`工具：**
* **GUN Stow**：通过软链接的方式将点文件组织到一个目录，并将目录的中文件链接到对应的位置。*（我使用的工具）*
* **Chezmoi**：一个专门用于管理 `dotfiles` 的工具，支持跨平台和多机器同步。
* **Ansible**：通过自动化工具管理和部署 `dotfiles`。
# Stow用法
## Stow基本用法
**安装`stow`**
```
sudo pacman -S stow
```
`stow`的核心命令格式：
```
stow [OPTIONS] PACKAGE
```
* `PACKAGE`：软件包名（通常是目录名称）。
* `OPTIONS`：配置选项。
**创建软链接**
```
stow bash
```
**删除软链接**
```
stow -D bash
```
**更新软链接**
```
stow --restow bash
```
**模拟操作**
在实际进行软链接之前，可以使用`-n`选项来模拟操作并查看对应的结果：
```
stow -n stow
```
> 这里将会显示要创建软链接，但是不会实际执行。

**指定目录**
`stow`会默认将软链接添加到当前目录的父目录，如`$HOME/dotfiles`该目录的父目录则是`$HOME`。如果要将要更改软链接的模板目录可以使用`-t`选项。
```
stow -t /path/to/target bash
```
**忽略文件或目录**
如果想忽略某些文件或目录，可以使用 `--ignore=REGEX` 选项。例如，忽略所有 `.bak` 文件：
```
stow --ignore='\.bak$' bash
```

> bash代表软件包名，也就是对应的目录名称，`-`或`--`则是选项。

## Stow高级用法
### 配置`.stow-local-ignore`
`.stow-local-ignore` 文件用于指定在当前 `stow` 进行软链接时，需要忽略的文件或目录。它的文件内容是一个正则表达式列表，匹配的文件或目录将不会被 `stow` 处理。该文件需要放置在 **`dotfiles` 仓库的根目录**中（例如 `~/dotfiles/.stow-local-ignore`）。
**使用示例**
如果想要忽略所有以 `.bak` 结尾的文件，可以在 `.stow-local-ignore` 中添加以下内容：
```
.*\.bak$
```
### 配置`.stowrc`
`.stowrc` 文件用于设置`stow` 的全局配置选项。可以在此文件中指定 `stow` 的默认行为，例如目标目录、忽略模式等。该文件可以放置在 **用户的主目录（`~/.stowrc`）** 或 **Stow 仓库的根目录**（例如 `~/dotfiles/.stowrc`）。如果在两个位置都存在，`Stow` 会优先使用主目录中的配置。
**使用示例**
如果想要`stow`每次运行时都自动忽略`.git`目录，则可在`stowrc`中添加以下内容：
```
--ignore='^\.git$'
```
### 处理冲突
在进行软链接的时候，如果目标目录存在同名文件，则`stow`会报错，此时可以使用以下选项解决问题：
* `--adopt`：
* `--override`：
## 管理dotfiles
假设你的`dotfiles`目录结构如下：
```
~/dotfiles/
├── bash/
│   ├── .bashrc
│   └── .bash_profile
├── git/
│   ├── .gitconfig
│   └── .gitignore_global
└── vim/
    ├── .vimrc
    └── .vim/
```
**可以使用一下命令建立软链接：**
```
cd dotfiles
# 对bash git vim都建立软链接
stow bash git vim
```
如果`dotfiles`目录结构如下：
```
~/dotfiles/
├── .bashrc
├── .gitconfig
└── .vimrc
```
则可在`dotfiles`目录下使用`stow .`命令，该命令将会把`.bashrc`、`.gitconfig`、`vimrc`都链接到`$HOME`。 直接使用`stow .`命令可能会将`.git`目录链接到`$HOME`目录，建议 **创建一个 `.stow-local-ignore` 文件** 并显式忽略 `.git` 目录。具体步骤如下：
**创建`.stow-local-ignore` 文件**：
```
touch ~/dotfiles/.stow-local-ignore
```
**添加忽略规则**：
在 `.stow-local-ignore` 文件中添加以下内容：
```
^\.git$
```
