### 文件目录

```php
.
├── .git  //git文件目录 由git init生成
├── .gitignore  //忽略文件
├── .idea  
├── .travis.yml //travis配置文件
├── LICENSE 
├── README.md //可放置自动构建状态  gitbook 主页连接等
├── SUMMARY.md //目录
├── assets //其他资源文件 比如内嵌的图片
├── book.json //gitbook 配置
├── node_modules //gitbook 配置依赖和主题依赖
├── package-lock.json //依赖包
└── styles //样式自定义
```



### Travis 继续继承配置

```bash
language: node_js
node_js: stable
#安装npm gitbook-cli gitbook-summary
install:
  - npm install
  - npm install gitbook-cli -g
  - npm install -g gitbook-summary
#事件通知邮箱配置
notifications:
  email:
    - nuxseme@gmail.com
  on_success: always
  on_failure: always
#安装gitbook 在当前目录构建book  构建目录
script:
  - gitbook -v
  - gitbook install
  - gitbook build
  - book sm
#进入构建好的目录下初始化git 并强行更新到对应的仓储
after_script:
  - cd ./_book
  - ls -al
  - git init
  - git config --global user.name "nuxseme"
  - git config --global user.email "nuxseme@gmail.com"
  - git add .
  - git commit -m "Auto deploy from Travis-CI "
  - git push --force  "https://${GH_TOKEN}@${GH_REF}" master:master #需优先定义好git token

branches:
  only:
    - master
env:
 global:
   - GH_REF: github.com/nuxseme/project.git

# 在各个阶段可以输出版本号及列出文件方便在job log中定位自动化构建异常及错误
```



### Book.json

```json
"styles": {
	"website": "styles/website.css"         
    } #定义外部样式引用
"fontsettings": {
            "theme": "night",
            "family": "sans" 
        }#定义默认主体及字体
```

默认主体及字体只能在github pages有效或者默认的直接使用构建出的源文件有效 由github同步直gitbook之后，新版的gitbook不支持自定义默认主体和字体

### Assets

引用资源目录

### Summary

目录文件，依赖gitbook-summary

```bash
npm install -g gitbook-summary
book sm
```

可以按数字前缀排列目录的顺序

### 构建流程图

![构建流程图](./assets/构建流程图.png)


