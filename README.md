# 系统要求：
CentOS 6+ / Debian 6+ / Ubuntu 14.04 +
推荐 Debian 7 x64，这个是我一直使用的系统，我的脚本在这个系统上面出错率最低。并且最容易安装锐速（锐速不支持OpenVZ）

CentOS 7 自带防火墙问题(firewalld)自行解决，其他版本没有做测试

脚本版本脚本特点：



本脚本与另一个SSR脚本 『原创』CentOS/Debian/Ubuntu ShadowsocksR 单/多端口 一键管理脚本的区别是什么？
ssrmu.sh 脚本是单服务器多用户脚本，使用的是 SSR服务端的MudbJSON模式，可以给每个用户(端口)设置不同的加密方式/协议/混淆/限制速度/设备数限制/可用总流量等功能。即实现单服务器多用户流量管理等功能。
而 ssr.sh 则是单服务器单用户脚本，使用的是 SSR服务端的单用户配置方式，即使实现了多端口，但是还算不算多用户，不支持每个用户(端口)不同的加密方式/协议/混淆等，并且无法管理流量使用。
如何选择这两个脚本？
根据你的需求选择，比如你仅仅是 一个或两个人使用，并且不需要流量管理功能，那么选择 ssr.sh 好了。而如果很多人使用，并且都需要限制流量来管理，那你适合使用 ssrmu.sh，所以自己看着选，多试试（两个脚本不能共存）！

# 安装步骤

#简单的来说，如果你什么都不懂，那么你直接一路回车就可以了！

本脚本需要Linux root账户权限才能正常安装运行，所以如果不是 root账号，请先切换为root，如果是 root账号，那么请跳过！

sudo su
输入上面代码回车后会提示你输入当前用户的密码，输入并回车后，没有报错就继续下面的步骤安装ShadowsocksR。

注意：如果你安装的有我的另一个 ssr.sh 脚本，请先卸载ShadowsocksR服务端，再安装这个脚本（不能共存）！

wget -N --no-check-certificate https://softs.loan/Bash/ssrmu.sh && chmod +x ssrmu.sh && bash ssrmu.sh
备用下载地址（上面的链接无法下载，就用这个）：
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssrmu.sh && chmod +x ssrmu.sh && bash ssrmu.sh

下载运行后会提示你输入数字来选择要做什么。输入 1 ，就会开始安装ShadowsocksR服务端，并且会提示你输入Shadowsocks的 端口/密码/加密方式/ 
协议/混淆（混淆和协议是通过输入数字选择的）等参数来添加第一个用户。注意：用户名不支持中文，如果输入中文会一直保存下去！

# 使用说明
# 运行脚本，

bash ssrmu.sh

# 还有一个 运行参数，是用于所有用户流量清零的

bash ssrmu.sh clearall

# 不过不需要管这个，可以通过脚本自动化的设置 crontab 定时运行脚本

输入对应的数字来执行相应的命令。
  ShadowsocksR MuJSON一键管理脚本 [vX.X.X]
  ---- Toyo | doub.io/ss-jc60 ----
  1. 安装 ShadowsocksR
  2. 更新 ShadowsocksR
  3. 卸载 ShadowsocksR
  4. 安装 libsodium(chacha20)
  5. 查看 账号信息
  6. 显示 连接信息
  7. 设置 用户配置
  8. 手动 修改配置
  9. 配置 流量清零
 10. 启动 ShadowsocksR
 11. 停止 ShadowsocksR
 12. 重启 ShadowsocksR
 13. 查看 ShadowsocksR 日志
 14. 其他功能
 15. 升级脚本
当前状态: 已安装 并 已启动
请输入数字 [1-15]：
注意：添加/删除/修改 用户配置后，无需重启ShadowsocksR服务端，ShadowsocksR服务端会定时读取数据库文件内的信息，不过修改 用户配置后，
可能要等个十几秒才能应用最新的配置（因为ShadowsocksR不是实时读取数据库的，所以有间隔时间）。

# 文件位置：
安装目录：/usr/local/shadowsocksr
配置文件：/usr/local/shadowsocksr/user-config.json
数据文件：/usr/local/shadowsocksr/mudb.json
注意：ShadowsocksR服务端不会实时的把流量数据写入 数据库文件，所以脚本读取流量信息也不是实时的！



# 其他说明
ShadowsocksR 安装后，自动设置为 系统服务，所以支持使用服务来启动/停止等操作，同时支持开机启动。
启动 ShadowsocksR：/etc/init.d/ssrmu start
停止 ShadowsocksR：/etc/init.d/ssrmu stop
重启 ShadowsocksR：/etc/init.d/ssrmu restart
查看 ShadowsocksR状态：/etc/init.d/ssrmu status
ShadowsocksR 默认支持UDP转发，服务端无需任何设置。
本脚本已经集成了 安装/卸载 锐速(ServerSpeeder)/Lotserver，但是是否支持请查看 Linux支持内核列表。（锐速、LotServer不支持OpenVZ）
注意：本脚本中的 显示链接信息中的 获取IP归属地功能使用的是 IPIP.NET 的免费API接口，
因为限速所以每秒只能检测一次，同时 IPIP.NET 的免费API接口并不会保证稳定性，可能什么时候就突然暂时失效了，这是本人不可控的，有条件可以自建API接口。
ShadowsocksR目前支持的协议和混淆：
协议（Protocol）：origin，auth_sha1_v4，auth_aes128_md5，auth_aes128_sha1，auth_chain_a，auth_chain_b
混淆（Obfs）：plain，http_simple，http_post，random_head，tls1.2_ticket_auth，tls1.2_ticket_fastauth(这个是客户端用的，
而服务端需要选择tls1.2_ticket_auth)
origin 和 plain 是原版，加粗的是推荐使用的。



如果你想要使用tls1.2_ticket_fastauth 混淆插件，那么服务端选择tls1.2_ticket_auth，客户端选择tls1.2_ticket_fastauth 即可。
如果服务端 设置混淆参数为：tls1.2_ticket_auth_compatible (兼容原版)
那么客户端 可使用的混淆为：plain / tls1.2_ticket_auth / tls1.2_ticket_fastauth
tls1.2_ticket_auth 与 tls1.2_ticket_fastauth 的区别为，后者不会等待服务器回应，所以不会增加延迟。适合于，因为混淆插件增加延迟的原因不得不选择原版混淆 plain，
但是又因为QOS等因素而处于延迟与干扰/限速等之间抉择的时候，可以选择tls1.2_ticket_fastauth 客户端混淆插件！
