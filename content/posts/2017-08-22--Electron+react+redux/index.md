---
title: Electron+react+redux开发桌面app中遇到的一些问题记录 
author: chie4
---

&emsp;&emsp;由于自身需要最近开发了一个图片下载工具，底层使用nodejs编写，通过构造了一个Promise队列控制了并发请求数量。稳定使用了一段时间后还是觉得缺点什么，正好最近也在学react相关的知识，那就干脆用Electron结合react和一些新技术做个界面吧！而且由于Electron的跨平台特性，也便于将我自己的写的c++、javascript脚本和工具分享给更多的人使用。

&emsp;&emsp;学习过程中参考一些比较有名的模板，比如[electron-react-boilerplate](https://github.com/chentsulin/electron-react-boilerplate)，不过这个作者最近更新维护的频率明显降低了。在使用这个项目构建时也是遇到不少深坑，比如有次react-router版本没装对，结果花了一下午才找到问题。

&emsp;&emsp;这个模板的webpack构建代码还是比较稳定的，但在react和redux这一层还是存在不少小问题。 当你的项目中只引用一个外部库时很难出现bug，尤其像redux和react这种成熟项目，经过大量的版本迭代，已经很难在里面找出明显的bug了。但当你同时引用很多个并且相互依赖的库时，各种问题就接踵而至了。你很难判断出现的bug是源自哪个库，因为从多个库里都可以修复这个问题。

&emsp;&emsp;这次也是跟随技术潮流选择了react全家桶，react-router、react-redux、react-router-redux等等。下面我把我在electron开发中遇到的问题列举一下，相信很多人多少会遇到类似的问题。

&emsp;&emsp;

## 使用`<Switch>`标签时，使用history.goForward()和history.goBack()路由跳转不正确
详细问题见这里[code](https://codesandbox.io/s/G6nWE3X0r)

&emsp;&emsp;这个问题只会在你同时使用react-router的`<Switch>`和react-router-redux时出现。当你按下后退按钮时，Store里的路由地址是正确，然而页面却没有跳转，而且每次你按下后退，你都会跳转到目标地址的下一个地址。就仿佛路由地址在Switch这一层阻塞了一样，并没有及时的传递下来。

&emsp;&emsp;其实这个问题解决方法也很简单，把正确的路由地址传递给`<Switch>`标签即可，代码如下：
```bash
const mapStateToProps = state => ({
  location: state.router.location
});

const ConnectedSwitch = connect(mapStateToProps)(Switch);
// 将这个ConnectedSwitch替换之前的Switch
```

如果你有对出现这个bug的原因感兴趣，请参考react-router的这一段源码[https://github.com/ReactTraining/react-router/blob/master/packages/react-router/modules/Route.js#L56](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/modules/Route.js#L56)

&emsp;&emsp;

## 使用react-router的history模块时，`Browserify`和`HashHistory`初始路径不一致

&emsp;&emsp;在使用electron-react-boilerplate构建项目时，发现`Browserify`和`HashHistory`初始路径不一致，使用`Browserify`时点击前进后退按钮跳转到错误的路由地址。
```bash
const history = createHashHistory();
console.log(history.location.pathname);     
//  "/"
```

```bash
const history = createBrowserHistory();
console.log(history.location.pathname);     
//  "/C:/Users/asdf/Documents/GitHub/electron-react-boilerplate/app/app.html"
//  This path matches the route "/C:/"
```

&emsp;&emsp;这里注意到BrowserHistory创建的默认地址不是'/'，而是electron应用启动的载入页面的文件路径！这里提供一个简单的解决方法，创建BrowserHistory时把根路径设置为相应路径即可。

```bash
const history = createBrowserHistory({
  basename: window.location.pathname
});
```

# 结尾

&emsp;&emsp;

&emsp;&emsp;暂时先写这两条，遇到其他问题之后再补充。过几天可能会写一点redux相关的文章。不得不说redux想优化到极致真是太难了，要填的坑超多，强迫症用这个简直想撞墙（摊手）