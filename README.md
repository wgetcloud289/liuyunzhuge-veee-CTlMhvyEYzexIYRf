
`uniapp`是一个使用Vue.js开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、以及各种小程序。深受广大前端开发者的喜爱。`uniapp`官方也提供了自己的IDE工具`HBuilderX`，可以快速开发`uniapp`项目。但是很多前端的同学已经比较习惯使用`VSCode`去开发项目，为了开发`uniapp`项目再去切换开发工具，而且对新的开发工具也要有一定的适应过程，大多数前端的同学肯定是不愿意的。下面我们就看看用`VSCode`如何搭建`uniapp`项目。


### 安装node和pnpm


`node`的安装我就不多说了，去官网下载，直接安装就可以了。node安装好以后，我们再来安装`pnpm`。咦？`node`安装完不是自带`npm`吗？这个`pnpm`又是啥？这里简单介绍一下`npm`和`pnpm`的区别，不做重点。使用 `npm` 时，依赖每次被不同的项目使用，都会重复安装一次。 而在使用`pnpm`时，依赖会被存储在一个公共的区域，不同的项目在引入相同的依赖时，会从公共区域去引入，节省了空间。


`pnpm`我们直接全局安装就可以了，执行以下的命令：



```
npm install -g pnpm

```

安装好以后，我们在命令行执行`pnpm -v`，能够看到版本号就说明安装成功了。


### 创建uniapp项目


由于我们要使用`VSCode`去开发项目，而且项目要使用`Vue3`和`TypeScript`，所以我们要使用命令行去创建`uniapp`项目。先进入我们存放`VSCode`的项目目录，我的项目目录是`D:\VSProjects`，进入后，执行命令如下：



```
npx degit dcloudio/uni-preset-vue#vite-ts 项目名称

```

`项目名称`写你自己真实的项目名称就可以了，我的项目叫做`my-vue3-uniapp`。这个命令会把官方提供的使用了`TypeScript`和`Vite`的`uniapp`项目模板下载下来，然后我们就可以去开发`uniapp`项目了。


我们使用`VSCode`打开项目，项目的目录如下：


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223140729-66209747.png)


我们可以看到`src`目录里的文件都是`uniapp`项目的文件，包括页面、样式、静态文件等，`src`目录外是整个项目的文件，如：`vite.config.ts`和`tsconfig.json`等。然后我们打开终端，使用`pnpm`命令安装一下依赖，执行命令如下：



```
pnpm i

```

执行完成后，我们熟悉的`node_modules`目录出现在了项目中，如图：


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223205589-1936143364.png)


然后我们运行项目，执行命令如下：



```
pnpm run dev:mp-weixin

```

上面的命令会把我们的代码编译成微信小程序代码，如图：


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223229755-401434918.png)


编译完成后，我们的项目中出现了`dist`目录，这个目录就是编译后的输出目录。然后我们打开微信小程序开发工具，目录选择`/dist/dev/mp-weixin`，如图：


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223244898-689736574.png)


AppID写我们自己的小程序的AppID，点击确定，


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223301835-1943616663.png)


看到这个画面，说明我们的`uniapp`项目搭建成功了，而且可以通过微信小程序开发工具去预览。我们可以通过`VSCode`在页面上添加些文字，看看微信小程序开发工具的画面是否有改变。这里就不给大家演示了。


### 添加uni\-ui扩展组件


在我们开发项目时，会用到各种组件，仅仅使用uniapp的内置组件是远远不够的，我们还需安装官方提供的扩展组件uni\-ui，怎么安装呢？我们同样使用`pnpm`命令去安装，在具体安装uni\-ui扩展组件之前，我们先需要安装`sass`和`sass-loader`，


安装sass



```
 pnpm i sass -D

```

安装sass\-loader



```
pnpm i sass-loader@v8.x

```

由于现在的node版本都是大于16的，所以，我们根据uniapp官方的建议，安装`v8.x`的版本。


最后我们安装uni\-ui，如下：



```
pnpm i @dcloudio/uni-ui

```

uni\-ui安装完成后，我们再配置`easycom`，`easycom`的好处是，可以自动引入uni\-ui组件，无需我们手动`import`，这对于我们开发项目来说非常的方便，我们打开`src`目录下的 `pages.json` 并添加 `easycom` 节点：



```
// pages.json
{
	"easycom": {
		"autoscan": true,
		"custom": {
			// uni-ui 规则如下配置
			"^uni-(.*)": "@dcloudio/uni-ui/lib/uni-$1/uni-$1.vue"
		}
	},
	
	// 其他内容
	pages:[
		// ...
	]
}

```

这样uni\-ui扩展组件就添加到我们的项目中了。


### Json文件的注释


我们在添加`easycom`的时候，发现`pages.json`文件中的注释是有错误提示的，我们想让Json文件中可以有注释，至少`pages.json`和`manifest.json`两个文件这种可以有注释，这个我们需要在`VSCode`中配置一下，打开`文件->首选项->设置`，如图：


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223327710-1327416201.png)


然后我们在`文本编辑器`中找到`文件`，再在`Associations`中添加项，如下：


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223342147-175873983.png)


然后我们回到`pages.json`和`manifest.json`这两个文件看一下，注释就不报错了。


### VSCode插件安装


到现在为止，我们的uniapp项目已经搭建起来了，而且已经可以正常运行了，两个比较重要的json文件中，注释文字也不报错了。但这离我们正常开发还差很多，我们在使用uniapp组件的时候，没有提示，这使得我们编写程序很不方便，我们可以安装几个uniapp插件解决这些问题。我们在`VSCode`的扩展商店中搜索一下uniapp，这里需要安装3个插件：


* uniapp小程序扩展
* uni\-create\-view
* uni\-helper


安装完之后，我们在编写页面时，会有提示：


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223358212-107644878.png)


在新建页面时，会有uniapp相关的选项：


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223416270-1602381901.png)


这些对于我们实际开发是非常由帮助的。


### 安装uniapp的types


我们可以看到vue文件中，uniapp的组件并没有变绿，说明ts是没有生效的，我们先把uniapp的类型文件安装一下，如下：



```
pnpm i -D @uni-helper/uni-app-types @uni-helper/uni-ui-types

```

我们在使用pnpm安装时，会报错，我们根据uni\-helper的官方文档中的提示，将 `shamefully-hoist` 为 `true`。这个需要我们找到家目录下的`.npmrc`文件，如图：


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223442805-328804650.png)


然后在文件中增加：



```
registry=http://registry.npm.taobao.org
shamefully-hoist=true

```

然后，我们再执行pnpm命令安装类型文件。安装完成后，在项目根目录下，打开`tsconfig.json`文件，在`types`中增加我们安装的类型：



```
{
  "extends": "@vue/tsconfig/tsconfig.json",
  "compilerOptions": {
    ……
    "types": [
      "@dcloudio/types",
      "@uni-helper/uni-app-types",
      "@uni-helper/uni-ui-types"
    ]
  }
	……
}

```

添加完成后，我们发现`compilerOptions`是有报错的，鼠标悬停上去发现：


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223506783-1829459183.png)


报错提示两个选项将要废弃，我们要把这个错误提示去掉，可以在文件中增加`"ignoreDeprecations": "5.0",`：



```
{
  "extends": "@vue/tsconfig/tsconfig.json",
  "compilerOptions": {
    "ignoreDeprecations": "5.0",
   ……
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"]
}

```

这样`compilerOptions`就不报错了。然后我们打开vue文件，发现uniapp的标签都变绿了，但是会有报错，这个`VSCode`的插件之间有冲突造成的，我们可以配置如下解决，参考官方文档：


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223523997-727367697.png)



```
{
  ……
  "vueCompilerOptions": {
    "plugins": ["@uni-helper/uni-app-types/volar-plugin"]
  },
  ……
}

```

然后重启`VSCode`。最后我们发现vue文件的uniapp标签变绿了，而且没有报错：


![](https://img2024.cnblogs.com/blog/1191201/202409/1191201-20240910223539800-801220710.png)


最后`tsconfig.json`的整体内容如下：



```
{
  "extends": "@vue/tsconfig/tsconfig.json",
  "compilerOptions": {
    "ignoreDeprecations": "5.0",
    "sourceMap": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    },
    "lib": ["esnext", "dom"],
    "types": [
      "@dcloudio/types",
      "@uni-helper/uni-app-types",
      "@uni-helper/uni-ui-types"
    ]
  },
  "vueCompilerOptions": {
    "plugins": ["@uni-helper/uni-app-types/volar-plugin"]
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"]
}


```

### 最后


到这里，我们的uniapp项目就搭建完成了，而且是使用我们非常熟悉的`VSCode`，项目中还是用了`Vue3`，`Typescript`，`Vite`，该装的插件也已经装上了，鼠标悬停会给我们组件的提示，大大提高了我们的开发效率。OK了，去开发我们的项目应用吧\~\~\~


 本博客参考[MeoMiao 萌喵加速](https://biqumo.org)。转载请注明出处！
