# Packages and Modules

沉浸在一个生态系统中的一个关键步骤是学习它的词汇。Node.js和NPM对于包和模块有非常具体的定义，很容易混淆。我们将在这里讨论这些定义，使他们区分开，并解释为什么某些默认文件的命名会是这样的。

一个包是一个由package.json文件描述的文件或文件夹。这可能通过一堆不同的方式实现！欲了解更多信息，请参阅下面的"What is a package?"。
一个模块是可以通过Node.js的**require()**加载的任何文件或文件夹。同样，也有一些配置，实现这种情况。欲了解更多信息，请参阅下面的"What is a module?"。

What is a package?
一个包是以下任一：

* a）由一个package.json文件描述的含有一个程序的文件夹
* b）含有(a)的一个gzip压缩包
* c）解析向(b)一个url
* d）用(c)在注册中心上发布的一个`<name>@<version>`
* e）指向至(d)的一个`<name>@ <tag>`
* f）具有满足(e)的一个最新的标签的一个`<name>`
* g）克隆时，会产生(a)的一个**git** url 。

注意到所有这些**包**的可能性，所以，即使你从来没有把你的包发布到公共注册中心，你仍然可以得到大量的使用NPM的好处：

* 如果你只是想编写一个node程序，抑或
* 如果你想能够在压缩成一个压缩包后很容易地在别的地方安装它

Git的urls可以是以下形式：

```
git://github.com/user/project.git#commit-ish
git+ssh://user@hostname:project.git#commit-ish
git+http://user@hostname/project/blah.git#commit-ish
git+https://user@hostname/project/blah.git#commit-ish
```

**commit-ish**可以是任何标记、sha或者分支，用来作为从git checkout的参数，默认是**master**

What is a module?

一个模块是在Node.js中可以通过**require()**加载的任何东西。以下是可以作为模块加载的所有例子：

* 包含一个含有`main`域的package.json文件的文件夹。
* 有一个index.js文件的文件夹。
* 一个JavaScript文件。

**大多数npm包是模块**

通常，在Node.js的程序中使用的npm包使用**require**加载，它们就是模块。然而，没有说npm包必须是一个模块要求的！

有些包，比如：命令行包只含有可执行命令行接口，而且不为在Node.js中的使用提供一个**main**域。这些包不是模块。

If you use npm init to initialize your packages, you can pass in your scope like this:

`
npm init --scope=<your_scope>
`

If you use the same scope most of the time, you'll probably want to set it in your default configuration instead.

`
npm config set scope <your_scope>
`

shorthands cli

-v: --version
-h, -?, --help, -H: --usage
-s, --silent: --loglevel silent
-q, --quiet: --loglevel warn
-d: --loglevel info
-dd, --verbose: --loglevel verbose
-ddd: --loglevel silly
-g: --global
-C: --prefix
-l: --long
-m: --message
-p, --porcelain: --parseable
-reg: --registry
-f: --force
-desc: --description
-S: --save
-D: --save-dev
-O: --save-optional
-B: --save-bundle
-E: --save-exact
-y: --yes
-n: --yes false
ll and la commands: ls --long

You may receive an EACCES error when you try to install a package globally. This indicates that you do not have permission to write to the directories that npm uses to store global packages and commands.

You can fix this problem using one of three options:

1. Change the permission to npm's default directory.
2. Change npm's default directory to another directory.
3. Install node with a package manager that takes care of this for you.

You should back-up your computer before moving forward.

### Option 1: Change the permission to npm's default directory

1. Find the path to npm's directory:

 `npm config get prefix`
For many systems, this will be /usr/local.

WARNING: If the displayed path is just /usr, switch to Option 2 or you will mess up your permissions.

2. Change the owner of npm's directories to the name of the current user (your username!):

 `sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}`
 
 This changes the permissions of the sub-folders used by npm and some other tools 
 (lib/node_modules, bin, and share).
 
### Option 2: Change npm's default directory to another directory

There are times when you do not want to change ownership of the default directory that npm uses (i.e. /usr) as this could cause some problems, for example if you are sharing the system with other users.

Instead, you can configure npm to use a different directory altogether. In our case, this will be a hidden directory in our home folder.

1. Make a directory for global installations:

 `mkdir ~/.npm-global`
 
2. Configure npm to use the new directory path:

 `npm config set prefix '~/.npm-global'`
 
3. Open or create a ~/.profile file and add this line:

 `export PATH=~/.npm-global/bin:$PATH`
 
4. Back on the command line, update your system variables:

 `source ~/.profile`
 
Test: Download a package globally without using sudo.

 `   npm install -g jshint`
    
Instead of steps 2-4 you can also use the corresponding ENV variable (e.g. if you don't want to modify ~/.profile):

 `   NPM_CONFIG_PREFIX=~/.npm-global npm install -g jshint`
 
### Option 3: Use a package manager that takes care of this for you.

If you're doing a fresh install of node on Mac OS you can avoid this problem altogether by using the Hombrew package manager. Homebrew sets things up out of the box with the correct permissions.
`brew install node`

v2和v3版本npm的差异

![v2-1](https://docs.npmjs.com/images/how-npm-works/deps4.png)

![v2-2](https://docs.npmjs.com/images/npm3deps2.png)

![v3](https://docs.npmjs.com/images/npm3deps4.png)

```
npm install (with no args, in package dir)
npm install [<@scope>/]<name>
npm install [<@scope>/]<name>@<tag>
npm install [<@scope>/]<name>@<version>
npm install [<@scope>/]<name>@<version range>
npm install <tarball file>
npm install <tarball url>
npm install <folder>

alias: npm i
common options: [-S|--save|-D|--save-dev|-O|--save-optional] [-E|--save-exact] [--dry-run]
```

![nested](https://docs.npmjs.com/images/npm3deps5.png)

![nested2](https://docs.npmjs.com/images/npm3deps6.png)

![top-level](https://docs.npmjs.com/images/npm3deps7.png)

![top-level2](https://docs.npmjs.com/images/npm3deps8.png)

![aup](https://docs.npmjs.com/images/npm3deps9.png) ![eup](https://docs.npmjs.com/images/npm3deps11.png)

![dedupe](https://docs.npmjs.com/images/npm3deps12.png)

`npm dedupe`

`npm ddp`

![](https://docs.npmjs.com/images/npm3deps13.png)

## npm3 does not install dependencies in a deterministic way.
## 文件夹 

`npm dist-tag ls [<pkg>]`

`npm outdated`

Tags can be used to provide an alias instead of version numbers.



