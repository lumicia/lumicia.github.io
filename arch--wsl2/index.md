# 

# Arch + WSL2

下载 `arch.zip`：https://github.com/yuk7/ArchWSL/releases。

解压，执行 `Arch.exe`：

```bash
.\Arch.exe
```

成功后再次执行 `Arch.exe`。获取普通用户的 `root` 权限：

```bash
echo "%wheel ALL=(ALL) ALL" > /etc/sudoers.d/wheel
```

设置 `root` 密码：

```bash
passwd
```

创建用户并设置密码：

```bash
useradd -m -G wheel -s /bin/bash <username>

passwd <username>
```

设置默认用户：

```bash
exit

.\Arch.exe config --default-user <username>
```

在 `/etc/pacman.d/mirrorlist` 文件找到下行并取消注释：

```bash
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```

修改 `/etc/pacman.conf` 文件：

```bash
[archlinuxcn]
# 清华大学
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

初始化 `keyring`：

```bash
sudo pacman-key --init
sudo pacman-key --populate
sudo pacman -Syy archlinuxcn-keyring
```

关闭所有终端，然后重新打开 `Arch` 安装更新软件和包：

```bash
sudo pacman -Syu
sudo pacman -S zsh git clang lld rustup bat ripgrep fd starship zoxide neovim luajit gcc gdb make nodejs openssh gitui yarn 
```

更换默认 `shell` 为 `zsh`：

```bash
chsh -s /usr/bin/zsh
```

安装 `zsh` 插件：

```bash
sudo pacman -S zsh-autosuggestions zsh-syntax-highlighting zsh-completions
```

在 `.zshrc` 文件添加：

```bash
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
```

在 `zsh` 中启用 `starship`、`zoxide`，将下面代码添加到 `.zshrc` 文件中：

```bash
eval "$(starship init zsh)"

eval "$(zoxide init bash)"
```

`rustup` 换源，在 `.zshrc` 文件添加：

```bash
export RUSTUP_DIST_SERVER="https://rsproxy.cn"
export RUSTUP_UPDATE_ROOT="https://rsproxy.cn/rustup"
```

安装 `rust`：

```bash
rustup install nightly
```

添加 `rustup` 组件：

```bash
rustup component add rust-analyzer-preview rust-analysis miri rust-src
```

`cargo` 换源：

```toml
[source.crates-io]
replace-with = 'rsproxy'

[source.rsproxy]
registry = "https://rsproxy.cn/crates.io-index"

[registries.rsproxy]
index = "https://rsproxy.cn/crates.io-index"

[net]
git-fetch-with-cli = true
```

配置 `neovim`，当前比较流行采用 `packer.nvim` 包管理器进行插件管理。我参考的[配置](https://zhuanlan.zhihu.com/p/469355805)。

我遇到了 `telescope-fzf-native` 找不到 `fzf` 扩展的问题。在网上搜索后找到[解决方案](https://github.com/LunarVim/LunarVim/issues/1804)。输入以下命令，注意 `telescope-fzf-native` 的安装位置可能有所不同。

```bash
cd ~/.local/share/nvim/site/pack/packer/opt/telescope-fzf-native.nvim &&make clean && make
```

另外在 wsl2 中配置的主题似乎无效，只能选择 `windows terminal` 的[主题](https://windowsterminalthemes.dev/)。

使用 `ssh` 连接到 GitHub。首先配置用户，然后创建一个新的密钥：

```bash
git config --global user.name "your github name"
git config --global user.email "your github default email address"

ssh-keygen -t ed25519 -C "your_email@example.com"
```

可选的 `passphase`。显示 `~/.ssh/id_ed25519.pub` 公钥的内容然后复制：

```bash
bat ~/.ssh/id_ed25519.pub
```

到 https://github.com/settings/keys 点击 `New SSH key`，取名 `archlinux-wsl2-ssh`，粘贴公钥，点击 `Add SSH key`，输入 GitHub 密码确认。测试 SSH 连接：

```bash
ssh -T git@github.com
```

会显示 GitHub 的公钥，和[官方公布的公钥](https://docs.github.com/en/github/authenticating-to-github/githubs-ssh-key-fingerprints)对比验证后输入 `yes`。
