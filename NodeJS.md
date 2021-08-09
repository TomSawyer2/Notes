## NodeJS

### 简介

NodeJS是一个跨平台的js运行环境。

在浏览器外NodeJS运行V8引擎。

Hello World见Hello World.js。

### 历史

2009出现。

### 浏览器与NodeJS的区别

1. 浏览器提供DOM以及BOM操作，但NodeJS没有（其实也不需要）。
2. 浏览器无法对文件进行操作但NodeJS可以。
3. 在NodeJS中使用require()而在浏览器中使用import。

### V8引擎

使用C++编写，可跨系统运行。

js通常意义上是解释性语言，但现代引擎也会对其进行编译。

js在V8内部进行编译，使用即时编译加快执行速度。

### 运行

`node Hello World.js`

### 退出

1. Ctrl+C

2. `process.exit()`进程被强制中止，可以在括号内部填写退出码，也可以通过`process.exitCode = 1`来设置exit时的返回码为1。

3. 若NodeJS内部起了个服务器则应该采用如下方式中止进程：

   ```js
   process.on('SIGTERM', () => {
     server.close(() => {
       console.log('进程已终止')
     })
   })
   ```

### 读取环境变量

`process.env.NODE_ENV`

### NodeJS REPL

输入`node`进入REPL模式。

REPL模式相当于python，可以运行js的表达式，也可以用于搜索js对象等等。

#### 点命令

- `.help`: 显示点命令的帮助。
- `.editor`: 启用编辑器模式，可以轻松地编写多行 JavaScript 代码。当处于此模式时，按下 ctrl-D 可以运行编写的代码。
- `.break`: 当输入多行的表达式时，输入 `.break` 命令可以中止进一步的输入。相当于按下 ctrl-C。
- `.clear`: 将 REPL 上下文重置为空对象，并清除当前正在输入的任何多行的表达式。
- `.load`: 加载 JavaScript 文件（相对于当前工作目录）。
- `.save`: 将在 REPL 会话中输入的所有内容保存到文件（需指定文件名）。
- `.exit`: 退出 REPL（相当于按下两次 ctrl-C）。

当然如果REPL识别出了输入的是多行代码则不需要使用`.editor`，REPL会在最前面显示...来表示继续输入。

### 从命令行接收参数

使用命令调用js时可以传入任意数量的参数，参数可以是独立的，也可以有键和值。

```bash
node app.js
node app.js tom
node app.js name=tom
```

`process.argv`包含所有命令行调用的参数，是一个数组。

第一个参数是node命令的完整路径。

第二个参数是正在被执行文件的完整路径。

其余参数从第三个开始。

*以上部分跟C语言几乎一模一样。*

第二种方式传入的值可以直接通过`process.argv[2]`获得。

第三种方式传入的值需要通过minimist库处理：

```js
const args = require('minimist')(process.argv.slice(2))
args['name'] //tom
```

但是必须在参数名前面用双破折号：`node app.js --name=tom`

### 输出到命令行

可以直接console.log输出到命令行，当然也可以格式化字符串：

```js
console.log('格式化%s字符串有%d个字符', '的', 50)
```

- `%s` 会格式化变量为字符串
- `%d` 会格式化变量为数字
- `%i` 会格式化变量为其整数部分
- `%o` 会格式化变量为对象

`console.clear()`可以清空控制台。

`console.count()`可用于统计数量：

```js
const oranges = ['橙子', '橙子']
const apples = ['苹果']
oranges.forEach(fruit => {
  console.count(fruit)
})
apples.forEach(fruit => {
  console.count(fruit)
})
```

输出：

```bash
橙子: 1
橙子: 2
苹果: 1
```

`console.trace()`可以用于打印堆栈踪迹。

`console.time()`与`console.timeEnd()`配合使用可以用于计算函数运行所需事件。

```js
const doSomething = () => console.log('测试')
const measureDoingSomething = () => {
  console.time('doSomething()')
  //做点事，并测量所需的时间。
  doSomething()
  console.timeEnd('doSomething()')
}
measureDoingSomething()
```

#### stdout和stderr

`console.log`打印出的信息是标准输出，也就是stdout。

`console.error`打印出的信息是stderr，会出现在错误日志中。

#### 改变输出的颜色

底层方法：`console.log('\x1b[33m%s\x1b[0m', '你好')`

使用库：Chalk。

1. 安装：`npm install chalk`

2. 使用：

   ```js
   const chalk = require('chalk')
   console.log(chalk.yellow('Hello World in yellow!'))
   ```

#### 进度条

1. 安装：`npm install progress`

2. 使用：

   ```js
   const ProgressBar = require('progress')
   
   const bar = new ProgressBar(':bar', { total: 10 })
   const timer = setInterval(() => {
     bar.tick()
     if (bar.complete) {
       clearInterval(timer)
     }
   }, 100)
   ```

   total表示进度条一共有几步，100表示多少毫秒完成一步，但是只能匀速走。

   *进度条长得有点挫*

### 从命令行接收输入

readline模块可以每次一行地从可读流读入。

```javascript
const readline = require('readline').createInterface({
  input: process.stdin,
  output: process.stdout
})

readline.question(`你叫什么名字?`, name => {
  console.log(`你好 ${name}!`)
  readline.close()
})
```

qustion()方法显示第一个参数并等待输入，类似于scanf，回车后调用回调函数输出并关闭接口。

inquirer.js提供更多输入的选择。

1. 安装：`npm install inquirer`

2. 使用：

   ```javascript
   const inquirer = require('inquirer')
   
   var questions = [
     {
       type: 'input',
       name: 'name',
       message: "你叫什么名字?"
     }
   ]
   
   inquirer.prompt(questions).then(answers => {
     console.log(`你好 ${answers['name']}!`)
   })
   ```

   这个包还可以实现多选、展示单选按钮、确认等等操作。

### 使用export暴露模块

导入模块：`const lib = require('./lib')`

但前提是lib.js将功能公开了。

方法一：`module.exports = lib`

方法二：将要导出的对象添加为exports的属性，这样就可以导出多个数据。

```js
const car = {
  brand: 'Ford',
  model: 'Fiesta'
}

exports.car = car

//等同于

exports.car = {
  brand: 'Ford',
  model: 'Fiesta'
}
```

在另一个文件中require进来后.car即可访问。

module.export与export的区别：前者公开了它指向的对象。 后者公开了它指向的对象的属性。

### npm包管理器

npm仓库：[npm (npmjs.com)](https://www.npmjs.com/)

打包上传方式：[(2条消息) 打包发布自己的nodejs包_gfdfhjj的博客-CSDN博客_nodejs 打包](https://blog.csdn.net/gfdfhjj/article/details/83892151)

若项目有package.json则运行`npm install`可以直接安装项目所需的所有包。

`npm install xxx`安装特定的xxx包，会安装到项目所在文件夹并且在package.json中的dependencies属性中添加xxx包。

- `--save` 安装并添加条目到 `package.json` 文件的 dependencies。
- `--save-dev` 安装并添加条目到 `package.json` 文件的 devDependencies。
- `-g`执行全局安装，可以通过`npm root -g`找到位置。

`devDependencies` 通常是开发的工具（例如测试的库），而 `dependencies` 则是与生产环境中的应用程序相关。

`npm update`自动更新所有包，也可指定更新某一个包。

`npm run xxx`需要配合package.json中scripts的内容使用，可以简化打包的过程。

### 使用/执行npm安装的包

js库只需要直接require进来：`const lodash = require('lodash')`

可执行文件一般会被放在node_modules/.bin/文件夹下，包含指向其二进制文件的符号链接，有两种执行方式（以cowsay为例）：

1. 输入`./node_modules/.bin/cowsay`抄家执行。
2. `npx cowsay`直接执行。

### package.json

该文件是项目的清单，是工具的配置中心，也是 `npm` 和 `yarn` 存储所有已安装软件包的名称和版本的地方。

#### 文件结构

一般情况下package.json只要符合json格式即可。

空的package.json：`{}`

#### 属性分类

- name：设置软件包的名称。必须少于 214 个字符，且不能包含空格，只能包含小写字母、连字符（`-`）或下划线（`_`）。
- author：软件包的作者名称。
- contributors：数组，用于列出贡献者。
- bugs：链接到软件包的问题跟踪器，issues页面常用。
- homepage：设置软件包的主页。
- version：指定软件包的当前版本。第一个数字是主版本号，第二个数字是次版本号，第三个数字是补丁版本号。仅修复缺陷的版本是补丁版本，引入向后兼容的更改的版本是次版本，具有重大更改的是主版本。
- license：指定软件包的许可证。
- keywords：包含关键字的数组。
- description：包含对软件包的简短描述。
- repository：指定此程序包仓库所在的位置。
- main：设置软件包的入口点，在应用程序中导入此软件包时，应用程序会在该位置搜索模块的导出。
- private：若为true则可防止被发布到npm。
- scripts：可以定义一组可运行的node脚本。
- dependencies：设置作为依赖安装的npm软件包的列表，npm install或者yarn add的时候会自动插入列表。
- devDependencies：设置作为开发依赖安装的软件包列表，这些包只需要安装在开发环境而无需安装在生产环境。对应npm install的--save-dev和yarn add的--dev。
- engines：设置此软件包/应用程序要运行的NodeJS或者其他命令的版本。
- browserslist：用于告知要支持哪些浏览器（及其版本）。
- 特有属性：Babel、ESLint会需要。

#### 软件包版本

使用语义版本控制（semver）。

- 如果写入的是 `〜0.13.0`，则只更新补丁版本：即 `0.13.1` 可以，但 `0.14.0` 不可以。
- 如果写入的是 `^0.13.0`，则要更新补丁版本和次版本：即 `0.13.1`、`0.14.0`、依此类推。
- 如果写入的是 `0.13.0`，则始终使用确切的版本。

### package-lock.json

旨在跟踪被安装的每个软件包的确切版本，以便产品可以以相同的方式被 100％ 复制。

### 查看npm包安装的版本

获取所有的包：`npm list`或者`npm list -g`

获取顶层的软件包：`npm list --depth=0`

获取指定名称的包：`npm list xxx`

查看软件在npm仓库上最新的版本：`npm view xxx version`

### 安装旧版本的npm包

`npm install xxx@1.0.0`会安装xxx的1.0.0版本。

### 将依赖包更新到最新版本

`npm update`不会更新到主版本。

若想更新到主版本则应：

1. 安装npm-check-updates包：`npm install -g npm-check-updates`
2. 运行：`ncu -u`

这样会升级package.json中的dependencies和devDependencies中的所有版本以便npm可以安装新的主版本。

3. `npm update`

### 使用npm语义版本控制

- 第一个数字是主版本。
- 第二个数字是次版本。
- 第三个数字是补丁版本。



- 当进行不兼容的 API 更改时，则升级主版本。
- 当以向后兼容的方式添加功能时，则升级次版本。
- 当进行向后兼容的缺陷修复时，则升级补丁版本。



- `^`: 只会执行不更改最左边非零数字的更新。 如果写入的是 `^0.13.0`，则当运行 `npm update` 时，可以更新到 `0.13.1`、`0.13.2` 等，但不能更新到 `0.14.0` 或更高版本。 如果写入的是 `^1.13.0`，则当运行 `npm update` 时，可以更新到 `1.13.1`、`1.14.0` 等，但不能更新到 `2.0.0` 或更高版本。
- `~`: 如果写入的是 `〜0.13.0`，则当运行 `npm update` 时，会更新到补丁版本：即 `0.13.1` 可以，但 `0.14.0` 不可以。
- `>`: 接受高于指定版本的任何版本。
- `>=`: 接受等于或高于指定版本的任何版本。
- `<=`: 接受等于或低于指定版本的任何版本。
- `<`: 接受低于指定版本的任何版本。
- `=`: 接受确切的版本。
- `-`: 接受一定范围的版本。例如：`2.1.0 - 2.6.2`。
- `||`: 组合集合。例如 `< 2.1 || > 2.6`。
- 无符号: 仅接受指定的特定版本（例如 `1.2.1`）。
- `latest`: 使用可用的最新版本。

### 卸载包

`npm uninstall xxx`卸载xxx包。

使用-S或者--save移除在package.json文件中的引用。

若在devDependencies中则使用-D或者--save-dev移除。

若是全局安装的则使用-g或者--global移除。

### 依赖与开发依赖

生产环境使用时需要`npm install --production`避免安装开发依赖。

### 包运行器npx

npx可以自动搜索本地安装的命令并且运行。

`npx node@10 -v`npx可以使用@指定版本并将其与node结合使用。

npx还可以直接运行github上gist的代码。

### 事件循环

#### 调用堆栈

事件循环会不断检查调用堆栈，查看是否有要运行的函数。

#### 消息队列

在消息队列中，用户触发的事件（如单击或键盘事件、或获取响应）也会在此排队，然后代码才有机会对其作出反应。 

事件循环会赋予调用堆栈优先级，它首先处理在调用堆栈中找到的所有东西，一旦其中没有任何东西，便开始处理消息队列中的东西。

#### 作业队列

这种方式会尽快地执行异步函数的结果，而不是放在调用堆栈的末尾。在当前函数结束之前 resolve 的 Promise 会在当前函数之后被立即执行。

### process.nextTick()

一次完整的事件循环即为一个tick。

在下一个tick开始前调用函数：

```js
process.nextTick(() => {
    //下一个tick前干的事情
})
```

这种方式相当于插队执行函数。

### setImmediate()

在下一个tick中调用函数：

```js
setImmediate(() => {
  //运行一些东西
})
```

### JS定时器

#### setTimeout()

提供回调函数以及延迟运行的时间：

```javascript
setTimeout(() => {
  // 2 秒之后运行
}, 2000)

setTimeout(() => {
  // 50 毫秒之后运行
}, 50)
```

也可以传入其他参数：

```javascript
const myFunction = (firstParam, secondParam) => {
  // 做些事情
}

// 2 秒之后运行
setTimeout(myFunction, 2000, firstParam, secondParam)
```

setTimeout()会返回定时器的id，可以用于清除该定时器。

#### 零延迟

将延迟时间设为0，回调函数会尽快执行，但是在当前函数执行之后。

#### setInterval()

会在指定的特定时间间隔（以毫秒为单位）一直地运行回调函数，而不是只运行一次。

```javascript
setInterval(() => {
  // 每 2 秒运行一次
}, 2000)
```

需要用clearInterval(id)来停止循环，所以会放在定时器内部：

```javascript
const interval = setInterval(() => {
  if (App.somethingIWait === 'arrived') {
    clearInterval(interval)
    return
  }
  // 否则做些事情
}, 100)
```

#### 递归的setTimeout()

setInterval()每n毫秒启动一次函数，但不会考虑函数执行的时间，因此若函数执行时间不一会导致重叠执行等等问题。以下方案可以使函数均匀执行：

```javascript
const myFunction = () => {
  // 做些事情

  setTimeout(myFunction, 1000)
}

setTimeout(myFunction, 1000)
```

### 异步编程与回调

程序是异步的且会暂停执行直到需要关注，这使得计算机可以同时执行其他操作。 当程序正在等待来自网络的响应时，则它无法在请求完成之前停止处理器。JS默认情况下是同步、单线程的。

以按钮为例，在写代码的时候不知道这个按钮什么时候会被触发，因此用一个回调函数来存放触发时要执行的函数。

#### 处理回调的错误

任何回调函数中的第一个参数为错误对象（即错误优先的回调）。

```javascript
fs.readFile('/文件.json', (err, data) => {
  if (err !== null) {
    //处理错误
    console.log(err)
    return
  }

  //没有错误，则处理数据。
  console.log(data)
})
```

#### 复杂回调

使用Promise/Async/Await。

### Promise

最终会变为可用值的代理，可用于处理异步代码。

#### 创建Promise

```javascript
let done = true

const isItDoneYet = new Promise((resolve, reject) => {
  if (done) {
    const workDone = '这是创建的东西'
    resolve(workDone)
  } else {
    const why = '仍然在处理其他事情'
    reject(why)
  }
})
```

#### 消费Promise

```javascript
const isItDoneYet = new Promise(/* ... 如上所述 ... */)
//...

const checkIfItsDone = () => {
  isItDoneYet
    .then(ok => {
      console.log(ok)
    })
    .catch(err => {
      console.error(err)
    })
}
```

#### 错误处理

发生错误时会执行最近的catch()语句。

#### 级联错误

catch()内部的错误也可以由下一个catch()处理。

#### 编排Promise

Promise.all()可以用于定义Promise列表并且在所有Promise解决后执行操作。

```javascript
const f1 = fetch('/something.json')
const f2 = fetch('/something2.json')

Promise.all([f1, f2])
  .then(res => {
    console.log('结果的数组', res)
  })
  .catch(err => {
    console.error(err)
  })
```

Promise.race()可以在第一个传入的Promise解决后执行。

```javascript
const first = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, '第一个')
})
const second = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, '第二个')
})

Promise.race([first, second]).then(result => {
  console.log(result) // 第二个
})
```

#### 错误

Uncaught TypeError: undefined is not a promise

使用 `new Promise()` 而不是 `Promise()`。

UnhandledPromiseRejectionWarning

在 `then` 之后添加 `catch` 。

### Async/Await

在任何函数之前加上 `async` 关键字意味着该函数会返回 promise。

只需要在耗时操作前面await即可实现Promise的.then的功能，会使代码看起来非常简洁并且使调试变得非常简单。

### 事件触发器

NodeJS提供events模块用于触发事件，此模块提供了EventEmitter类。

初始化：

```javascript
const EventEmitter = require('events')
const eventEmitter = new EventEmitter()
```

- `emit` 用于触发事件。
- `on` 用于添加回调函数（会在事件被触发时执行）。

```javascript
eventEmitter.on('start', () => {
  console.log('开始')
})
```

当运行以下代码时：

```javascript
eventEmitter.emit('start')
```

事件处理函数会被触发，且获得控制台日志。

也可以加入额外的参数。

- `once()`: 添加单次监听器。
- `removeListener()` / `off()`: 从事件中移除事件监听器。
- `removeAllListeners()`: 移除事件的所有监听器。

### HTTP服务器

示例：

```javascript
const http = require('http')

const port = 3000

const server = http.createServer((req, res) => {
  res.statusCode = 200
  res.setHeader('Content-Type', 'text/plain')
  res.end('你好世界\n')
})

server.listen(port, () => {
  console.log(`服务器运行在 http://${hostname}:${port}/`)
})
```

### 发送HTTP请求

#### GET

```javascript
const https = require('https')
const options = {
  hostname: 'nodejs.cn',
  port: 443,
  path: '/todos',
  method: 'GET'
}

const req = https.request(options, res => {
  console.log(`状态码: ${res.statusCode}`)

  res.on('data', d => {
    process.stdout.write(d)
  })
})

req.on('error', error => {
  console.error(error)
})

req.end()
```

#### POST

标准模块

```javascript
const https = require('https')

const data = JSON.stringify({
  todo: '做点事情'
})

const options = {
  hostname: 'nodejs.cn',
  port: 443,
  path: '/todos',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Content-Length': data.length
  }
}

const req = https.request(options, res => {
  console.log(`状态码: ${res.statusCode}`)

  res.on('data', d => {
    process.stdout.write(d)
  })
})

req.on('error', error => {
  console.error(error)
})

req.write(data)
req.end()
```

axios

```javascript
const axios = require('axios')

axios
  .post('http://nodejs.cn/todos', {
    todo: '做点事情'
  })
  .then(res => {
    console.log(`状态码: ${res.statusCode}`)
    console.log(res)
  })
  .catch(error => {
    console.error(error)
  })
```

#### PUT/DELETE

与POST类似，只需更改请求方式。

### 获取请求正文数据

express

```javascript
const express = require('express')
const app = express()

app.use(
  express.urlencoded({
    extended: true
  })
)

app.use(express.json())

app.post('/todos', (req, res) => {
  console.log(req.body.todo)
})
```

标准模块

```javascript
const server = http.createServer((req, res) => {
  let data = '';
  req.on('data', chunk => {
    data += chunk;
  })
  req.on('end', () => {
    JSON.parse(data).todo // '做点事情'
  })
})
```

### 使用文件描述符

文件描述符是使用 `fs` 模块提供的 `open()` 方法打开文件后返回的：

```javascript
const fs = require('fs')

fs.open('/Users/joe/test.txt', 'r', (err, fd) => {
  //fd 是文件描述符。
})
```

- `r+` 打开文件用于读写。
- `w+` 打开文件用于读写，将流定位到文件的开头。如果文件不存在则创建文件。
- `a` 打开文件用于写入，将流定位到文件的末尾。如果文件不存在则创建文件。
- `a+` 打开文件用于读写，将流定位到文件的末尾。如果文件不存在则创建文件。

`fs.openSync`方法可以用于打开文件并直接返回文件描述符。

### 文件属性

- 使用 `stats.isFile()` 和 `stats.isDirectory()` 判断文件是否目录或文件。
- 使用 `stats.isSymbolicLink()` 判断文件是否符号链接。
- 使用 `stats.size` 获取文件的大小（以字节为单位）。

### 文件路径

Linux、MacOS以及Win的文件路径表述方式不同，因此需要进行区分。

```javascript
const path = require('path')
```

- `dirname`: 获取文件的父文件夹。
- `basename`: 获取文件名部分。
- `extname`: 获取文件的扩展名。

示例：

```javascript
const notes = '/users/joe/notes.txt'

path.dirname(notes) // /users/joe
path.basename(notes) // notes.txt
path.extname(notes) // .txt
```

通过为 `basename` 指定第二个参数来获取不带扩展名的文件名：

```javascript
path.basename(notes, path.extname(notes)) //notes
```

使用 `path.join()` 连接路径的两个或多个片段：

```javascript
const name = 'joe'
path.join('/', 'users', name, 'notes.txt') //'/users/joe/notes.txt'
```

使用 `path.resolve()` 获得相对路径的绝对路径计算：

```javascript
path.resolve('joe.txt') //'/Users/joe/joe.txt' 如果从主文件夹运行。
```

将 `/joe.txt` 附加到当前工作目录。 如果指定第二个文件夹参数，则 `resolve` 会使用第一个作为第二个的基础：

```javascript
path.resolve('tmp', 'joe.txt') //'/Users/joe/tmp/joe.txt' 如果从主文件夹运行。
```

如果第一个参数以斜杠开头，则表示它是绝对路径：

```javascript
path.resolve('/etc', 'joe.txt') //'/etc/joe.txt'
```

`path.normalize()`可用于计算相对路径的绝对路径。

### 读取文件

fs.readFile()

```javascript
const fs = require('fs')

fs.readFile('/Users/joe/test.txt', 'utf8' , (err, data) => {
  if (err) {
    console.error(err)
    return
  }
  console.log(data)
})
```

fs.readFileSync()

```javascript
const fs = require('fs')

try {
  const data = fs.readFileSync('/Users/joe/test.txt', 'utf8')
  console.log(data)
} catch (err) {
  console.error(err)
}
```

### 写入文件

fs.writeFile()

```javascript
const fs = require('fs')

const content = '一些内容'

fs.writeFile('/Users/joe/test.txt', content, err => {
  if (err) {
    console.error(err)
    return
  }
  //文件写入成功。
})
```

fs.writeFileSync()

```javascript
const fs = require('fs')

const content = '一些内容'

try {
  const data = fs.writeFileSync('/Users/joe/test.txt', content, { flag: 'a+' })
  //文件写入成功。
} catch (err) {
  console.error(err)
}
```

注意：这两种方式写入文件会替换文件的内容。

- `r+` 打开文件用于读写。
- `w+` 打开文件用于读写，将流定位到文件的开头。如果文件不存在则创建文件。
- `a` 打开文件用于写入，将流定位到文件的末尾。如果文件不存在则创建文件。
- `a+` 打开文件用于读写，将流定位到文件的末尾。如果文件不存在则创建文件。

fs.appendFile()

fs.appendFileSync()

```javascript
const content = '一些内容'

fs.appendFile('file.log', content, err => {
  if (err) {
    console.error(err)
    return
  }
  //完成！
})
```

注意：这两种方式是追加内容。

### 使用文件夹

`fs.access()` 检查文件夹是否存在以及 Node.js 是否具有访问权限。

 `fs.mkdir()` 或 `fs.mkdirSync()` 创建文件夹。

 `fs.readdir()` 或 `fs.readdirSync()` 读取目录的内容并返回所有文件、子文件夹的相对路径。

#### 重命名文件夹

使用 `fs.rename()` 或 `fs.renameSync()` 。 第一个参数是当前的路径，第二个参数是新的路径。

#### 删除文件夹

使用 `fs.rmdir()` 或 `fs.rmdirSync()` 。

若文件夹有内容，则需要：

1. 安装：`npm install fs-extra`

2. ```javascript
   fs.remove(folder)
     .then(() => {
       //完成
     })
     .catch(err => {
       console.error(err)
     })
   ```

### 文件系统模块

fs的所有方法默认是异步的，但加上Sync也可以变为同步的。

### 路径模块

`path.basename()`返回路径的最后一部分。第二个参数可以过滤拓展名。

`path.dirname()`返回路径的目录。

`path.extname()`返回路径的拓展名。

`path.isAbsolute()`判断是否为绝对路径。

`path.join()`连接路径的多个部分。

`path.normalize()`计算绝对路径。

`path.parse()`解析路径为片段。

`path.relative()`接收两个参数，计算从第一个路径到第二个路径的相对路径。

`path.resolve()`通过相对路径获取绝对路径，指定第二个参数，`resolve` 会使用第一个参数作为第二个参数的基准，如果第一个参数以斜杠开头，则表示它是绝对路径。

### 系统模块

`os.arch()`系统底层架构。

`os.cpus()`系统可用CPU信息。

`os.endianness()`判断是使用大端序编译NodeJS还是小端序。

`os.freemen()`可用内存字节数。

`os.homedir()`主目录路径。

`os.hostname()`主机名。

`os.loadavg()`操作系统对平均负载的计算。

`os.networkInterfaces()`系统上可用网络接口的信息。

`os.platform()`编译NodeJS的平台。

`os.release()`操作系统版本号。

`os.tmpdir()`指定临时文件夹的路径。

`os.totalmem()`系统可用总内存。

`os.type()`操作系统标识。

`os.uptime()`计算机运行秒数。

`os.userInfo()`返回有关user的对象。

### 事件模块

`emitter.emit()`按照事件被注册的顺序同步地调用每个事件监听器。

`emitter.eventNames()`返回字符串（表示在当前 EventEmitter 对象上注册的事件）数组。

`emitter.getMaxListeners()`获取可以添加到 EventEmitter 对象的监听器的最大数量。

`emitter.listenerCount()`获取作为参数传入的事件监听器的计数。

`emitter.listeners()`获取作为参数传入的事件监听器的数组。

`emitter.off()`emitter.removeListener() 的别名。

`emitter.on()`添加当事件被触发时调用的回调函数。

`emitter.once()`添加当事件在注册之后首次被触发时调用的回调函数。

`emitter.prependListener()`当使用 on 或 addListener 添加监听器时，监听器会被添加到监听器队列中的最后一个，并且最后一个被调用。 使用 prependListener 则可以在其他监听器之前添加并调用。

`emitter.prependOnceListener()`当使用 once 添加监听器时，监听器会被添加到监听器队列中的最后一个，并且最后一个被调用。 使用 prependOnceListener 则可以在其他监听器之前添加并调用。

`emitter.removeAllListeners()`移除 EventEmitter 对象的所有监听特定事件的监听器。

`emitter.removeListener()`移除特定的监听器。 可以通过将回调函数保存到变量中（当添加时），以便以后可以引用它。

`emitter.setMaxListeners()`设置可以添加到 `EventEmitter` 对象的监听器的最大数量。

### HTTP模块

没啥好记的。

### Buffer

Buffer本质是整数数组，每个整数代表一个数据字节。

Buffer 与流紧密相连。 当流处理器接收数据的速度快于其消化的速度时，则会将数据放入 buffer 中。

### 流

在传统的方式中，当告诉程序读取文件时，这会将文件从头到尾读入内存，然后进行处理。

使用流，则可以逐个片段地读取并处理（而无需全部保存在内存中）。

`pipe()` 方法的返回值是目标流，这是非常方便的事情，它使得可以链接多个 `pipe()` 调用。

- `process.stdin` 返回连接到 stdin 的流。
- `process.stdout` 返回连接到 stdout 的流。
- `process.stderr` 返回连接到 stderr 的流。
- `fs.createReadStream()` 创建文件的可读流。
- `fs.createWriteStream()` 创建到文件的可写流。
- `net.connect()` 启动基于流的连接。
- `http.request()` 返回 http.ClientRequest 类的实例，该实例是可写流。
- `zlib.createGzip()` 使用 gzip（压缩算法）将数据压缩到流中。
- `zlib.createGunzip()` 解压缩 gzip 流。
- `zlib.createDeflate()` 使用 deflate（压缩算法）将数据压缩到流中。
- `zlib.createInflate()` 解压缩 deflate 流。

#### 流的类型

- `Readable`: 可以通过管道读取、但不能通过管道写入的流（可以接收数据，但不能向其发送数据）。 当推送数据到可读流中时，会对其进行缓冲，直到使用者开始读取数据为止。
- `Writable`: 可以通过管道写入、但不能通过管道读取的流（可以发送数据，但不能从中接收数据）。
- `Duplex`: 可以通过管道写入和读取的流，基本上相对于是可读流和可写流的组合。
- `Transform`: 类似于双工流、但其输出是其输入的转换的转换流。

### 开发环境和生产环境

生产环境：

- 日志记录保持在最低水平。
- 更高的缓存级别以优化性能。

可以使用条件语句在不同的环境中执行代码：

```javascript
if (process.env.NODE_ENV === "development") {
  //...
}
if (process.env.NODE_ENV === "production") {
  //...
}
if(['production', 'staging'].indexOf(process.env.NODE_ENV) >= 0) {
  //...
})
```

### 错误处理

使用 `throw` 关键字创建异常：

```javascript
throw value
```

常规的程序流会被停止，且控制会被交给最近的异常处理程序。

错误对象是 Error 对象的实例：

```javascript
throw new Error('错误信息')
```

异常处理程序是 `try`/`catch` 语句。

`try` 块中包含的代码行中引发的任何异常都会在相应的 `catch` 块中处理：

```javascript
try {
  //代码行
} catch (e) {}
```

在此示例中，`e` 是异常值。

监听 `process` 对象上的 `uncaughtException` 事件以处理未捕获的异常。

### 记录对象

防止因为嵌套过多NodeJS直接返回一个[Object]，有以下两种方式：

```javascript
console.log(JSON.stringify(obj, null, 2))
```

其中 `2` 是用于缩进的空格数。

```javascript
require('util').inspect.defaultOptions.depth = null
console.log(obj)
```

但第 2 级之后的嵌套对象会被展平。

### TS

梦回C。

运行：

1. `npm install typescript`
2. `tsc test.ts`
3. 运行生成的js文件。

# NodeJS基础部分学习结束

