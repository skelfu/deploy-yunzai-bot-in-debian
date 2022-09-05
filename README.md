# 云崽bot安装方法：Debian
0、访问链接查看CentOs安装方法：[Linux环境搭建](https://github.com/Le-niao/Yunzai-Bot/issues/3)

note：
如果下载慢或者下载失败百度linux系统换软件的方法；

## 更新软件和确保sudo能用
1、**root**权限下：
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

### 安装nodejs
`apt install nodejs`

### 安装npm
`apt install npm`

### 检查版本
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
喵喵插件地址：[Github](https://github.com/yoimiya-kokomi/miao-plugin),[Gitee](https://gitee.com/yoimiya-kokomi/miao-plugin)
成就：[GitHub](https://github.com/zolay-poi/achievements-plugin),[Gitee](https://gitee.com/zolay-poi/achievements-plugin)
查委托：不知道地址，某群友给的

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

----
**For yunzaiV3**
查委托.js
内容：
```
import plugin from '../../lib/plugins/plugin.js'
import fetch from 'node-fetch'

export class assignment extends plugin {
  constructor () {
    super({
      /** 功能名称 */
      name: '查委托',
      /** 功能描述 */
      dsc: '查每日委托',
      /** https://oicqjs.github.io/oicq/#events */
      event: 'message',
      /** 优先级，数字越小等级越高 */
      priority: 500,
      rule: [
        {
          /** 命令正则匹配 */
          reg: '^#*查委托(.*)',
          /** 执行方法 */
          fnc: 'task123321'
        }
      ]
    })
  }
  /****
   * #task123321
   * @param e oicq传递的事件参数e
   */
async task123321 (e) {
    logger.info("TEST_LOG"+e.msg.replace(/#|＃|查委托/g, ""));
    //
    let url = "http://114.132.218.87:12583/api/genshin/task/" + e.msg.replace(/#|＃|查委托/g, ""); //接口地址
    let response = await fetch(url); //调用接口获取数据
    let data = await response.json();//结果json字符串转对象
    let msg, name, code = response.status;
    if (code == 201) //请求不成功
    {
        e.reply(data.msg);
    }
    else if (code == 200) 
    {
        if (data.hidden == true) 
        {//判断是不是隐藏
            name = "隐藏成就" + ('《' + data.name + '》\n')
        } else {
            name = "成就" + ('《' + data.name + '》\n')
        }
        msg = data.desc + "\n"
        for (var key of data.involve) {
            if (key.type == '世界任务') break;
            msg += '每日委托' + ('《' + key.task + '》\n')
        }
        msg += "————\n" + data.msg + "\n————\n※ 文案: B站 oz水银"
        logger.info('[查委托]\n'+name + msg);
        e.reply(name + msg);
    } 
    else 
    {
        logger.error("[查委托]错误：\n"+data.msg);
        e.reply("错误：\n"+data.msg);
    }
    }
}
```
---- 

**For YunzaiV2**
查委托.js（文件名随意）
内容：
```
import { segment } from "oicq";
import fetch from "node-fetch";

//项目路径
const _path = process.cwd();

export const rule = {
  task123321: {
    reg: "^#*查委托(.*)", //匹配消息正则，命令正则
    priority: 500, //优先级，越小优先度越高
    describe: "查委托是否有成就，以及委托详细过程", //【命令】功能说明
  },
};

export async function task123321(e) {

  let url = "http://114.132.218.87:12583/api/genshin/task/" + e.msg.replace(/#|＃|查委托/g, ""); //接口地址
  let response = await fetch(url); //调用接口获取数据
  let data = await response.json();//结果json字符串转对象
  let msg, name, code = response.status;
  if (code != 200 && code != 201) return;//请求不成功
  function requestOK() {
    if (data.hidden == true) {//判断是不是隐藏
      name = "隐藏成就" + ('《' + data.name + '》\n')
    } else {
      name = "成就" + ('《' + data.name + '》\n')
    }
    msg = data.desc + "\n"
    for (var key of data.involve) {
      if (key.type == '世界任务') break;
      msg += '每日委托' + ('《' + key.task + '》\n')
    }
    msg += "————\n" + data.msg + "\n————\n※ 文案: B站 oz水银"
    e.reply(name + msg);
  };
  if (code == 201) return e.reply(data.msg);
  if (code == 200) return requestOK();
  return true; 
}
```


