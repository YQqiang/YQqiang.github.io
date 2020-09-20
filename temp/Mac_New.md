[Toc]

# 系统配置
## 触摸板
* 滚动方向自然
* 轻点来点按
* 启动三指拖移
* 四指页面切换

## 快捷键
* 输入法切换 `⌘空格键`
* 搜索 `⌃空格键`

## Touch Bar
* 显示功能栏
* siri 改为 锁屏

## Launch pad
设置图标尺寸（数字改为 `default` 可恢复默认）

```
defaults write com.apple.dock springboard-rows -int 6
defaults write com.apple.dock springboard-columns -int 7
killall Dock
```

## 密码限制
去除四位密码限制

```
pwpolicy -clearaccountpolicies
```

# 工具配置
## 浏览器
### Chrome

## 梯子
### V2Ray
https://github.com/Cenmrev/V2RayX

### Proxifier
https://xclient.info/s/proxifier.html

## 终端
### iTerm
官网下载即可：https://www.iterm2.com/
切换 zsh

```
chsh -s /bin/zsh
```
### Oh my zsh
curl 安装

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### PowerLine

```
sudo easy_install pip
pip install powerline-status --user
```

### PowerFonts 字体

```
git clone https://github.com/powerline/fonts.git --depth=1
cd fonts
./install.sh
```

### altercation 配色
双击 `Solarized Dark.itermcolors` 和 `Solarized Light.itermcolors` 即可安装明暗两种配色
```
git clone https://github.com/altercation/solarized
cd solarized/iterm2-colors-solarized/
open .
```

### agnoster 主题

```
git clone https://github.com/fcamblor/oh-my-zsh-agnoster-fcamblor.git
cd oh-my-zsh-agnoster-fcamblor/
./install
```
拷贝完成后，执行命令打开zshrc配置文件，将ZSH_THEME后面的字段改为agnoster。

```
vi ~/.zshrc
```

### 高亮插件

```
cd ~/.oh-my-zsh/custom/plugins/
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
```
编辑 `.zshrc` 文件

```
vi ~/.zshrc
plugins=(
         git
         zsh-syntax-highlighting
        )
```
使更改生效
```
source ~/.zshrc
```

# 软件配置
