# giffgaff
Giffgaff节点搭建-worker/3x-ui
写在前面的话，其实就是简单的443端口搭建vless+套证书，本教程不仅适用于giffgaff节点搭建，也适用于正常节点搭建。
大致分为两种，第一种是自己没有vps服务器的，可以利用赛博大善人cloudflare（以下简称cf）；第二种是自己有服务器的，那就直接搭建，套域名+证书就ok。
本教程综合整理网络公开搭建方法，适用于小白宝宝体质，很简单，所以不必担心你会搭建不成功。特别是某zr这个猪头，你也能学会。下面，废话不多说，来吧。

-------------------------------------------------------------------------------
***⚠️ 免责声明***
本免责声明适用于 GitHub 上的 “giffgaff" 项目（以下简称“本项目”），项目链接为:https://github.com/chen686868/giffgaff

用途
本项目仅供教育、研究和安全测试目的而设计和开发。旨在为安全研究人员、学术界人士及技术爱好者提供一个探索和实践网络通信技术的工具。

合法性
在下载和使用本项目代码时，必须遵守使用者所适用的法律和规定。使用者有责任确保其行为符合所在地区的法律框架、规章制度及其他相关规定。

免责
我强调本项目仅应用于合法、道德和教育目的。
作者不认可、不支持亦不鼓励任何形式的非法使用。如果发现本项目被用于任何非法或不道德的活动，作者将对此强烈谴责。
作者对任何人或组织利用本项目代码从事的任何非法活动不承担责任。使用本项目代码所产生的任何后果，均由使用者自行承担。
作者不对使用本项目代码可能引起的任何直接或间接损害负责。
为避免任何意外后果或法律风险，使用者应在使用本项目代码后的 24 小时内删除代码。
通过使用本项目代码，使用者即表示理解并同意本免责声明的所有条款。如使用者不同意这些条款，应立即停止使用本项目。

作者保留随时更新本免责声明的权利，且不另行通知。最新版本的免责声明将发布在本项目的 GitHub 页面上。

-------------------------------------------------------------------------------
***论坛大佬的二级域名申请地址***
[https://dns.45mb.com/](https://dns.45mb.com)

**友情链接**:
https://www.yaohuo.me/bbs-1430565.html
https://github.com/lowrie2000/test/blob/main/3x-ui-giffgaff.md

***1.  利用cf***
适用于自己没服务器的，或者纯白嫖的，缺点是公交车，网速慢延迟高等。
这个分为两类方法，edgetunnel和bpb

**1.1 edgetunnel**
方法来源于：[https://github.com/zizifn/edgetunnell](https://github.com/zizifn/edgetunnell)
建议使用修缮版：[https://github.com/cmliu/edgetunnel](https://github.com/cmliu/edgetunnel)

edgetunnel主要有两种方式部署，分为cf的worker和pages，具体方法差不多，效果自测。

**1.1.1 edgetunnel-worker**

a.部署 CF Worker：

打开cf官网仪表板 [https://dash.cloudflare.com/](https://dash.cloudflare.com/)，在 CF Worker 控制台（左侧列表的“计算（worker）”字样，展开，点击Worker and Pages）中创建一个新的 Worker。用默认选择的Worker的“从 Hello World! 开始”。直接部署，注意名字不要包含任何edgetunnel的字样，以规避cf检查。
前往上面给出的两个方法来源网站中，找到[https://github.com/cmliu/edgetunnel/blob/main/_worker.js] _worker.js 的文件，将其内容复制，全选替换 Worker 编辑器中原来的全部代码，部署。**注意本步骤复制时，建议使用电脑操作，手机复制不了那么多字符。**
将第 4 行 userID 修改成你自己的 UUID 。UUID自己随机生成一个就好，可以到这个网站去生成，[https://www.lddgo.net/string/uuid](https://www.lddgo.net/string/uuid)


b.访问订阅内容：
访问 https://替换成你的worker的url链接/UUID 即可获取订阅内容。


c.给 workers绑定 自定义域：
在 workers控制台的 触发器选项卡，下方点击 添加自定义域。
填入论坛大佬免费的次级域名[https://dns.45mb.com/](https://dns.45mb.com)，例如:vless.ocardinalcommerce.com后 点击添加自定义域，等待证书生效即可。
如果你是小白，你现在可以直接用了，不用再往下看了！！！
使用自己的优选域名/优选IP的订阅内容：


d.如果你想使用自己的优选域名或者是自己的优选IP，可以参考[ WorkerVless2sub GitHub ](https://github.com/cmliu/WorkerVless2sub)仓库 中的部署说明自行搭建。
打开[https://github.com/cmliu/edgetunnel/blob/main/_worker.js] _worker.js 文件，在第 12 行找到 sub 变量，将其修改为你部署的订阅生成器地址。例如 let sub = 'sub.cmliussss.workers.dev';，注意不要带https等协议信息和符号。
注意，如果您使用了自己的订阅地址，要求订阅生成器的 sub域名 和 [YOUR-WORKER-URL]的域名 不同属一个顶级域名，否则会出现异常。您可以在 sub 变量赋值为 workers.dev 分配到的域名。


**1.1.1.x edgetunnel-worker的简易部署，合适手机操作**

与上面方法步骤都一样，就是在a步骤的时候，创建worker，选择使用github仓库。使用这一步，需要你在github中点击fork到你自己的仓库。建议fork的时候，名字还是换成无关的字样，甚至随便输入无意义字符，防止被ban。


**1.1.2 edgetunnel-**
部署 CF Pages：

a.下载[ main.zip ](https://github.com/cmliu/edgetunnel/archive/refs/heads/main.zip)文件
在 CF Pages 控制台中选择 上传资产后，为你的项目取名后点击 创建项目，然后上传你下载好的 main.zip 文件后点击 部署站点。
部署完成后点击 继续处理站点 后，选择 设置 > 环境变量 > 制作为生产环境定义变量 > 添加变量。 变量名称填写UUID，值则为你的UUID，可以在这个网站生成一个[https://www.lddgo.net/string/uuid](https://www.lddgo.net/string/uuid)后点击 保存即可。
返回 部署 选项卡，在右下角点击 创建新部署 后，**重新上传** main.zip 文件后点击 保存并部署 即可。**（每次部署之后，再有设置改动，需要重新部署）**


b.访问订阅内容：
访问 https://替换成你的worker的url链接/UUID 即可获取订阅内容。


c.给 Pages绑定 CNAME自定义域：
在 Pages控制台的 自定义域选项卡，下方点击 设置自定义域。
填入论坛大佬免费的次级域名，例如:vless.ocardinalcommerce.com后 点击添加自定义域，等待证书生效即可。
如果你是小白，那么你的 pages 绑定自定义域之后即可直接起飞，不用再往下看了！！！
使用自己的优选域名/优选IP的订阅内容：


d.如果你想使用自己的优选域名或者是自己的优选IP，可以参考[ WorkerVless2sub GitHub ](https://github.com/cmliu/WorkerVless2sub)仓库 中的部署说明自行搭建。
在 Pages控制台的 设置选项卡，选择 环境变量> 制作> 编辑变量> 添加变量；
变量名设置为SUB，对应的值为你部署的订阅生成器地址。例如 sub.cmliussss.workers.dev，后点击 保存。
之后在 Pages控制台的 部署选项卡，选择 所有部署> 最新部署最右的 ...> 重试部署，即可。
注意，如果您使用了自己的订阅地址，要求订阅生成器的 SUB域名 和 [YOUR-PAGES-URL]的域名 不同属一个顶级域名，否则会出现异常。您可以在 SUB 变量赋值为 Pages.dev 分配到的域名。


***1.2 bpb***
方法来源于：[https://github.com/bia-pain-bache/BPB-Worker-Panel](https://github.com/bia-pain-bache/BPB-Worker-Panel)
建议使用修缮版（带自动更新功能）：[https://github.com/byJoey/wk-Auto-update](https://github.com/byJoey/wk-Auto-update)

**同样的，还是不要使用有关方法的字样创建工程，避免被ban**

a.fork工程
前往[https://github.com/byJoey/wk-Auto-update](https://github.com/byJoey/wk-Auto-update)，同样的fork到自己仓库

**注意**
在fork好之后，需要检查一下这个文件的actions，在code的那一栏的右边，如果提示no run等等，点击在右边的settings，然后左侧有个actions展开，里面选择general，划到最先，有个工作流权限，选择读取存储库内容和包权限（正常韭菜最下面的最下面那个选项），然后save保存就好了。

b.部署cf的pages
打开cf官网仪表板 [https://dash.cloudflare.com/](https://dash.cloudflare.com/)，在 CF Worker 控制台（左侧列表的“计算（worker）”字样，展开，点击Worker and Pages）中创建一个新的 pages，选择 导入现有 Git 存储库，导入刚刚的fork项目。
继续，划到最下面，添加环境变量（高级），添加如下三项：
UUID  在这个网站生成一个[https://www.lddgo.net/string/uuid](https://www.lddgo.net/string/uuid)
TR_PASS  自己设定
FALLBACK  推荐设置为example.com

完成好之后，直接部署。

c.配置KV
cf网页的左侧一栏，存储和数据库 展开，选择KV，创建，名字随便。
然后来到刚刚的pages页面，到设置里面，左侧 绑定 ，添加一个名字为**小写**的kv，选择你刚刚创建的KV数据库。

这时候，已经可以作为日常魔法节点使用了，但是用giffgaff免流，还需要添加自定义域名

d.添加自定义域名
pages那一栏 自定义域名，[https://dns.45mb.com/](https://dns.45mb.com)，添加进去。

e.重新部署
回到pages的第一栏 部署，所有部署里面，找一个最新的记录，右边三个点展开，重试部署。

f.使用
你的面板地址为 https://域名/panel
第一次进入 需要设置密码，之后忘记了可以在存储与数据库里面的KV里面找到PASSWORD的值。



***2.  利用vps部署3x-ui***
原版：[https://github.com/MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui)

汉化+功能增强版（适合汉语宝宝体质）：[https://github.com/gm-cx/3x-ui-cn](https://github.com/gm-cx/3x-ui-cn)


a.开放你的服务器的端口，80,443,2053等
如果你是小白，这一步跳过，正常的都没开。

b.使用脚本一键拉取部署3x-ui
正常电脑使用MobaXterm或者finalshell，本人推荐使用前面那个。大家可以直接搜索汉化破解版，仅作为学习交流用途哦！
手机使用Termius 或者不少服务器供应商都自带web的VNC链接方式，如xtermJs和noVnc。

先来更新一下vps：
apt update
apt upgrade -y
apt dist-upgrade -y
apt autoclean
apt autoremove -y

原版3x-ui的部署命令为：
bash <(curl -Ls https://raw.githubusercontent.com/MHSanaei/3x-ui/refs/tags/v2.6.0/install.sh)

这里推荐用 汉化+功能增强版（适合汉语宝宝体质）：
bash <(curl -Ls https://raw.githubusercontent.com/xeefei/3x-ui/master/install.sh)

跟着脚本走，很简单。安装好之后，使用 x-ui 命令，可以再次调出脚本选项

c.关闭防火墙
注意，有的服务器供应商需要网站里面关一次防火墙，在vps机器里面再关一次，如oracle就是
用x-ui调出脚本之后，选择21，关闭防火墙

d.开机bbr（建议）
x-ui命令之后 选择22，主要是一个优化和加速的作用，开了不亏

e.申请证书（可选）
x-ui命令之后 选择18。需要提前在域名申请网站申请一个域名
giff配套使用的域名申请[https://dns.45mb.com/](https://dns.45mb.com)
然后跟着脚本走，注意，有的机器可能会比较慢，耐心等待就好。

f.面板设置
x-ui命令之后 选择10，看到你的登陆链接。复制到浏览器，打开。
这里有个小技巧，如果你x-ui的部署端口改成80，登录访问路径设置为空，这样直接就可以使用域名进入到x-ui的管理面板，省的你自己再用nginx了。但是这样有一个缺点，就是证书的自动申请会使用80端口。如果你是用的giffgaff的现成的证书，可以直接忽视80端口的占用问题。毕竟这是小白直接用域名进入管理面板的最简单方式。

管理面板左侧 面板设置，下划，
面板证书公钥文件路径，粘贴如下内容：
/root/.acme.sh/你的子域名.ocardinalcommerce.com_ecc/你的子域名.ocardinalcommerce.com.cer
面板证书密钥文件路径，粘贴如下内容：
/root/.acme.sh/你的子域名.ocardinalcommerce.com_ecc/你的子域名.ocardinalcommerce.com.key

g.添加节点  **认真观看**
面板左侧 入站列表，右侧 添加入站，协议使用vless和vmess，这里曾有个猪头问我，这两种啥区别，我说的是vless速度快一点，但是相对的不安全，vmess速度慢一点，但是安全一点。
端口号改成443（**giffgaff免流用户**）
传输 TCP WebSocket gRPC 三选一，有一种说法是giffgaff断流问题，使用gRPC类型可以解决，这里自测。
安全 改成TLS
SNI输入你的域名
下面Allow Insecure默认关闭，给它打开
数字证书，如果你上面自己申请了证书的，选择文件路径，然后点击 从面板设置证书。如果你没申请证书，可以选择 文件内容，粘贴论坛大佬现成的证书。域名填写cardinalcommerce.com，https://luqiao.lanzouw.com/iiGri2x559af，这个文件我会上传到我的realise。
没了，最右下角，添加。
然后你会发现列表里面多出一个节点，最左侧 加号，点击展开，点击二维码图标，复制到你的软件上。

手机的v2rayn，在设置里面 高级 把跳过tls验证勾选。
