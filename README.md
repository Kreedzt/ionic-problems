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

在编译后的安卓平台内修改`build.gradle`文件

末尾增加如下内容:
```gradle
configurations.all {
    resolutionStrategy {
        force 'com.android.support:support-v4:27.1.0'
    }
}
```

### 编译时卸载/重添加平台报错问题

- 使用`ionic cordova platform remove android`或卸载平台后使用`ionic cordova platform add anroid`添加平台报错

    原因可能是没有管理员权限，若管理员打开后依旧报错，则可能是Cordova配置缓存没有删除（从提示可以看出）:
    
    通常定位目录到`C:\Users\lenovo\.config\configstore`内，删除所有文件，重新进行操作即可

## 组件内方法问题

- 下拉刷新问题(`ionRefresh`)

Q: 使用官方提供的`Refresher`下拉刷新后, 当前`page`固定定位失效

官方API:

```HTML
<ion-content>

  <ion-refresher (ionRefresh)="doRefresh($event)">
    <ion-refresher-content></ion-refresher-content>
  </ion-refresher>

</ion-content>
```

[官方issue: 仍未解决](https://github.com/ionic-team/ionic/issues/13237)
   
> 可以尝试使用ion-header尽力绕开该问题

- Tabs的跳转问题

```typescript
// 使用如下代码时，跳转可能失效
this.navCtrl.push(Page)
```

该问题可能是因为Tabs使用了懒加载，首次进入App时该页面未初始化，导致无法调用此页面的方法/无法直接路由跳转此页面

因Tab的实现方式与子页面不同，非hash路由，所以此处只能使用官方的`select`方式跳转

```typescript
// 例：在Tabs跟页面下使用如下方式跳转，因为不是路由跳转，无法传递参数
this.navCtrl.parent.select(2);
```

在非跟页面下时如上方法依旧无法跳转





## 插件问题

### 使用Cordova原生插件方法(ionic没有情况下)

使用时在前缀加上window即可。
由于TypeScript代码检查，可能直接添加window会报错，前缀再加上`<any>`泛型即可：

```TypeScript
// 例：官方文档使用时直接使用plugin
// 官方：
plugins.xxx

// 使用：
(<any>)window.plugins.xxx

// 官网文档使用时有cordova时：
// 官方:
cordova.plugins.xxx

// 使用：
(<any>)window.cordova.plugins.xxx
```


### ionic插件问题

- `Videogular`插件若无法正常工作, 需要制定版本为`6.1.1`
