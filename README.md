# 使用 Scoop 搭建 Windows 统一开发环境


## 一、科学上网第一步

* 下载安装 [Clash for Windows 汉化版](https://github.com/ender-zhao/Clash-for-Windows_Chinese/releases)


## 二、Scoop 安装及配置

* Scoop 官网: https://scoop.sh/

### 1) 安装 Scoop

```PowerShell
# 在 PowerShell 中打开远程权限
Set-ExecutionPolicy RemoteSigned -scope CurrentUser;

# 下载 Scoop 安装脚本
 irm get.scoop.sh `
    -outfile        'install.ps1' `
    -proxy          'http://127.0.0.1:7890'

# 执行 Scoop 安装脚本
.\install.ps1 `
    -ScoopDir       'D:\scoop'  `
    -ScoopGlobalDir 'D:\scoop\global' `
    -proxy          'http://127.0.0.1:7890' `
    -RunAsAdmin

# 设置 Scoop 代理
# https://github.com/ScoopInstaller/scoop/wiki/Using-Scoop-behind-a-proxy
scoop config proxy 127.0.0.1:7890

# 查看 Scoop 配置
scoop config
# ~\.config\scoop\config.json
```

```json
{
    "lastUpdate": "2022-09-30T15:15:43.6915791+08:00",
    "SCOOP_REPO": "https://github.com/ScoopInstaller/Scoop",
    "SCOOP_BRANCH": "master",
    "aria2-enabled": false,
    "aria2-max-connection-per-server": "16",
    "aria2-split": "16",
    "aria2-min-split-size": "1M",
    "rootPath": "D:\\scoop",
    "globalPath": "D:\\scoop\\global",
    "proxy": "127.0.0.1:7890"
}
```

### 2) 安装必备工具

```PowerShell
scoop install sudo          # 使用管理员权限执行命令
scoop install cacert        # CA根证书
# scoop install openssh     # Win10 系统自带, 无需安装
scoop install git           # Scoop 添加仓库及更新需要使用 git
scoop install aria2         # 下载工具 支持 BT 和磁力链接

scoop install 7zip          # 用于解压 7z、rar、tar.xz 等压缩包
scoop install innounp       # 用于解压 InnoSetup   安装包
scoop install dark          # 用于解压 WiX Toolset 安装包

# 其它常用命令行工具
scoop install wget curl curlie
scoop install grep sed less ccat touch which

# 使用管理员权限激活 “启用 Win32 长路径” 组策略
sudo Set-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem' -Name 'LongPathsEnabled' -Value 1


```

### 3) 添加 Scoop 仓库

```PowerShell
# 添加 extras 仓库
scoop bucket add extras

# 添加 versions 仓库
scoop bucket add versions

# 更新 Scoop 及仓库
scoop update

# 添加第三方仓库
# scoop bucket add dorado https://github.com/chawyehsu/dorado
# scoop install dorado/clash-for-windows

scoop list          # 查看已安装程序
scoop bucket list   # 查看已安装仓库
scoop checkup       # 检查潜在的问题
```


## 三、安装常用软件

### 1) 安装 VSCode

```PowerShell
# 安装 VSCode
scoop install vscode
reg import "D:\scoop\apps\vscode\current\install-associations.reg"
reg import "D:\scoop\apps\vscode\current\install-context.reg"
# 导入注册表
```

### 2) 安装 Sublime Text

```PowerShell
# 安装 Sublime Text 并导入注册表
scoop install sublime-text
reg import "D:\scoop\apps\sublime-text\current\install-context.reg"
# 导入注册表
```

### 3) 安装 openssl

```PowerShell
# scoop install openssl       # 完整版 openssl, 用于开发
scoop install openssl-light   # 精简版 openssl, 用于证书生成、格式转换
```

### 4) 其它常用软件

```PowerShell
scoop install baretail        # 查看日志文件
scoop install ccleaner        # 电脑清理优化大师
scoop install draw.io         # 画图神器
scoop install everything      # 文件搜索神器
scoop install gridea          # 一个小而美的静态博客写作客户端
scoop install switchhosts     # 管理、切换多个 hosts 方案的工具
```


## 四、Node 开发环境安装及配置

* [Node.js](https://nodejs.org/zh-cn/) : 是一个基于 Chrome V8 引擎 的 JavaScript 运行时环境
* [npm](https://www.npmjs.com/) : Node 包管理工具 (Node.js自带)
* [yarn](https://www.yarnpkg.cn/) : 快速、可靠、安全的依赖管理工具
* [pnpm](https://www.pnpm.cn/) : 速度快、节省磁盘空间的软件包管理器
* [volta](https://volta.sh/) : 一站式的JavaScript管理工具 ( 可惜不支持 pnpm )

### 1) 安装 volta node(含npm) pnpm

```PowerShell
scoop install volta         # 通过 scoop 安装 volta
volta install node@16.14.2  # 通过 volta 安装 node(含npm)
scoop install pnpm          # 通过 scoop 安装 pnpm
```

### 2) npm pnpm 配置

```PowerShell
# 设置国内镜像
npm  config set registry "https://repo.huaweicloud.com/repository/npm/"

# 设置 npm 路径
npm  config set cache            "D:\.npm\cache"

# 设置 pnpm 路径
pnpm config set store-dir       "D:\.pnpm-store"
pnpm config set global-bin-dir  "D:\.pnpm\bin"
pnpm config set global-dir      "D:\.pnpm\global"
pnpm config set cache-dir       "D:\.pnpm\cache"
pnpm config set state-dir       "D:\.pnpm\state"

# 查看配置信息
# pnpm 与 npm 共用配置文件: ~\.npmrc
npm  config list
pnpm config list
```

### 3) yarn 安装及配置

```PowerShell
# 通过 volta 安装 yarn
volta install yarn

# 设置国内镜像
yarn config set registry "https://repo.huaweicloud.com/repository/npm/"

# 设置 yarn 路径
yarn config set prefix              "D:\.yarn"
yarn config set cache-folder        "D:\.yarn\cache"
yarn config set yarn-offline-mirror "D:\.yarn\mirror"
yarn config set global-folder       "D:\.yarn\global"
yarn config set link-folder         "D:\.yarn\link"

# 查看配置信息
# yarn 配置文件  ~\.yarnrc
yarn config list
```

### 4) 只通过 volta 安装全局工具

```PowerShell
# 非必要不安装全局包或工具
# 建议只通过 volta 安装全局工具
# 如果需要通过 pnpm 安装全局工具
# 可将该目录添加到环境变量path中: D:\.pnpm\bin

# 查看全局包列表
npm  list -g
pnpm list -g
yarn global list

# 通过 volta 安装全局工具 rimraf
volta install rimraf
# 如果出错, 需启用开发人员模式, 或者使用管理员权限安装
# sudo volta install rimraf

# 查看 volta 安装列表
volta list
volta list --format plain
volta list node
volta list yarn
volta list rimraf
```


## 五、OpenResty 开发环境相关

### 1) 安装 OpenResty

```PowerShell
# 安装 openresty (32位)
scoop install openresty -a 32bit

# 如果需要运行 resty 等命令需要安装 perl
# scoop install perl
```

### 2) 安装 MinGW

```PowerShell
# 安装 mingw (32位)
scoop install mingw -a 32bit
# 编译 clib 需要用到 mingw32-gcc, mingw32-make
```

### 3) 安装 LuaRocks

```PowerShell
# 安装 luarocks (32位)
scoop install luarocks -a 32bit -i
# 使用 -i 参数可避免安装 lua 依赖
```

### 4) 修改 LuaRocks 配置文件

```PowerShell
# 打开 config.lua 配置文件
subl D:\scoop\apps\luarocks\current\config.lua
# 将以下代码复制到该文件并保存
```

```lua
-- luarocks config.lua 配置文件

-- 使用国内镜像
rocks_servers = {
    "https://luarocks.cn",
}

-- 使用代理
proxy = "http://127.0.0.1:7890"

local openresty = "D:/scoop/apps/openresty/current/"
local mingw_bin = "D:/scoop/apps/mingw/current/bin/"

-- 安装路径
rocks_trees = {
    {
        root    = openresty,
        bin_dir = openresty .. "bin",
        lib_dir = openresty .. "clib",
        lua_dir = openresty .. "lua",
    },
}

-- 使用 openresty 提供的 luajit
lua_interpreter = "luajit.exe"
lua_version     = "5.1"
verbose         = false

variables = {
    LUA_BINDIR  = openresty,
    LUA_DIR     = openresty,
    LUALIB      = 'lua51.dll',
    MSVCRT      = 'm',   -- make MinGW use MSVCRT.DLL as runtime
    MAKE        = mingw_bin .. "make.exe",
    CC          = mingw_bin .. "gcc.exe",
    LD          = mingw_bin .. "gcc.exe",
    RC          = mingw_bin .. "windres.exe",
    AR          = mingw_bin .. "ar.exe",
    RANLIB      = mingw_bin .. "ranlib.exe",
}
```

### 5) 使用 LuaRocks 安装 clib

```PowerShell
# 如果下载安装包失败, 可删除缓存目录再试:
# ~\AppData\Local\LuaRocks\Cache

luarocks install LuaFileSystem
luarocks install LuaSocket
luarocks install utf8
luarocks install hashids
```


## 六、代码管理工具及托管

* Git https://git-scm.com/
* TortoiseGit https://tortoisegit.org/
* GitHub - 代码托管 https://github.com/
* Gitee - 代码托管 https://gitee.com/

### 1) 安装 git 并配置代理

```PowerShell
# 通过 scoop 安装 git
scoop install git

# 设置 git 凭证管理器
git config --global credential.helper manager-core

# 设置通过代理访问 github 仓库
git config --global http.https://github.com.proxy socks5://127.0.0.1:7890

# 使用 sublime 打开 .gitconfig 配置文件
cd ~; touch .\.gitconfig; subl  .\.gitconfig
```

### 2) git 配置多个 ssh-key

* git 配置多个 ssh-key
  https://gitee.com/help/articles/4229

```PowerShell
cd ~\.ssh  # 切换目录

# 生成一个 github 用的 ssh-key
ssh-keygen -t rsa -C 'ssh-key@github' -f github_id_rsa

# 生成一个 gitee 用的 ssh-key
ssh-keygen -t rsa -C 'ssh-key@gitee' -f gitee_id_rsa

# 查看公钥, 并添加到 github
# https://github.com/settings/keys
more github_id_rsa.pub

# 查看公钥, 并添加到 gitee
# https://gitee.com/profile/sshkeys
more gitee_id_rsa.pub

# 使用 sublime 打开 ~\.ssh\config 配置文件
touch .\config; subl .\config
# 并将以下内容复制到该文件并保存
```

* ~\\.ssh\\config 配置文件

```sh
# 以下为 ~\.ssh\config 配置文件内容

# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/github_id_rsa

# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/gitee_id_rsa
```

## 七、其它配置

### 1) 安装 ColorTool 并设置 PowerShell 配色方案

```PowerShell
# 告别 Windows 终端的难看难用, 从改造 PowerShell 的外观开始
# https://sspai.com/post/52868

# 通过 scoop 安装 colortool
scoop install colortool

# 查看配色方案
colortool -s

# 设置 cmd 默认配色方案
colortool -d OneHalfDark.itermcolors

# 设置 PowerShell 配色
colortool OneHalfDark.itermcolors
# 右击 PowerShell 的标题栏, 点击属性, 然后查看配色标签页, 最后点击确定
```

### 2) curl 安装及配置

* curl 安装

```PowerShell
# 通过 scoop 安装 curl curlie
# scoop install curl curlie

# 注意: 在 PowerShell 中, curl 可能是 Invoke-WebRequest 的别名
# Get-Command -All curl
# 如果要使用安装的 curl, 可使用 curl.exe 代替

# 使用 sublime 打开 .curlrc 配置文件
cd ~; touch .\.curlrc; subl  .\.curlrc
# 并将以下内容复制到该文件并保存
```

* ~\\.curlrc 配置文件

```ini
# Using cURL with a proxy
# https://www.scrapingbee.com/blog/curl-proxy/

# 设置代理
proxy = http://127.0.0.1:7890
```

### 3) wget 安装及配置

* wget 安装

```PowerShell
# 通过 scoop 安装 cacert wget
# scoop install cacert wget

# 注意: 在 PowerShell 中, wget 可能是 Invoke-WebRequest 的别名
# Get-Command -All wget
# 如果要使用安装的 wget, 可使用 wget.exe 代替

# 使用 sublime 打开 wget.ini 配置文件
cd D:\scoop\apps\wget\current\; touch .\wget.ini; subl .\wget.ini
# 并将以下内容复制到该文件并保存
```

* wget.ini 或 ~\\.wgetrc 配置文件

```ini
# Using wget with a proxy
# https://www.scrapingbee.com/blog/wget-proxy/

# 设置代理
https_proxy     = http://127.0.0.1:7890
http_proxy      = http://127.0.0.1:7890
ftp_proxy       = http://127.0.0.1:7890

# 设置证书
ca_certificate  = D:\scoop\apps\cacert\current\cacert.pem
```


## 参考文章

* [搭建 Windows 统一开发环境（Scoop）](https://zhuanlan.zhihu.com/p/128955118)
* [Scoop——也许是Windows平台最好用的软件（包）管理器](https://zhuanlan.zhihu.com/p/463284082)
* [win10怎么样打开开发者选项启用开发人员模式](https://jingyan.baidu.com/article/0a52e3f4e4bea2bf62ed72da.html)
