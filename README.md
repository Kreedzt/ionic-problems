# ionic踩坑之路

> 版本: Angular@5, ionic@3, cordova@8.0.0, 安卓平台

## 编译问题

### 安卓平台问题

- 安卓平台版本问题

**警告**: 截止目前, 使用`ionic cordova platform add android@7.0.0`添加平台后, 90%插件不兼容安卓7, 导致编译后`platforms/android`的插件引入目录不对(安卓7与7以下插件目录不对).为避免频繁手动修改目录, 添加平台时请添加安卓6版本

例:

```zsh
ionic cordova platform add android@6.4.0 # 多半使用者也使用此版本
```

- 安卓平台编译后SDK限制不匹配问题

## 组件内方法问题

- 下拉刷新问题(`ionRefresh`)

使用官方提供的`Refresher`下拉刷新后, 当前`page`固定定位失效

官方API:

```HTML
<ion-content>

  <ion-refresher (ionRefresh)="doRefresh($event)">
    <ion-refresher-content></ion-refresher-content>
  </ion-refresher>

</ion-content>
```

[官方issue: 仍未解决](https://github.com/ionic-team/ionic/issues/13237)


## 插件问题

### 使用cordova原生插件方法(ionic没有情况下)

### ionic插件问题

- `Videogular`插件若无法正常工作, 需要制定版本为`6.1.1`
