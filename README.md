## Telegram messenger CLI [![Build Status](https://travis-ci.org/vysheng/tg.png)](https://travis-ci.org/vysheng/tg)

Command-line interface for [Telegram](http://telegram.org). Uses readline interface.

### Project Structure (New)

We have reorganized the project structure to make it more maintainable and easier to understand:

```
telegram-cli/
├── bin/                  # 编译后的二进制文件目录
├── src/                  # 源代码主目录
│   ├── core/             # 核心功能模块
│   │   ├── network/      # 网络通信相关
│   │   ├── crypto/       # 加密功能相关
│   │   └── storage/      # 数据存储相关
│   ├── interface/        # 用户界面相关
│   │   ├── cli/          # 命令行界面
│   │   └── tui/          # 文本用户界面
│   ├── modules/          # 功能模块
│   │   ├── messages/     # 消息处理
│   │   ├── media/        # 多媒体处理
│   │   ├── contacts/     # 联系人管理
│   │   └── chats/        # 群聊功能
│   └── bindings/         # 语言绑定
│       ├── lua/          # Lua绑定
│       └── python/       # Python绑定
├── lib/                  # 第三方库和依赖
│   └── tgl/              # Telegram库
├── include/              # 头文件
├── docs/                 # 文档
│   ├── api/              # API文档
│   ├── usage/            # 使用说明
│   └── development/      # 开发指南
├── examples/             # 示例代码
├── scripts/              # 各种脚本
│   ├── build/            # 编译脚本
│   └── utils/            # 实用工具脚本
├── tests/                # 测试代码
├── packaging/            # 打包相关
│   ├── debian/           # Debian打包
│   ├── rpm/              # RPM打包
│   └── gentoo/           # Gentoo打包
└── config/               # 配置文件目录
    └── samples/          # 配置样例
```

### API, Protocol documentation

Documentation for Telegram API is available here: http://core.telegram.org/api

Documentation for MTproto protocol is available here: http://core.telegram.org/mtproto

### Upgrading to version 1.0

First of all, the binary is now in ./bin folder and is named telegram-cli. So be careful, not to use old binary.

Second, config folder is now ${HOME}/.telegram-cli

Third, database is not compatible with older versions, so you'll have to login again.

Fourth, in peer_name '#' are substitued to '@'. (Not applied to appending of '#%d' in case of two peers having same name).

### Installation

Clone GitHub Repository

     git clone --recursive https://github.com/vysheng/tg.git && cd tg

### Python Support

Python support is currently limited to Python 2.7 or Python 3.1+. Other versions may work but are not tested.

#### Linux and BSDs

Install libs: readline, openssl and (if you want to use config) libconfig, liblua, python and libjansson.
If you do not want to use them pass options --disable-libconfig, --disable-liblua, --disable-python and --disable-json respectively.

On Ubuntu/Debian use: 

     sudo apt-get install libreadline-dev libconfig-dev libssl-dev lua5.2 liblua5.2-dev libevent-dev libjansson-dev libpython-dev make 

On gentoo:

     sudo emerge -av sys-libs/readline dev-libs/libconfig dev-libs/openssl dev-lang/lua dev-libs/libevent dev-libs/jansson dev-lang/python

On Fedora:

     sudo dnf install lua-devel openssl-devel libconfig-devel readline-devel libevent-devel libjansson-devel python-devel

On Archlinux:

     yaourt -S telegram-cli-git

On FreeBSD:

     pkg install libconfig libexecinfo lua52 python

On OpenBSD:

     pkg_add libconfig libexecinfo lua python

On openSUSE:

     sudo zypper in lua-devel libconfig-devel readline-devel libevent-devel libjansson-devel python-devel libopenssl-devel

Then,

     ./configure
     make

#### Other methods to install on linux

On Gentoo: use ebuild provided.

On Arch: https://aur.archlinux.org/packages/telegram-cli-git

#### Mac OS X

The client depends on [readline library](http://cnswww.cns.cwru.edu/php/chet/readline/rltop.html) and [libconfig](http://www.hyperrealm.com/libconfig/), which are not included in OS X by default. You have to install these libraries manually.

If using [Homebrew](http://brew.sh/):

     brew install libconfig readline lua python libevent jansson
     export CFLAGS="-I/usr/local/include -I/usr/local/Cellar/readline/6.3.8/include"
     export LDFLAGS="-L/usr/local/lib -L/usr/local/Cellar/readline/6.3.8/lib"
     ./configure && make

Thanks to [@jfontan](https://github.com/vysheng/tg/issues/3#issuecomment-28293731) for this solution.

If using [MacPorts](https://www.macports.org):
     
     sudo port install libconfig-hr
     sudo port install readline
     sudo port install lua51
     sudo port install python34
     sudo port install libevent
     export CFLAGS="-I/usr/local/include -I/opt/local/include -I/opt/local/include/lua-5.1"
     export LDFLAGS="-L/usr/local/lib -L/opt/local/lib -L/opt/local/lib/lua-5.1"
     ./configure && make

Install these ports:

* devel/libconfig
* devel/libexecinfo
* lang/lua52

Then build:

     env CC=clang CFLAGS=-I/usr/local/include LDFLAGS=-L/usr/local/lib LUA=/usr/local/bin/lua52 LUA_INCLUDE=-I/usr/local/include/lua52 LUA_LIB=-llua-5.2 ./configure
     make

#### Other UNIX

If you manage to launch it on other UNIX, please let me know.

### Contacts 
If you would like to ask a question, you can write to my telegram or to the github (or both). To contact me via telegram, you should use import_card method with argument 000653bf:0738ca5d:5521fbac:29246815:a27d0cda


### Usage

    bin/telegram-cli -k <public-server-key>
    
By default, the public key is stored in tg-server.pub in the same folder or in /etc/telegram-cli/server.pub. If not, specify where to find it:

    bin/telegram-cli -k tg-server.pub

Client support TAB completion and command history.

Peer refers to the name of the contact or dialog and can be accessed by TAB completion.
For user contacts peer name is Name <underscore> Lastname with all spaces changed to underscores.
For chats it is it's title with all spaces changed to underscores
For encrypted chats it is <Exсlamation mark> <underscore> Name <underscore> Lastname with all spaces changed to underscores. 

If two or more peers have same name, <sharp>number is appended to the name. (for example A_B, A_B#1, A_B#2 and so on)
  
### Supported commands

#### Messaging

* **msg** \<peer\> Text - sends message to this peer
* **fwd** \<user\> \<msg-seqno\> - forward message to user. You can see message numbers starting client with -N
* **chat_with_peer** \<peer\> starts one on one chat session with this peer. /exit or /quit to end this mode.
* **add_contact** \<phone-number\> \<first-name\> \<last-name\> - tries to add contact to contact-list by phone
* **rename_contact** \<user\> \<first-name\> \<last-name\> - tries to rename contact. If you have another device it will be a fight
* **mark_read** \<peer\> - mark read all received messages with peer
* **delete_msg** \<msg-seqno\> - deletes message (not completly, though)
* **restore_msg** \<msg-seqno\> - restores delete message. Impossible for secret chats. Only possible short time (one hour, I think) after deletion

#### Multimedia

* **send_photo** \<peer\> \<photo-file-name\> - sends photo to peer
* **send_video** \<peer\> \<video-file-name\> - sends video to peer
* **send_text** \<peer\> \<text-file-name> - sends text file as plain messages
* **load_photo**/load_video/load_video_thumb/load_audio/load_document/load_document_thumb \<msg-seqno\> - loads photo/video/audio/document to download dir
* **view_photo**/view_video/view_video_thumb/view_audio/view_document/view_document_thumb \<msg-seqno\> - loads photo/video to download dir and starts system default viewer
* **fwd_media** \<msg-seqno\> send media in your message. Use this to prevent sharing info about author of media (though, it is possible to determine user_id from media itself, it is not possible get access_hash of this user)
* **set_profile_photo** \<photo-file-name\> - sets userpic. Photo should be square, or server will cut biggest central square part


#### Group chat options

* **chat_info** \<chat\> - prints info about chat
* **chat_add_user** \<chat\> \<user\> - add user to chat
* **chat_del_user** \<chat\> \<user\> - remove user from chat
* **rename_chat** \<chat\> \<new-name\>
* **create_group_chat** \<chat topic\> \<user1\> \<user2\> \<user3\> ... - creates a groupchat with users, use chat_add_user to add more users
* **chat_set_photo** \<chat\> \<photo-file-name\> - sets group chat photo. Same limits as for profile photos.

#### Search

* **search** \<peer\> pattern - searches pattern in messages with peer
* **global_search** pattern - searches pattern in all messages

#### Secret chat

* **create_secret_chat** \<user\> - creates secret chat with this user
* **visualize_key** \<secret_chat\> - prints visualization of encryption key. You should compare it to your partner's one
* **set_ttl** \<secret_chat\> \<ttl\> - sets ttl to secret chat. Though client does ignore it, client on other end can make use of it
* **accept_secret_chat** \<secret_chat\> - manually accept secret chat (only useful when starting with -E key)

#### Stats and various info

* **user_info** \<user\> - prints info about user
* **history** \<peer\> [limit] - prints history (and marks it as read). Default limit = 40
* **dialog_list** - prints info about your dialogs
* **contact_list** - prints info about users in your contact list
* **suggested_contacts** - print info about contacts, you have max common friends
* **stats** - just for debugging
* **show_license** - prints contents of GPLv2
* **help** - prints this help
* **get_self** - get our user info

#### Card
* **export_card** - print your 'card' that anyone can later use to import your contact
* **import_card** \<card\> - gets user by card. You can write messages to him after that.

#### Other
* **quit** - quit
* **safe_quit** - wait for all queries to end then quit

---

# Telegram Messenger CLI

## 项目介绍

Telegram Messenger CLI 是一个命令行界面的 [Telegram](http://telegram.org) 客户端，使用readline接口。

## 项目结构（新）

我们已经重新组织了项目结构，使其更易于维护和理解：

```
telegram-cli/
├── bin/                  # 编译后的二进制文件目录
├── src/                  # 源代码主目录
│   ├── core/             # 核心功能模块
│   │   ├── network/      # 网络通信相关
│   │   ├── crypto/       # 加密功能相关
│   │   └── storage/      # 数据存储相关
│   ├── interface/        # 用户界面相关
│   │   ├── cli/          # 命令行界面
│   │   └── tui/          # 文本用户界面
│   ├── modules/          # 功能模块
│   │   ├── messages/     # 消息处理
│   │   ├── media/        # 多媒体处理
│   │   ├── contacts/     # 联系人管理
│   │   └── chats/        # 群聊功能
│   └── bindings/         # 语言绑定
│       ├── lua/          # Lua绑定
│       └── python/       # Python绑定
├── lib/                  # 第三方库和依赖
│   └── tgl/              # Telegram库
├── include/              # 头文件
├── docs/                 # 文档
│   ├── api/              # API文档
│   ├── usage/            # 使用说明
│   └── development/      # 开发指南
├── examples/             # 示例代码
├── scripts/              # 各种脚本
│   ├── build/            # 编译脚本
│   └── utils/            # 实用工具脚本
├── tests/                # 测试代码
├── packaging/            # 打包相关
│   ├── debian/           # Debian打包
│   ├── rpm/              # RPM打包
│   └── gentoo/           # Gentoo打包
└── config/               # 配置文件目录
    └── samples/          # 配置样例
```

## API与协议文档

Telegram API文档：http://core.telegram.org/api

MTproto协议文档：http://core.telegram.org/mtproto

## 升级到1.0版本

1. 二进制文件现在位于./bin文件夹中，名为telegram-cli。请注意不要使用旧的二进制文件。
2. 配置文件夹现在是${HOME}/.telegram-cli
3. 数据库与旧版本不兼容，因此您需要重新登录。
4. 在peer_name中，'#'被替换为'@'。（不适用于两个联系人具有相同名称时附加的'#%d'）。

## 安装

克隆GitHub仓库：

     git clone --recursive https://github.com/vysheng/tg.git && cd tg

## Python支持

Python支持目前仅限于Python 2.7或Python 3.1+。其他版本可能可以工作，但未经测试。

### Linux和BSD系统

安装库：readline、openssl以及（如果您想使用配置）libconfig、liblua、python和libjansson。
如果您不想使用它们，请分别传递选项--disable-libconfig、--disable-liblua、--disable-python和--disable-json。

在Ubuntu/Debian上使用：

     sudo apt-get install libreadline-dev libconfig-dev libssl-dev lua5.2 liblua5.2-dev libevent-dev libjansson-dev libpython-dev make 

在Gentoo上：

     sudo emerge -av sys-libs/readline dev-libs/libconfig dev-libs/openssl dev-lang/lua dev-libs/libevent dev-libs/jansson dev-lang/python

在Fedora上：

     sudo dnf install lua-devel openssl-devel libconfig-devel readline-devel libevent-devel libjansson-devel python-devel

在Archlinux上：

     yaourt -S telegram-cli-git

在FreeBSD上：

     pkg install libconfig libexecinfo lua52 python

在OpenBSD上：

     pkg_add libconfig libexecinfo lua python

在openSUSE上：

     sudo zypper in lua-devel libconfig-devel readline-devel libevent-devel libjansson-devel python-devel libopenssl-devel

然后，

     ./configure
     make

### 在Linux上安装的其他方法

在Gentoo上：使用提供的ebuild。

在Arch上：https://aur.archlinux.org/packages/telegram-cli-git

### Mac OS X

客户端依赖于[readline库](http://cnswww.cns.cwru.edu/php/chet/readline/rltop.html)和[libconfig](http://www.hyperrealm.com/libconfig/)，OS X默认不包含这些库。您必须手动安装这些库。

如果使用[Homebrew](http://brew.sh/)：

     brew install libconfig readline lua python libevent jansson
     export CFLAGS="-I/usr/local/include -I/usr/local/Cellar/readline/6.3.8/include"
     export LDFLAGS="-L/usr/local/lib -L/usr/local/Cellar/readline/6.3.8/lib"
     ./configure && make

感谢[@jfontan](https://github.com/vysheng/tg/issues/3#issuecomment-28293731)提供此解决方案。

如果使用[MacPorts](https://www.macports.org)：
     
     sudo port install libconfig-hr
     sudo port install readline
     sudo port install lua51
     sudo port install python34
     sudo port install libevent
     export CFLAGS="-I/usr/local/include -I/opt/local/include -I/opt/local/include/lua-5.1"
     export LDFLAGS="-L/usr/local/lib -L/opt/local/lib -L/opt/local/lib/lua-5.1"
     ./configure && make

安装这些端口：

* devel/libconfig
* devel/libexecinfo
* lang/lua52

然后构建：

     env CC=clang CFLAGS=-I/usr/local/include LDFLAGS=-L/usr/local/lib LUA=/usr/local/bin/lua52 LUA_INCLUDE=-I/usr/local/include/lua52 LUA_LIB=-llua-5.2 ./configure
     make

### 其他UNIX系统

如果您成功在其他UNIX系统上启动它，请告知我。

## 联系方式

如果您想咨询问题，可以通过Telegram或GitHub（或两者）联系我。要通过Telegram联系我，您应该使用import_card方法，参数为000653bf:0738ca5d:5521fbac:29246815:a27d0cda

## 使用方法

    bin/telegram-cli -k <公共服务器密钥>
    
默认情况下，公钥存储在同一文件夹中的tg-server.pub或/etc/telegram-cli/server.pub中。如果不是，请指定在哪里找到它：

    bin/telegram-cli -k tg-server.pub

客户端支持TAB补全和命令历史记录。

对等方指的是联系人或对话的名称，可以通过TAB补全访问。
对于用户联系人，对等方名称是名字_姓氏，所有空格都改为下划线。
对于聊天，它是其标题，所有空格都改为下划线。
对于加密聊天，它是<感叹号>_名字_姓氏，所有空格都改为下划线。

如果两个或更多对等方有相同的名称，会在名称后附加<尖号>数字。（例如A_B、A_B#1、A_B#2等）

## 支持的命令

### 消息

* **msg** \<peer\> Text - 向该对等方发送消息
* **fwd** \<user\> \<msg-seqno\> - 将消息转发给用户。您可以使用-N启动客户端来查看消息编号
* **chat_with_peer** \<peer\> 开始与此对等方的一对一聊天会话。/exit或/quit结束此模式。
* **add_contact** \<phone-number\> \<first-name\> \<last-name\> - 尝试通过电话号码将联系人添加到联系人列表
* **rename_contact** \<user\> \<first-name\> \<last-name\> - 尝试重命名联系人。如果您有另一个设备，可能会产生冲突
* **mark_read** \<peer\> - 标记已读与对等方的所有接收消息
* **delete_msg** \<msg-seqno\> - 删除消息（虽然不完全删除）
* **restore_msg** \<msg-seqno\> - 恢复已删除消息。对于加密聊天无法使用。只能在删除后短时间内（我认为是一小时）使用

### 多媒体

* **send_photo** \<peer\> \<photo-file-name\> - 向对等方发送照片
* **send_video** \<peer\> \<video-file-name\> - 向对等方发送视频
* **send_text** \<peer\> \<text-file-name> - 以纯文本消息形式发送文本文件
* **load_photo**/load_video/load_video_thumb/load_audio/load_document/load_document_thumb \<msg-seqno\> - 将照片/视频/音频/文档加载到下载目录
* **view_photo**/view_video/view_video_thumb/view_audio/view_document/view_document_thumb \<msg-seqno\> - 将照片/视频加载到下载目录并启动系统默认查看器
* **fwd_media** \<msg-seqno\> 在您的消息中发送媒体。使用此功能可以防止共享媒体作者的信息（尽管可以从媒体本身确定user_id，但无法获取该用户的access_hash）
* **set_profile_photo** \<photo-file-name\> - 设置用户头像。照片应该是正方形的，否则服务器会裁剪最大的中心正方形部分

### 群聊选项

* **chat_info** \<chat\> - 打印关于聊天的信息
* **chat_add_user** \<chat\> \<user\> - 将用户添加到聊天
* **chat_del_user** \<chat\> \<user\> - 从聊天中删除用户
* **rename_chat** \<chat\> \<new-name\>
* **create_group_chat** \<chat topic\> \<user1\> \<user2\> \<user3\> ... - 创建一个与用户的群聊，使用chat_add_user添加更多用户
* **chat_set_photo** \<chat\> \<photo-file-name\> - 设置群聊照片。与个人资料照片相同的限制。

### 搜索

* **search** \<peer\> pattern - 在与对等方的消息中搜索模式
* **global_search** pattern - 在所有消息中搜索模式

### 加密聊天

* **create_secret_chat** \<user\> - 与该用户创建加密聊天
* **visualize_key** \<secret_chat\> - 打印加密密钥的可视化。您应该将其与您伙伴的进行比较
* **set_ttl** \<secret_chat\> \<ttl\> - 为加密聊天设置ttl。虽然客户端会忽略它，但另一端的客户端可以使用它
* **accept_secret_chat** \<secret_chat\> - 手动接受加密聊天（仅在使用-E键启动时有用）

### 统计信息和各种信息

* **user_info** \<user\> - 打印有关用户的信息
* **history** \<peer\> [limit] - 打印历史记录（并标记为已读）。默认限制= 40
* **dialog_list** - 打印有关您对话的信息
* **contact_list** - 打印有关联系人列表中用户的信息
* **suggested_contacts** - 打印有关您共同好友最多的联系人的信息
* **stats** - 仅用于调试
* **show_license** - 打印GPLv2的内容
* **help** - 打印此帮助
* **get_self** - 获取我们的用户信息

### 卡片
* **export_card** - 打印您的"卡片"，任何人以后都可以使用它导入您的联系人
* **import_card** \<card\> - 通过卡片获取用户。之后您可以向他发送消息。

### 其他
* **quit** - 退出
* **safe_quit** - 等待所有查询结束然后退出
