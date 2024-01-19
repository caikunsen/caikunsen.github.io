
# 1、Hexo 搭建步骤
## 1. 安装Hexo
- centos
```
npm install -g hexo-cli
```
- ubuntu
```
sudo npm install -g hexo-cli
```
## 2. 初始化Hexo 
```
hexo init blog
```
切换至blog目录 安装相应依赖
```
cd blog/
npm install
```
打开体验Hexo服务
```
hexo g
hexo s
```
     g 生成静态文件。
     s 启动服务器。默认情况下，访问网址为： `http://localhost:4000/`
打开浏览器输入访问网站可看到初始化的博客页面 
至此Hexo搭建完成

以下是目录说明：
|项目|说明|
|---|---|
|node_modules|依赖包|
|source|用来存放你的文章|
|themes|主题|
|_config.yml|博客的配置文件|
|public|存放生成的页面|
|scaffolds|生成文章的一些模板|

## 3.配置Hexo相应文件 方便后面托管到GitHub Pages
使用 `node --version` 指令检查你电脑上的 Node.js 版本，并记下该版本号(例如: 14.21.3)
在blog根目录下 找到.github文件，然后创建`pages.yml`文件，讲一下内容填入`pages.yml`文件。
注意 将以下的`16`版本号替换为你自己的版本号 然后保存文件
```
name: Pages  
  
on:  
  push:  
    branches:  
      - main # default branch  
  
jobs:  
  build:  
    runs-on: ubuntu-latest  
    steps:  
      - uses: actions/checkout@v3  
        with:  
          token: ${{ secrets.GITHUB_TOKEN }}  
          # If your repository depends on submodule, please see: https://github.com/actions/checkout  
          submodules: recursive  
      - name: Use Node.js 16  
        uses: actions/setup-node@v2  
        with:  
          node-version: '16'  
      - name: Cache NPM dependencies  
        uses: actions/cache@v2  
        with:  
          path: node_modules  
          key: ${{ runner.OS }}-npm-cache  
          restore-keys: |  
            ${{ runner.OS }}-npm-cache  
      - name: Install Dependencies  
        run: npm install  
      - name: Build  
        run: npm run build  
      - name: Upload Pages artifact  
        uses: actions/upload-pages-artifact@v2  
        with:  
          path: ./public  
  deploy:  
    needs: build  
    permissions:  
      pages: write  
      id-token: write  
    environment:  
      name: github-pages  
      url: ${{ steps.deployment.outputs.page_url }}  
    runs-on: ubuntu-latest  
    steps:  
      - name: Deploy to GitHub Pages  
        id: deployment  
        uses: actions/deploy-pages@v2
```


# 2、创建GitHub个人仓库
登录[GitHub](https://github.com), 点击`New repository`建立名为 `<你的 GitHub 用户名>.github.io` 的仓库
例:`zhangsan.github.io`
# 3、将blog目录推送到刚刚创建的仓库
git会默认把git控制的所有文件都加入到版本控制，在推送之前 我们需要将一些文件过滤掉

在blog根目录创建文件 `.gitignore`  将以下填入文件，依赖文件移除版本控制
     node_modules/

在blog根目录下执行命令 (由于github规则，分支“master”不允许部署到github-pages。所以下面我们要将目录提交到main分支)
```
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:你的GitHub用户名/你的GitHub用户名.github.io
git push -u origin main
```
提交推送完成

# 4、域名配置及GitHub配置
- 域名配置: 添加2条**CNAME**记录 @、wwww  指向你的github服务（`你的GitHub用户名.github.io`）
- github配置: 在你的仓库中前往`Settings > Pages > Source `并将 `Source` 改为 `GitHub Actions` 并在`Custom domain`中设置你的域名保存。
