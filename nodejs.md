
# Node.js

## node 版本含义

版本格式：主版本号.次版本号.修订号，版本号递增规则如下：

* 主版本号：当你做了不兼容的 API 修改，
* 次版本号：当你做了向下兼容的功能性新增，
* 修订号：当你做了向下兼容的问题修正。    
先行版本号及版本编译信息可以加到“主版本号.次版本号.修订号”的后面，作为延伸。

  |表达式|版本范围|说明|
  |:-|:--|:-----|
  |1.1.1|1.1.1|匹配指定版本|
  |^1.0.0	|>=1.0.0 且 <2.0.0	|^表示与指定的版本兼容，左边第一个非0字段不可变，后面的可变，即1.X.X但不得到2.0.0|
  |^0.0.3	|>=0.0.3 且 <0.0.4 |同上|
  |^5.x	|>=5.0.0 且 <6.0.0	|同上|
  |~0.1.1	|>=0.1.1 且 <0.2.0	|~表示约等于版本，如果存在次版本号，则允许修订号为最高的，否则允许次版本为最高，如 ~1匹配>=1.0.0 且 <2.0.0|
  |*	|匹配 >=0.0.0	|通配符|
  |>=3.0.0	|>=3.0.0	|其他符号还有<,<=,>,>=,=.字面意思。可使用空格表示AND，双竖线表示OR，範例：1.2.7 双竖线 >=1.2.9 <2.0.0 表示可包含 1.2.7、    1.2.9 和 1.4.6，不可包含 1.2.8 或 2.0.0|
  |1.30.2 - 2.30.2	|>=1.30.2 且 <=2.30.2|	字面意思|



## npm 常用命令
#### 查看 npm 包安装根路径
> -g 选项是可选的，不加就是当前项目包的根路径，否则是全局包的根路径  

    npm root [-g]

#### 查看/设置 npm 包安装路径前缀

    npm config get prefix
    npm config set prefix path

#### 清除缓存

     npm cache clean --force

#### production
添加了 production 参数后将仅仅安装  package.json 中dependencies 里面的包，不会安装 devDependencies 里面的包。


***

## [CommonJS](http://www.commonjs.org/)
CommonJS是服务器端模块的规范，Node.js采用了这个规范。    
根据CommonJS规范，一个单独的文件就是一个模块。加载模块使用require方法，该方法读取一个文件并执行，最后返回文件内部的exports对象。

***

## Node.js 错误
  
#### node-gyp rebuild 错误

有时安装 nodejs 插件时会出现以下错误：

    > node-gyp rebuild


    E:\ArangoDB3e-3.2.1-2_win64\var\lib\arangodb3-apps\_db\arango_gpro\my\gpro\APP\node_modules\nodejieba>if not defined npm_config_node_gyp (node "C:\Program Files\nodejs\node_modules\npm\bin\node-gyp-bin\\..\..\node_modules\node-gyp\bin\node-gyp.js" rebuild )  else (node "" rebuild )
    gyp ERR! configure error
    gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    gyp ERR! stack     at failNoPython (C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\lib\configure.js:449:14)
    gyp ERR! stack     at C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\lib\configure.js:404:11
    gyp ERR! stack     at C:\Program Files\nodejs\node_modules\npm\node_modules\graceful-fs\polyfills.js:264:29
    gyp ERR! stack     at FSReqWrap.oncomplete (fs.js:123:15)
    gyp ERR! System Windows_NT 6.1.7601
    gyp ERR! command "C:\\Program Files\\nodejs\\node.exe" "C:\\Program Files\\nodejs\\node_modules\\npm\\node_modules\\node-gyp\\bin\\node-gyp.js" "rebuild"
    gyp ERR! cwd E:\ArangoDB3e-3.2.1-2_win64\var\lib\arangodb3-apps\_db\arango_gpro\my\gpro\APP\node_modules\nodejieba
    gyp ERR! node -v v6.10.0
    gyp ERR! node-gyp -v v3.4.0
    gyp ERR! not ok

原因是 node-gyp 安装失败。 node-gyp 用来编译原生C++模块，可能是下载那几个模块的时候 node-gyp 会编译那些模块。

解决方法： 
Windows 下安装以下工具，以管理员的身份启动 powershell，输入以下命令：
    npm --add-python-to-path --python-mirror=https://npm.taobao.org/mirrors/python/ install --global windows-build-tools

node-gyp rebuild 的故障解决办法:      
1 首先清除根目录下的node-gyp  
卸载node-gyp模块   

        npm uninstall node-gyp -g
2 安装环境 

        npm i -g windows-build-tools
重新安装node-gyp

        npm install -g node-gyp
3 设置python版本

        npm iconfig set python python
4 安装.net 2.0 
5 安装vs 201*  c++  环境
6 安装microtime运行时

        npm i microtime --save-dev



#### node-pre-gyp install --fallback-to-build 错误
此时，安装node-gyp

    npm install -g node-gyp



node.js 相关博文    
[PHP vs node.js: The REAL statistics](https://prahladyeri.com/blog/2014/06/php-vs-node-js-real-statistics.html)    



