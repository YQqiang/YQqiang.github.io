# 系统配置
## 触摸板
* 滚动方向自然
* 轻点来点按
* 启动三指拖移
* 四指页面切换

## 快捷键
* 输入法切换 `⌘空格键`
* 搜索 `⌃空格键`

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
### Proxifier

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

# 软件配置
