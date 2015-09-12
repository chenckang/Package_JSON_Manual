# npm package.json规范

### 概述

这个文档是所有package.json中的内容。package.json中的内容必须是JSON格式的，而不是JS的对象字面量。

这个文档所描述的部分行为受npm-config的配置项影响

#### name

package.json中最重要的部分就是 ***nane*** 及 ***version*** 域。这二者实际上是必须项。***name***和***version***一起形成了一个独一无二的标识。

名字就是所发布的应用的名称。

满足的条件：

* 名字必须小于214个字符
* 不能以点和下划线开头
* 名字中不能有大写字母
* 不能包含任何非URL安全（non-URL-safe）的字符

一些建议：
* 不要和核心的node模块同名
* 不要在名字中包含‘js’以及‘node’。当你编写package.json时，其设定就是js。并且你可以在***engine***域中标注引擎
* 名字可能会作为参数传递给require()，所以名字应该简短而木有描述性
* 可以到npm registry中查询 ***name*** 是否已经被占用

***name*** 之前可以选择性的加入前缀，例如， @myorg/package。更多信息请查询npm-scope。

#### version

package.json中最重要的部分就是 ***nane*** 及 ***version*** 域。这二者实际上是必须项。***name***和***version***一起形成了一个独一无二的标识。

***version*** 必须可以被[node-semver](https://github.com/npm/node-semver "node-semver github")所解析，node-semver也是npm的一个依赖。

更多信息请参阅semver

#### description

***description*** 可以帮助人们发现你所发布的内容。

#### keywords

***keywords*** 是一个字符串数组，可以帮助人们通过npm search找到你的发布内容

#### homepage

链接到你得项目主页的链接地址

> 这个和 ***url*** 域并不相同。如果你加上***url***域， registry会认为这是一个重定向到你发布包的地址

#### bugs

指定到你的项目的问题追踪页面，也可以是反馈问题的邮件地址。这对于那些在使用发布包的过程中遇到问题的人会很有帮助。

如下所示：

    {
        "url": "http://github.com/",
        "email": "p@p.com"
    }

你可以设定一个或者两个值，如果你只想提供一个地址，那么你不需要像上面那样使用一个对象，直接提供一个字符串的值就好。

#### license

你应该为你的项目指定一个license，那样人们就能够知道你所发布的包的许可范围，包括允许的授权和限制的授权。

#### people域： author, contributors

***author*** 是一个人而 ***contributors*** 是多个人。每个人都是一个有name以及可选的url和email属性的对象，如下：

    {
        "name": "xxxx",
        "email": "xxxx",
        "url": "xxxx"
    }

也可以用字符串的形式来表示：

    "Ruby Robe <ruberobe@xxx.com> (http://rubyrobe.com/)"

#### files

***files*** 是你项目中的一个文件数组，如果你将文件夹的名称也包含在内，那么文件夹下面所包含的文件也会被包含进来。

可以在项目的更目录增加***.npmigore***文件来讲文件排除在外。其工作方式如同***.gitigonore***一样。

一些文件无路如何设置都默认被包含：

* package.json
* README
* CHANGELOG
* LICENSE/LICENCE

相反的，一些文件将会被忽略：

* .git
* CSV
* .svn
* .hg
* .lock-wscript
* wafpickle-N
* *.swp
* .DS_Store
* _*
* npm-debug.log

#### main

***main***域就是你的程序的主入口。也就是，如果你的包被命名为foo，并且用户装载了它且执行了***require('foo')***，那么你么主模块将会被返回。

这应该是相对于包的根目录的一个模块id

对于大多数模块， 有一个main脚本是十分必要的，并且也仅需要一个main脚本。

#### bin

指定执行文件的所在地（后续补充）

#### man

指定要么一个文件要么多个文件提供给***man***来查找。

#### directories

CommonJS的包规范中指定了一些方式来表明你所发布的包的结构，使用dirctories对象来达成这种目的。如果你看下[npm's package.json](https://registry.npmjs.org/npm/latest)，你会发现存在doc，lib及man目录。

未来，这些信息将会以其他富有创意的方式被使用。

##### directories.lib

告诉人们library文件的所在地

##### directories.bin

##### directories.man

指定man pages的所属地。

##### directroies.doc

将markdown文件放在这个目录下

##### directories.example

将示例脚本放在这里

#### repository

指定项目的源代码躲在地

    "repository": {
        "type": "git",
        "url": "https://github.com/npm/npm.git"
    }

#### scripts

待补充

#### config

待补充

#### dependencies

***dependencies***将作为一个简单的对象来映射到一个包名称及一个版本范围。

semver

* version 必须完全匹配version
* >version 必须大于version
* >=version 必须大于等于version
* <version 必须小于version
* <=version 必须小于等于version
* ~version 接近于等于version
* ^version 和version兼容
* 1.2.x 可以为1.2.1，1.2.9但是不能为1.3.0
* * 匹配任何
* "" 同*
* version1 - version2 同>=version1 <=version2
* range1 || range2 range1和range2只需要一个满足
* git
* local paths

例如

    { "dependencies" :
        { 
            "foo" : "1.0.0 - 2.9999.9999"
            , "bar" : ">=1.0.2 <2.1.2"
            , "baz" : ">1.0.2 <=2.3.4"
            , "boo" : "2.0.1"
            , "qux" : "<1.0.0 || >=2.3.1 <2.4.5 || >=2.5.2 <3.0.0"
            , "asd" : "http://asdf.com/asdf.tar.gz"
            , "til" : "~1.2"
            , "elf" : "~1.2.3"
            , "two" : "2.x"
            , "thr" : "3.3.x"
            , "lat" : "latest"
            , "dyl" : "file:../dyl"
        }
    }

#### devDependencies

如果某个开发者只是计划使用你的程序，但是不不需要去构建或者包含你的应用或者代码，也就是对方的项目中并不需要引入你的源代码，而是作为在其开发中的一个工具来辅助开发。这样的依赖可以放入***devDependencies***中。

#### pearDependencies

所引入的项目和你的工程有极其共同的接口和兼容性。例如，你的项目实现了另一个工程的接口

#### bundledDependencies

#### optionalDependencies

如果一个依赖可以被使用，但是你又想在npm无法找到或者不能现在该依赖的情况下，继续去处理，那么你就可以间这些放入optionalDependencies中。

#### engines

可以指定应用运行在的node版本

#### engineStrict

将***engineStrict***设置为true，那么你得应用将只能运行在***engines***所定义的域之上。

#### os

指定所运行的操作系统

    "os": ["linux", "!win32"]

如上，应用在linux上运行，不能再win32上运行

#### cpu

指定所运行的cpu架构

    "cpu": ["x64", "!amd"]

#### preferGlobal

待补充

#### private

待补充

#### publishConfig

待补充



### Default Values

一些属性的默认值

* "scripts": {"start": "node server.js"}

如果项目的根目录下存在server.js文件，那么运行npm run start将会运行server.js

* "script": {"preinstall": "node-gyp rebuild"}

如果项目的根目录存在build.gyp文件，那么npm默认会预先运行preinstall命令

* "contributors": [...]

如果项目的更目录中存在AUTHORS文件，npm将会对文件的每行解析为
    
    Name <email> (url)