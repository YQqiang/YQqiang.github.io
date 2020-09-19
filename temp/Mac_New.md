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

# 软件配置
