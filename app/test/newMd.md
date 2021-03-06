#### 目录

  - [clone mpvue-quickstart模板](#clone mpvue-quickstart模板)
    - [注意⚠️](#注意⚠️)
      - [提示](#提示)
- [分包体验](#分包体验)
  - [初始化之后](#初始化之后)
  - [现有项目的分包改造](#现有项目的分包改造)

"这个功能可以说是让我们这些用mpvue的等的很焦灼，眼看着项目的大小一天天地逼近2M，mpvue还不能很好地支持分包加载，这可咋整？好消息是最近mpvue要支持分包加载了，不过目前在develop分支下面。下面我们一步步来看看怎么初始化一个支持分包加载的mpvue项目，以及不平滑的完成对老项目的改造。
<a name="clone mpvue-quickstart模板" id="clone mpvue-quickstart模板"><h3>clone mpvue-quickstart模板</h3></a>

初始化一个mpvue项目是基于mpvue-quickstart项目模板的，使用的是下面的命令:



但是这样是基于quickstart的master分支创建的项目，所以我们可以把这个模板clone下来，然后切换到develop分支上，再基于本地的模板创建一个新的mpvue项目，以下是一通（猛如虎的）操作:



这时，在项目的template/src目录下会有一个app.json文件，表明你现在在开发分支上。
<a name="注意⚠️" id="注意⚠️"><h4>注意⚠️</h4></a>

datdattt
<a name="提示" id="提示"><h5>提示</h5></a>

  66666666666
<a name="分包体验" id="分包体验"><h2>分包体验</h2></a>

首先用本地分mpvue模板初始化一个项目,参考vue-cli使用本地模板的[文档](https://github.com/vuejs/vue-cli/tree/v2#local-templates):


可以看到我们将模板替换成了本地的模板，后面的操作就熟悉了。
<a name="初始化之后" id="初始化之后"><h3>初始化之后</h3></a>

初始化好项目之后，我们来写一个分包加载的demo。进入项目目录，我们可以看到一个json文件，就是上面提到的app.json。然后参考小程序文档，加入subpackages的相关配置：


然后在pages/下，新建pA/a目录，在目录下再新建两个文件，`main.js`和`index.vue`，最终目录结构如下图所示:

<img width="300px" src="https://images2018.cnblogs.com/blog/1016471/201808/1016471-20180817223857688-655855736.png"/>

后面的操作就跟之前的mpvue开发过程一致了，这里不再赘述。直接贴上相关代码:


当点击上面的链接时，手机上会首先出现正在加载模块，然后跳转到build出来的`pages/pA/a/mian`页面,表示分包生效。
<a name="现有项目的分包改造" id="现有项目的分包改造"><h3>现有项目的分包改造</h3></a>

对于想将现有项目改造成支持分包的朋友，可能要麻烦一点，还要踩一点坑。下面我就详细说一下我们的改造过程以及遇到的坑。下面内容主要参考[issue 672](https://github.com/Meituan-Dianping/mpvue/issues/672)

- 将项目备份一份，包括依赖

	没有人希望分包改造不成功，还把原来能跑的搞的不能跑了，所以，先将整个项目复制一份，然后在副本里搞
    
- 升级依赖

    

- 修改webpack配置

	在这一步，会修改build目录下的`webpack.base.conf.js`,`webpack.prod.conf.js`,`webpack.dev.conf.js`三个文件，具体细节参考[这里](https://github.com/mpvue/mpvue-quickstart/pull/39/files)
    
- 修改config目录下的配置

	打开 config/index.js,将`assetsSubDirectory`字段的值由static改成''
    
    

- 将app.json的配置从main.js中移出来，命名为main.json

	之前mpvue将app.json写到main.js的export中，现在把它拿到同级目录下，新建一个main.json文件（注意是main.json，不是app.json），按小程序文档的格式粘贴进去。
    
![uploading-image-666580.png](https://images2018.cnblogs.com/blog/1016471/201808/1016471-20180823220142058-70937048.png)
    ￼
最后 `npm run dev`看看有没有跑起来(完)
￼
最近写东西越来越水了。。。"
