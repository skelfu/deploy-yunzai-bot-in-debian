# 云崽bot部署方法：Debian
0、访问链接查看CentOs安装方法：[Linux环境搭建](https://github.com/Le-niao/Yunzai-Bot/issues/3)

note：
如果下载慢或者下载失败百度linux系统换软件的方法；

## 更新软件和确保sudo能用
**root**权限下：
```
apt-get update
apt-get upgrade
apt-get install sudo
```

### 可能遇到的问题
#### 只有root，没有用户
```
useradd -m -s /bin/bash 用户名
passwd 用户名
输入密码-重复密码
vi /etc/passwd
```
使用：  
`sudo adduser 用户名 sudo`  
添加此用户到sudoers文件

## 安装redis
在**root**下：
`apt-get install redis redis-server`

## 安装nodejs和npm
### 1、添加nodeSource库
在**root**下：  
`curl -sL https://deb.nodesource.com/setup_16.x | bash - `

### 2、安装nodejs
`apt install nodejs`

### 3、安装npm
`apt install npm`

### 4、检查版本
```
node -v
npm -v
```

## 安装中文字体
**root**下：
具体教程链接：[安装中文字体](https://codeleading.com/article/53945756330)
```
apt-get install ttf-mscorefonts-installer
apt-get install fontconfig

mkfontscale
mkfontdir
fc-cache -fv
fc-list

apt-get install locales
dpkg-reconfigure locales
```

用↓键翻页找到：  
\[ \]en_US ISO-8859-1  
\[ \]en_US.UTF8 UTF-8  
\[ \]zh_CN GB2312  
\[ \]zh_CN.GB18030 GB18030  
\[ \]zh_CN GBK GBK  
\[ \]zh_CN UTF-8 UTF-8  
然后用空格选中变成\[\*\],用tab切换选中OK

`apt-get install ttf-wqy-zenhei`

## 克隆机器人
**切换回普通用户**  
（以免后面用普通用户访问root权限文件出各种各样问题）  
`su 用户名`

此时状态：
**用户名@设备名：~$**  
V2,V3选一个，推荐V2  
### yunzai-botV3:
```
git clone --depth=1 -b main https://github.com/Le-niao/Yunzai-Bot.git
cd Yunzai-Bot
npm install pnpm -g
pnpm install -P
```
### yunzai-botV2:
```
git clone https://github.com/yoimiya-kokomi/Yunzai-Bot.git
cd Yunzai-Bot
npm install -g cnpm --registry=https://registry.npmmirror.com
cnpm install
```

## 安装chrome依赖
具体查看这里：[Puppteer-故障排除](https://pptr.dev/troubleshooting)

```
sudo apt-get install ca-certificates fonts-liberation libappindicator3-1 libasound2 libatk-bridge2.0-0 libatk1.0-0 libc6 libcairo2 libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgbm1 libgcc1 libglib2.0-0 libgtk-3-0 libnspr4 libnss3 libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 lsb-release wget xdg-utils
```

## 首次运行
`node app`
按照提示登陆小号  
退出：  
Ctrl+C

## 添加插件
### 常用插件（反正我常用）
|插件名|项目地址|
|--:|--:|
|喵喵插件|[Github](https://github.com/yoimiya-kokomi/miao-plugin),[Gitee](https://gitee.com/yoimiya-kokomi/miao-plugin)|  
|成就|[GitHub](https://github.com/zolay-poi/achievements-plugin),[Gitee](https://gitee.com/zolay-poi/achievements-plugin)|
|查委托|[Gitee](https://gitee.com/mofengdada/chaweituo)|  

### 其他插件  
[Yunzai-Bot插件目录](https://github.com/yhArcadia/Yunzai-Bot-plugins-index)  

### 安装喵喵插件
此时状态：**非root**  
**用户名@设备名：~$**
```
cd Yunzai-Bot
git clone https://github.com/yoimiya-kokomi/miao-plugin.git ./plugins/miao-plugin/
```

### 安装成就插件
此时状态：**非root**  
**用户名@设备名：~$**
```
cd Yunzai-Bot
git clone https://gitee.com/zolay-poi/achievements-plugin.git ./plugins/achievements-plugin/
```

### 更新喵喵插件
直接给bot发送：
`#喵喵更新`
即可自动更新喵喵插件  
手动：
在bot目录下输入：
```
cd ./plugins/miao-plugin/
git pull https://github.com/yoimiya-kokomi/miao-plugin.git
```

### 更新成就插件（懒得更新）
```
cd ./plugins/achievements-plugin/
git pull https://gitee.com/zolay-poi/achievements-plugin.git 
```

## 后台启动bot
在bot目录下：
`npm start`
查看log：
`npm run log`
停止运行：
`npm stop`

## 更新云崽bot
**先停止运行机器人**  
在bot目录下输入：
`npm stop`
### 更新V2
在bot目录下输入：  
`git pull https://github.com/yoimiya-kokomi/Yunzai-Bot.git`
### 更新V3
在bot目录下输入：  
`git pull https://github.com/Le-niao/Yunzai-Bot.git main`


