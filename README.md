# 饥荒联机版开服教程：从挑云服务器到端口放行，4 人开黑档一次跑通

想和朋友长期玩《饥荒联机版》，最后大概率都会走到自己开服这一步。原因不复杂：官方服务器人多的时候延迟飘忽，而用游戏里"创建世界"的方式开档，房主一退出游戏，整个世界就停了。

自建专用服务器的意义就在这两件事上——24 小时在线、延迟可控。下面这套流程按 Ubuntu 写，从买机器到第一次进服，中间不跳步骤。

## 开服前先算清楚：这台机器到底要什么配置

配置买小了会卡，买大了纯浪费。几组被反复验证过的数字摆在这：

- **内存**：一个新存档，地上加洞穴，实际占用大约 1.5–2GB。官方给的最低线是 4GB，6 人以上长期开建议 8GB。
- **磁盘**：服务端本体约 3.9GB，加上 MOD 缓存和几份自动备份，20GB 起步比较稳。
- **CPU**：DST 吃单核性能。双核 2GHz 是底线，人一多、MOD 一上，单核频率比核心数量更管用。
- **带宽**：上行 5Mbps 能跑起来，6 人以上建议 10Mbps 往上。
- **人数参考**：1核2G 大概带 2–3 人；2核4G 覆盖 2–6 人；6 人以上直接看 4核8G。

有个容易被忽略的点：**地上和洞穴是两个独立进程**。同一台机器同时开，内存占用翻倍，CPU 也互相抢。所以给洞穴单独配一台 2核4G 是很常见的方案，两台小机器加起来比一台大机器更稳。

## 选机器：雨云这边哪些产品适合开饥荒

先说品牌。雨云（RainYun）是一家 2018 年成立的国产云服务商，产品线里跟开服直接相关的是两块：**游戏云 VPS** 和**普通云服务器**。

游戏云 VPS 用的是高主频处理器——i9-14900K/KF、13900KF、Ryzen 9950X、7950X、Gold 6146、EPYC 7A23 这些型号，配 NVME 固态和 BGP 高防网络。对 DST 来说，高主频正是关键，所以这条线是首选。它支持 Linux 和 Windows，计费分**固定计费**和**动态计费**两种。

计费方式怎么选，判断标准很简单：每天玩超过 2 小时，选固定计费；低于 2 小时，动态计费更省——闲置时不烧钱，对"周末才开黑"的档特别合适。

还有一个坑要提前说。游戏云部分机房是 **NAT 机型**，也就是几个人共用一个公网 IP，可以开 15 条端口映射，也可以加钱买独立公网 IP。DST 走的是 UDP，而端口映射对 UDP 的支持取决于具体机型。如果你不想折腾，直接选带独立公网 IP 的机型，或者买完加购一个独立 IP（普通 IP 大约 5 元/个/月），省心很多。

雨云支持 **1 元试用 1 天**，云服务器和游戏云还支持 7 天无理由退款（每个账号限 3 次）。这意味着你可以花一块钱先把端口和性能验证一遍，确认能跑再付整月——对第一次开服的人，这比看任何评测都实在。

👉 [先花 1 元试一台雨云机器，验证端口和性能](https://www.rainyun.com/MzA2MTc2_)

### 雨云游戏云（高主频机型，公开挂牌价参考）

| 机房 / CPU | 典型配置 | 上行带宽 | 参考月付 | 适合场景 | 选购 |
| --- | --- | --- | --- | --- | --- |
| 浙江宁波 · Gold 6146 | 2核4G / 50GB NVME | 10Mbps | 63 元起 | 2–4 人纯原版 | [查看这档配置](https://www.rainyun.com/MzA2MTc2_) |
| 浙江宁波 · Gold 6146 | 4核8G / 50GB NVME | 15Mbps | 119 元起 | 4–6 人 + 少量 MOD | [查看这档配置](https://www.rainyun.com/MzA2MTc2_) |
| 浙江宁波 · Gold 6146 | 6核16G / 50GB NVME | 20Mbps | 161 元起 | 地上 + 洞穴同机 | [查看这档配置](https://www.rainyun.com/MzA2MTc2_) |
| 浙江宁波 · Ryzen 9950X | 2核4G | 10Mbps | 210 元/月 | MOD 服，单核优先 | [查看这档配置](https://www.rainyun.com/MzA2MTc2_) |
| 浙江宁波 · Ryzen 9950X | 4核8G | 15Mbps | 330 元/月 | 6 人以上 + MOD | [查看这档配置](https://www.rainyun.com/MzA2MTc2_) |
| 浙江宁波 · Ryzen 9950X | 6核12G | 20Mbps | 450 元/月 | 大型服 / 多存档 | [查看这档配置](https://www.rainyun.com/MzA2MTc2_) |
| 浙江温州双线 · i9-14900K | 4核8G | 15Mbps（100G 高防） | 200 元/月 | 电信+移动双线 | [查看这档配置](https://www.rainyun.com/MzA2MTc2_) |
| 江苏镇江 BGP · i9-14900K | 4核8G | 15Mbps（200G 高防） | 220 元/月 | 三网 BGP，延迟稳 | [查看这档配置](https://www.rainyun.com/MzA2MTc2_) |

套餐的 CPU、内存、带宽、硬盘都可以弹性调整，所以表里的价格是公开挂牌价，下单时会随区域、库存和活动浮动，以结算页显示为准。

### 雨云普通云服务器（美国洛杉矶 / 中国香港）

如果你还想在这台机器上顺手放个网站，普通云服务器的档位更全：

| 机房 | 方案 | 配置 | 带宽（上/下） | 月付 | 选购 |
| --- | --- | --- | --- | --- | --- |
| 美国洛杉矶 | KVM 入门版 | 1核1G / 30–2000GB 硬盘 | 20M / 30M | 25 元 | [查看云服务器档位](https://www.rainyun.com/MzA2MTc2_) |
| 美国洛杉矶 | KVM 标准版 | 2核2G | 30M / 50M | 34 元 | [查看云服务器档位](https://www.rainyun.com/MzA2MTc2_) |
| 美国洛杉矶 | KVM 中配版 | 2核4G | 40M / 60M | 44 元 | [查看云服务器档位](https://www.rainyun.com/MzA2MTc2_) |
| 美国洛杉矶 | KVM 进阶版 | 4核4G | 60M / 80M | 60 元 | [查看云服务器档位](https://www.rainyun.com/MzA2MTc2_) |
| 美国洛杉矶 | KVM 高配版 | 4核8G | 80M / 100M | 113 元 | [查看云服务器档位](https://www.rainyun.com/MzA2MTc2_) |
| 美国洛杉矶 | KVM 旗舰版 | 8核16G | 100M / 150M | 197 元 | [查看云服务器档位](https://www.rainyun.com/MzA2MTc2_) |
| 中国香港 | KVM 入门版 | 1核1G | 10M / 10M | 28 元 | [查看云服务器档位](https://www.rainyun.com/MzA2MTc2_) |
| 中国香港 | KVM 标配版 | 2核2G | 20M / 20M | 35 元 | [查看云服务器档位](https://www.rainyun.com/MzA2MTc2_) |
| 中国香港 | KVM 中配版 | 2核4G | 20M / 20M | 48 元 | [查看云服务器档位](https://www.rainyun.com/MzA2MTc2_) |
| 中国香港 | KVM 高配版 | 4核4G | 30M / 30M | 68 元 | [查看云服务器档位](https://www.rainyun.com/MzA2MTc2_) |
| 中国香港 | KVM 顶配版 | 4核8G | 30M / 30M | 108 元 | [查看云服务器档位](https://www.rainyun.com/MzA2MTc2_) |
| 中国香港 | KVM 旗舰版 | 8核16G | 50M / 50M | 188 元 | [查看云服务器档位](https://www.rainyun.com/MzA2MTc2_) |

**开饥荒优先选国内机房。** 美国洛杉矶和香港的延迟对国内玩家不友好，那两个档位更适合"顺手建站"或者朋友都在海外的场景。玩家在国内，就老老实实看宁波、温州、镇江、宿迁这几个点。

## 实操：在 Ubuntu 上把服务端装起来

装完系统先用 SSH 连上去，root 登录后依次执行。

### 1. 装 32 位运行库

DST 服务端是 32 位程序，缺了这些库会直接报错退出：

bash
sudo dpkg --add-architecture i386
sudo add-apt-repository multiverse
sudo apt update
sudo apt install -y libstdc++6 libgcc1 libcurl4-gnutls-dev:i386 lib32z1


不同 Ubuntu 版本的包名会变，遇到"无法定位软件包"就把 `lib32gcc1` 换成 `lib32gcc-s1` 再试。

### 2. 装 SteamCMD 并拉取服务端

bash
mkdir -p ~/steamcmd && cd ~/steamcmd
wget https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz
tar -xvzf steamcmd_linux.tar.gz
./steamcmd.sh


进入 `steam>` 提示符后：


force_install_dir /home/dst/dst_server
login anonymous
app_update 343050 validate
quit


`343050` 就是《饥荒联机版》专用服务器在 Steam 上的 App ID，`validate` 会在下载后校验文件完整性——服务端更新后出现各种奇怪报错，通常重跑这一行就能解决。

### 3. 拿服务器令牌

这一步是新手最容易翻车的地方。先登录 Klei 的账号页面，用 Steam 账号授权后进入 Don't Starve Together 的 Game Servers 页面，填一个集群名字，点 Add New Server，页面上会生成一串很长的令牌。

把令牌存成文件：

bash
mkdir -p ~/.klei/DoNotStarveTogether/Cluster_1
echo "你的令牌" > ~/.klei/DoNotStarveTogether/Cluster_1/cluster_token.txt


注意别把 `cluster_token` 这几个字母本身复制进去——网上求助"YOUR SERVER WILL NOT START"的，十有八九是犯了这一条。如果启动时仍然提示缺令牌，再把 `cluster_token.txt` 往 `Master` 和 `Caves` 子目录各复制一份。

### 4. 改配置

集群配置放在 `Cluster_1/cluster.ini`，通常只关心这几行：

ini
[GAMEPLAY]
game_mode = survival        ; survival / wilderness / endless
max_players = 6
pvp = false
pause_when_empty = true     ; 没人时自动暂停

[NETWORK]
cluster_name = MyDediServer
cluster_description = 几个人的饥荒档
cluster_password = 你的密码
cluster_intention = cooperative


`pause_when_empty = true` 建议开着，不然你下线那段时间，熊獾能把家拆干净。

分片配置在各分片目录的 `server.ini` 里，首次启动会自动生成。你基本只需要确认 `server_port`——地上分片默认 10999，并且 `is_master = true`。洞穴分片用另一个端口，具体值看它自己生成的 `server.ini`。

### 5. 放行端口，然后启动

云服务器要在控制台的安全组里放行 **UDP 10999**。如果同时开洞穴，把洞穴分片那个端口也一起放行；NAT 机型还要在后台把这些端口做成映射（雨云游戏云可以开 15 条）。

启动用 `screen`，这样关掉 SSH 窗口服务器也不会停：

bash
cd ~/dst_server/bin
screen -S dst_master ./dontstarve_dedicated_server_nullrenderer -cluster Cluster_1 -shard Master


要开洞穴就再开一个：

bash
screen -S dst_caves ./dontstarve_dedicated_server_nullrenderer -cluster Cluster_1 -shard Caves


看到日志里出现 "Sim Paused"，说明世界生成完了。按 `Ctrl + A` 再按 `D` 把它切到后台。进游戏，在"浏览游戏"里搜集群名字，能搜到带"专用服务器"标记的房间就算成功。

## MOD 上服：两个文件的事

服务端不用你去手动订阅创意工坊。在服务端目录里找到 `dedicated_server_mods_setup.lua`，把要装的 MOD 按 ID 写进去：

lua
ServerModSetup("你的MOD工坊ID")


然后在 `Master/modoverride.lua` 和 `Caves/modoverride.lua` 里把对应 MOD 设为启用。重启服务端，它会自己下载。

有个细节值得记一下：DST 每次大更新，服务端目录里的相关配置可能被覆盖，动手前先把 `dedicated_server_mods_setup.lua` 备份一份，能省下重新翻 MOD 列表的时间。

## 开完之后：更新、崩溃和备份

专用服务器最烦的不是装，是维护。三条经验：

1. **游戏更新后必须同步服务端**。客户端版本一升，玩家就进不来。回到 SteamCMD 重跑一次 `app_update 343050 validate`，然后重启进程。
2. **崩档要能自启**。长时间开服，进程挂掉是常态。可以配一个 systemd 服务，或者用现成的管理脚本。GitHub 上有开源项目 `Linux_DST_SCRIPT`，支持 Ubuntu 和 Debian，能做崩档自启、自动更新服务端和 MOD、每 5 小时备份一次存档、每分钟记录一次在线玩家列表，还带查状态、回档、发公告这些控制台功能。
3. **备份别偷懒**。把 `~/.klei/DoNotStarveTogether` 整个目录定期打包，尤其是准备改存档、上大 MOD 之前。

## 常见问题对照表

| 现象 | 大概率原因 | 处理方式 |
| --- | --- | --- |
| 日志刷 YOUR SERVER WILL NOT START | 令牌没填、填错，或粘贴了 `cluster_token` 字样 | 检查 `cluster_token.txt` 内容，只放令牌字符串 |
| 游戏里搜不到房间 | 安全组没放行 UDP 端口 / NAT 没做映射 | 放行地上和洞穴分片端口，确认映射生效 |
| 进服后频繁掉线 | 上行带宽不够，或多人共用 NAT 被挤 | 升级带宽档位，或换带独立公网 IP 的机型 |
| 存档突然没了 | 崩溃未自动重启，或误操作覆盖 | 从备份目录回滚，给服务端配自启 |
| 开洞穴后变卡 | 地上洞穴抢同一份 CPU 和内存 | 洞穴单独开一台 2核4G |

## 费用怎么压下来

最后说钱。雨云的价格体系里，有几个能叠加的折扣值得留意：注册时通过优惠通道进入账号并绑定微信，可以拿到首月 5 折券；官方本身有年付 7 折、半年 8 折、季付 9 折；积分商城里的新购折扣券在部分情况下还能和年付折扣叠着用。

按这个逻辑算：朋友确定会长期玩、每天晚上都上线，直接年付比月付划算一大截；只是寒假暑假开一阵，那就先月付配 1 元试用，玩完停掉。

👉 [用优惠通道注册雨云账号，先把首月 5 折券领到手](https://www.rainyun.com/MzA2MTc2_)

如果只是想先验证一下"自己开服到底顺不顺"，别急着年付。花 1 块钱试一天，把 SteamCMD、令牌、端口这三关走通，再决定买哪一档——这三关过了，后面的事就都是重复劳动。
