## 如何使用Tree Shaking 清理你的JavaScript ## 

The world of JavaScript development can be frustrating and exciting.
JavaScript的发展正变得令人激动和兴奋.

Every day, new libraries and modules are published, and it can feel overwhelming to try to keep up. On the other hand, all of this change has its benefits. As a community, we’re heading more and more toward an ecosystem that’s easier to work in and reason about. We keep getting new candy, and it makes our lives better!
每一天都有新的库和模块发布,并且在不断推进中.另一方面,这些变化同时也有它的好处. 作为一个社区,我们正在走向一个越来越容易工作在其中和理性的生态系统中.我们不断收获新的好的工具,并且正变得越来越棒!

One JavaScript improvement that’s been getting some attention lately is the idea Tree Shaking. What’s that, you ask? Simply put, it’s a way to clean up your bundling process by excluding code you’re not using.
JavaScript界，一个最近正在受到关注的的想法是 Tree Shaking. 这是什么那？简单来说就是一个清理你打包过程，通过排除掉那些你并没有使用的代码.

We all hate bloat in our projects. Unused code increases mental overhead and makes it much harder to understand what’s going on. More importantly, it increases the size of the payload we’re sending to users in front-end projects.
我们都痛恨项目大小的膨胀.无用的代码加重了我们的精神压力,让我们更难以理解正在进行的工作.更严重的是,它增加了发送给用户的前端项目的体积大小.

In this post, we’ll take a look at tree shaking with JavaScript. What it is, how it works, and how to get started using it.
Let’s create a cleaner build for your project!
在下面的介绍中,我们将概览tree shaking 在JavaScript中的应用-- 它是什么? 如何工作? 并且如何开始使用它... 让我们一起创建一个更干净的项目构建吧!

Where Did It Come From?
The idea of tree shaking has begun gaining traction in the JavaScript world thanks to Rich Harris’s Rollup project.
Rollup is a JavaScript module bundler. From early on, it supported EcmaScript 6 (ES6) modules, which resulted in its creating smaller bundles. It also supports some sweet features like cyclical requires and export bindings.

### Tree Shaking 从哪里来? ### 
tree shaking 的想法的不断演进要感谢Rich Harris的Rollup 项目.

Rollup是一个JavaScript模块打包工具. 从一开始,它就支持ES6模块,导致其创建更小的包. 它也支持一些非常好的特性比如循环依赖和导出绑定.

But perhaps the most unique feature of Rollup is the fact that it only requires modules that you’ve actually imported somewhere. This means that unused code never makes it into the bundle. Note that this is slightly different than Dead Code Elimination. Roman Luitikov does a good job explaining this distinction in his post about Tree Shaking.

可能Rolup最独特的功能是他实际上只需要你将某处的模块导入就可以了.这意味着无用的代码根本不会被打包.你会发现这和普通的死代码清楚有略微的不同.Roman Luitkov 在他关于Tree Shaking的文章中对二者的区别做了很好的解释.

At the moment, a lot of people in the JS community are coalescing around Webpack as a build tool. Webpack’s maintainers are currently working on a Webpack 2 release. One of its interesting features is that it will support native ES6 modules without first transforming them into a CommonJS format. This is good, because tree shaking won’t work with CommonJS modules.

目前,js社区中的许多人都将webpack作为构建工具进行了整合. webpack的维护者目前正在专注于webpack2 的release版. 其中最有趣的特性之一就是他将支持ES6原生module而不是将其转化成CommonJS模块. 这是很棒,因为tree shaking 将无法用于CommonJS模块.

Recently, the estimable Dr. Axel Rauschmayer wrote a popular post about setting up tree-shaking with Webpack 2. I decided to give it a try in my colleague Marc Garreau’s Redux Starter Kit, to see what it would actually take to get tree shaking to work in a full project. You can see the result of my experiments on my GitHub.

最近,Dr.Axel Rauschmayer 写了一篇关于在webpack2中设置tree shaking的文章. 我决定在我同事Marc Garreau的Redux 的入门套件中试一试, 看看在一个完整项目中使用tree shaking的情况. 你可以在我的Github中看到我的实验结果.

How Does It Work?

### 它是如何工作的? ###
The steps involved in tree shaking are fairly simple.
tree shaking的运行步骤相当的简单.
You write your ES6 code as normal, importing and exporting modules as needed. When it comes time to create a bundle, Webpack grabs all of your modules and puts them into a single file, but removes the export from code that’s not being imported anywhere. Next, you run a minification process, resulting in a bundle that excludes any dead code found along the way.
If you’re curious, check Dr. Rauschmayer’s post for more details.
你跟以前一样正常的书写ES6代码,需要import和export模块. 当需要去构建打包的时候,webpack抓取你的模块放入一个单一文件中,但是移除那些你代码中export出去但是并没有在任何地方import的模块.然后你运行一个分解进程,会在打包中去除未使用的代码. 如果您好奇，请查看Dr. Rauschmayer博士的更多信息。
Setting It Up
###如何设置###
Since Webpack 2 is still in beta, you’ll need to update your package.json to point at the beta version. But before we do that let’s also talk about our Babel preset.
因为webpack2还是beta版本,你需要升级你的package.json去制定beta版本. 但是在之前我们还是要去探讨一下babel配置.
Typically, I use the es2015 preset, but this preset relies on the transform-es2015-modules-commonjs plugin, which won’t work for tree shaking. Dr. Rauschmayer pointed this out in his post. At the time of his writing, the best workaround was to copy and paste all of the plugins in that preset except transform-es2015-modules-commonjs.
通常,我使用es2015设置,但是这个设置依赖 transform-es2015-modules-commonjs插件,它是无法再free shaking下工作的.Rauschmayer在他的帖子中指出了这一点。在撰写本文时，最好的解决方法是复制并粘贴该预设中的所有插件，除了transform-es2015-modules-commonjs。
Thankfully, we can now get around this copy-and-pasting by including the es2015-native-modules or es2015-webpack preset instead. Both of these presets support native ES6 modules.
幸运的是,我们现在可以通过包括es2015-native-modules 和 es2015-webpack 设置来绕开复制粘贴这种方式.
So, let’s install Webpack 2 and the es2015-native-modules Babel preset by running npm install --save babel-preset-es2015-native-modules webpack@2.0.1-beta.

You should see these packages appear in your package.json dependencies section:
"dependencies": {
  "babel-preset-es2015-native-modules": "^6.6.0",
  "webpack": "^2.0.1-beta"
  // etc.
}

You’ll also need to update your .babelrc file to use the new preset:
{
  presets: ["es2015-native-modules", "react"]
}

Remember that we need to run a minification step during our bundle in order to take advantage of dead code elimination.
Change the build script to use the --optimize-minimize flag on our call to the Webpack executable:
// package.json
...
  "scripts": {
    "build": "webpack --optimize-minimize",
    // etc.
  },
...
Finally, update your Webpack config to make sure we’re only listing one loader at a time. This is for compatibility with Webpack 2, since it expects a slightly different syntax in the config.
// webpack.config.js
...
loaders: [
  {
    test: /\.jsx?$/,
    exclude: /node_modules/,
    loader: 'babel',
    include: __dirname
  },
]
Taking It For A Spin
Now we have everything we need in place to run a build with tree shaking.
Let’s try it out!
To make sure everything is working, we’ll need to export some modules that we’re not importing. In my example, I decided to create some useless functions:
// place-order.js

export function makeMeASandwich() {
  return 'make sandwich: operation not permitted';
}

export function sudoMakeMeASandwich() {
  return 'one open faced club sandwich coming right up';
}

Then in my actual project code, I only imported and used one of them.
// index.js

import { sudoMakeMeASandwich } from './place-order.js';
sudoMakeMeASandwich();

When we run npm run build, we should end up with a minified build that excludes the makeMeASandwich code.
When you run this command, you’ll see an output that accounts for removed modules. In my example, I saw a bunch of warnings from dependencies such as React and Webpack Hot Middleware.
I left a few of them in the pasted output below, but the line that’s of most interest of us is Dropping unused function makeMeASandwich, since that’s the code we’re checking on.
WARNING in bundle.js from UglifyJs
...
Dropping unused function makeMeASandwich [./src/place-order.js:1,16]
...
Dropping unused variable DOCUMENT_FRAGMENT_NODE_TYPE [./~/react/lib/ReactEventListener.js:26,0]
Condition always true [./~/style-loader!./~/css-loader!./~/sass-loader!./src/assets/stylesheets/base.scss:10,0]
Condition always false [(webpack)-hot-middleware/process-update.js:9,0]
Dropping unreachable code [(webpack)-hot-middleware/process-update.js:10,0]
Now if we open up the minified bundle and search for the contents of the makeMeASandwich function, we won’t be able to find it. Indeed, searching for make sandwich: operation not permitted yields no results. It works for one open faced club sandwich coming right up, though.
Success!
Going Further: Shaking Dependencies
Removing your own unused code is all well and good, but it’s probably only going to be a drop in the bucket of your overall bundle. The place where tree shaking really shines is in helping remove unused code from dependencies.
If you’re pulling in a whole package, but only using a single export from it, why would you send the rest of the package to your users?
Roman Luitikov’s post did a great job of walking through what it looks like to tree shake a dependency by showing what happens if you pull in Lodash, but only import and use the first function. The rest of Lodash gets thrown out, and the resulting build is much smaller!
Since Roman has already covered this, I won’t go into the details of what that looks like here, but you can see it in my example if you’re curious.
The main point to understand is that when you consider the sizeable amount of unused code that ends up in a typical JS project when you run npm install, removing it with tree shaking can yield huge savings on bundle size.
Run your next project on Engine Yard START FREE TRIAL 
Conclusion
Being a front-end JavaScript developer comes with a specific set of concerns. Because you’re sending a large amount of code execution off onto your user’s browser, the size of your payload can get really large. We do our users a huge service by being sensitive to the amount of data that we’re sending them.
Tree shaking offers a great way to cut down on your bundle size, and it’s easy to get set up in your existing project. I’m excited to see how this practice evolves as more of the community begins to embrace it and Webpack 2 is released.
In the meantime, I would recommend reading up on some other examples of tree shaking and more of the features coming in Webpack 2.
Until next time, happy coding!
P. S. If you decide to give tree shaking a try in your existing project, we’d love to hear how it goes. Leave us a comment!
