## Mac ATOM私有节点搭建-无docker版

+ bitcoin core全节点
+ nodeJs:https://nodejs.org/en
+ vim或其他的编辑器

### 1. 配置bitcore配置文件

```bash
server=1
txindex=1
daemon=1

# 相当于 `rpcuser=electrumx` 和 `rpcpassword=electrumx`，可以换成自己想配置的用户名和密码，用 [rpcauth.py](https://github.com/bitcoin/bitcoin/blob/master/share/rpcauth/rpcauth.py) 生成
rpcauth=electrumx:c7ed296134ebe0035d9ff786dfa102b5$9d40e8e36bcdba1e3ca0a79178c3864c3deaa9e6fd484ff683e7770690a97097

# 允许本机
rpcallowip=127.0.0.1

# 本地局域网ip：192.168.0.0/16，包含192.168.0.1 - 192.168.254.254
# 公网IP：xxx.xxx.xxx.xxx/32
rpcallowip=192.168.0.0/16

# 本地所有 ip 监听 8332 端口
rpcbind=0.0.0.0
```

### 2. 安装anaconda

+ 下载链接：https://www.anaconda.com/download
+ `anaconda`是`python`的可视化包管理器，对新手友好，一键安装依赖，而且可以隔离`python`环境，不需要的时候直接一键全部删除，无需学会如何清理和卸载`python模块`。

#### 2.1 建立新的`Python`环境

+ `python`版本：取一个自己喜欢的环境名，`python版本`建议选择`3.10`，因为安装会用到`pysha3`模块，但是`pysha3`模块与3.11及以上版本不兼容。

![image-20240107183045863](https://github.com/11001100111011101110001/atomrpc/assets/20775615/6e59c477-29e9-42d4-a704-73a08de174d8)

#### 2.2 安装snappy(1.1.10)、libcxx(14.0.6)、leveldb(1.23)、python-leveldb(0.201)、plyvel(1.5)

这里以leveldb为例：

![image-20240107183900798](https://github.com/11001100111011101110001/atomrpc/assets/20775615/e487332a-de57-4ca6-b766-b81a952b8b78)

#### 2.3 安装python-dotenv

![image-20240107184052314](https://github.com/11001100111011101110001/atomrpc/assets/20775615/2faa7fd3-8249-436d-b7d4-1690f7887f3f)

### 3. 安装electrumx server

#### 3.1 使用新建的Python环境打开终端

![image-20240107184425876](https://github.com/11001100111011101110001/atomrpc/assets/20775615/c2a1f88a-a7c5-4f81-91d7-0e14f5cc117c)

打开后如图所示：

![image-20240107184549635](https://github.com/11001100111011101110001/atomrpc/assets/20775615/be2f5173-da32-405a-98e7-e17b3b1c4c6a)

#### 3.2 安装electrumx server

```shell
# 如果没有特别指定文件夹，代码默认直接下载在当前用户home目录下
# 如果不会用git，可以直接下载到本地解压，https://github.com/atomicals/atomicals-electrumx
git clone git@github.com:atomicals/atomicals-electrumx.git
# 进入代码目录
cd atomicals-electrumx
# 安装
pip install .


# 输出内容大致如下
Processing /Volumes/T7/atomicals-electrumx
  Preparing metadata (setup.py) ... done
Collecting aiorpcX<0.23,>=0.22.0 (from aiorpcX[ws]<0.23,>=0.22.0->e-x==1.16.0)
  Using cached aiorpcX-0.22.1-py3-none-any.whl (36 kB)
Collecting attrs (from e-x==1.16.0)
  Using cached attrs-23.2.0-py3-none-any.whl.metadata (9.5 kB)
Collecting plyvel (from e-x==1.16.0)
  Using cached plyvel-1.5.0.tar.gz (152 kB)
  Preparing metadata (setup.py) ... done
Collecting pylru (from e-x==1.16.0)
  Using cached pylru-1.2.1-py3-none-any.whl (16 kB)
Collecting aiohttp<4,>=3.3 (from e-x==1.16.0)
  Downloading aiohttp-3.9.1-cp38-cp38-macosx_10_9_x86_64.whl.metadata (7.4 kB)
Collecting cbor2 (from e-x==1.16.0)
  Downloading cbor2-5.5.1-cp38-cp38-macosx_10_9_x86_64.whl.metadata (6.0 kB)
Collecting websockets (from e-x==1.16.0)
  Downloading websockets-12.0-cp38-cp38-macosx_10_9_x86_64.whl.metadata (6.6 kB)
Collecting regex (from e-x==1.16.0)
  Downloading regex-2023.12.25-cp38-cp38-macosx_10_9_x86_64.whl.metadata (40 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 40.9/40.9 kB 862.5 kB/s eta 0:00:00
Collecting krock32 (from e-x==1.16.0)
  Using cached krock32-0.1.1-py3-none-any.whl (7.7 kB)
Collecting merkletools (from e-x==1.16.0)
  Using cached merkletools-1.0.3.tar.gz (8.3 kB)
  Preparing metadata (setup.py) ... done
Collecting multidict<7.0,>=4.5 (from aiohttp<4,>=3.3->e-x==1.16.0)
  Downloading multidict-6.0.4-cp38-cp38-macosx_10_9_x86_64.whl (29 kB)
Collecting yarl<2.0,>=1.0 (from aiohttp<4,>=3.3->e-x==1.16.0)
  Downloading yarl-1.9.4-cp38-cp38-macosx_10_9_x86_64.whl.metadata (31 kB)
Collecting frozenlist>=1.1.1 (from aiohttp<4,>=3.3->e-x==1.16.0)
  Downloading frozenlist-1.4.1-cp38-cp38-macosx_10_9_x86_64.whl.metadata (12 kB)
Collecting aiosignal>=1.1.2 (from aiohttp<4,>=3.3->e-x==1.16.0)
  Using cached aiosignal-1.3.1-py3-none-any.whl (7.6 kB)
Collecting async-timeout<5.0,>=4.0 (from aiohttp<4,>=3.3->e-x==1.16.0)
  Using cached async_timeout-4.0.3-py3-none-any.whl.metadata (4.2 kB)
Collecting pysha3>=1.0b1 (from merkletools->e-x==1.16.0)
  Using cached pysha3-1.0.2.tar.gz (829 kB)
  Preparing metadata (setup.py) ... done
Collecting idna>=2.0 (from yarl<2.0,>=1.0->aiohttp<4,>=3.3->e-x==1.16.0)
  Using cached idna-3.6-py3-none-any.whl.metadata (9.9 kB)
Downloading aiohttp-3.9.1-cp38-cp38-macosx_10_9_x86_64.whl (399 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 399.0/399.0 kB 5.9 MB/s eta 0:00:00
Using cached attrs-23.2.0-py3-none-any.whl (60 kB)
Downloading cbor2-5.5.1-cp38-cp38-macosx_10_9_x86_64.whl (63 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 63.2/63.2 kB 1.7 MB/s eta 0:00:00
Downloading regex-2023.12.25-cp38-cp38-macosx_10_9_x86_64.whl (296 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 296.4/296.4 kB 6.4 MB/s eta 0:00:00
Downloading websockets-12.0-cp38-cp38-macosx_10_9_x86_64.whl (121 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 121.3/121.3 kB 4.3 MB/s eta 0:00:00
Using cached async_timeout-4.0.3-py3-none-any.whl (5.7 kB)
Downloading frozenlist-1.4.1-cp38-cp38-macosx_10_9_x86_64.whl (55 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 55.2/55.2 kB 1.1 MB/s eta 0:00:00
Downloading yarl-1.9.4-cp38-cp38-macosx_10_9_x86_64.whl (83 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 83.7/83.7 kB 3.0 MB/s eta 0:00:00
Using cached idna-3.6-py3-none-any.whl (61 kB)
Building wheels for collected packages: e-x, merkletools, plyvel, pysha3
  Building wheel for e-x (setup.py) ... done
  Created wheel for e-x: filename=e_x-1.16.0-py3-none-any.whl size=213140 sha256=af72d690feba468a956cb7e9cdf69b533f46ebc72dcb2178d937c0249d74bad5
  Stored in directory: /Users/yeye/Library/Caches/pip/wheels/0b/21/78/783b6d5e08e142858dd2a30747ece6bbf318ae81afd95d860d
  Building wheel for merkletools (setup.py) ... done
  Created wheel for merkletools: filename=merkletools-1.0.3-py3-none-any.whl size=5880 sha256=13d326be2326e514b0851b32100a6cb3782218c6d050f351a902505994ec57a0
  Stored in directory: /Users/yeye/Library/Caches/pip/wheels/b3/86/d3/3314b01697d92937a60e9d178f19cb640ac92041d3cfd55ae6
  Building wheel for plyvel (setup.py) ... done
  Created wheel for plyvel: filename=plyvel-1.5.0-cp38-cp38-macosx_10_9_x86_64.whl size=92958 sha256=8df7d8b2bd0b409ab2d7c00d7ae9236de0c63c630679f19e948f9f45b8c5e065
  Stored in directory: /Users/yeye/Library/Caches/pip/wheels/5e/e4/91/221f235b34e655cc3b9b7346fae4612fe283eb4f69376e7282
  Building wheel for pysha3 (setup.py) ... done
  Created wheel for pysha3: filename=pysha3-1.0.2-cp38-cp38-macosx_10_9_x86_64.whl size=46878 sha256=c74c5ae1a81a96b1f5496d48d1f12065b30a11a4a3f00dbcf33b2bfd9d583014
  Stored in directory: /Users/yeye/Library/Caches/pip/wheels/ee/35/47/386d23587167a3b03fc702b6be1ff60c92fddbeef125873088
Successfully built e-x merkletools plyvel pysha3
Installing collected packages: pysha3, pylru, plyvel, krock32, websockets, regex, multidict, merkletools, idna, frozenlist, cbor2, attrs, async-timeout, aiorpcX, yarl, aiosignal, aiohttp, e-x
Successfully installed aiohttp-3.9.1 aiorpcX-0.22.1 aiosignal-1.3.1 async-timeout-4.0.3 attrs-23.2.0 cbor2-5.5.1 e-x-1.16.0 frozenlist-1.4.1 idna-3.6 krock32-0.1.1 merkletools-1.0.3 multidict-6.0.4 plyvel-1.5.0 pylru-1.2.1 pysha3-1.0.2 regex-2023.12.25 websockets-12.0 yarl-1.9.
```

#### 3.3 修改host

```shell
# 编辑 /etc/hosts
vim /etc/hosts

# 在最后一行添加 127.0.0.1 localhost
127.0.0.1 localhost
```

#### 3.4 修改配置文件

````shell
# 修改.env
vim .env

# 修改DAEMON_URL，改成bitcore全节点配置里rpcauth的 用户名:密码@localhost:8332/
DAEMON_URL=http://electrumx:electrumx@localhost:8332/

# 修改DB_DIRECTORY，改成放置data数据的绝对路径，建议放在SSD盘上
DB_DIRECTORY=/Volumes/T7/electrumx_data

# 修改env_base
vim electrumx/lib/env_base.py

# 在 from os import environ 一行后边新增以下代码，用于加载配置文件
from dotenv import load_dotenv
load_dotenv()

# 运行electrumx_server
python electrumx_server

# 输出内容大致如下，只要不出现ERROR，问题不大。
INFO:electrumx:ElectrumX server starting
INFO:electrumx:logging level: INFO
INFO:Controller:Python version: 3.10.13 (main, Sep 11 2023, 08:39:02) [Clang 14.0.6 ]
INFO:Controller:software version: ElectrumX 1.16.0
INFO:Controller:aiorpcX version: 0.22.1
INFO:Controller:supported protocol versions: 1.4-1.4.3
INFO:Controller:event loop policy: None
INFO:Controller:reorg limit is 200 blocks
INFO:Daemon:daemon #1 at localhost:8332/ (current)
INFO:DB:switching current directory to /Volumes/T7/electrumx_data
INFO:DB:using leveldb for DB backend
INFO:DB:opened UTXO DB (for sync: True)
INFO:DB:UTXO DB version: 8
INFO:DB:coin: Bitcoin
INFO:DB:network: mainnet
INFO:DB:height: 1,049
INFO:DB:tip: 00000000adcaab3694464b4964c187048235e3548a7a4c9fbfcfdc2379f38823
INFO:DB:tx count: 1,068
INFO:DB:atomical count: 0
INFO:DB:flushing DB cache at 400 MB
INFO:DB:sync time so far: 09m 43s
INFO:History:history DB version: 1
INFO:History:flush count: 5
INFO:SessionManager:RPC server listening on localhost:8000
INFO:Prefetcher:catching up to daemon height 824,721 (823,672 blocks behind)
INFO:BlockProcessor:our height: 1,059 daemon: 824,721 UTXOs 0MB hist 0MB
...
````

### 4. 安装electrumx proxy

```shell
# 安装proxy，如果不会用git，可以直接下载到本地解压，https://github.com/atomicals/electrumx-proxy
git clone git@github.com:atomicals/electrumx-proxy.git
cd electrumx-proxy
npm install
npm run dev

# 输出内容如下
(AtomRPC) yeye@xiexiedeMacBook-Pro electrumx-proxy % npm run dev

> electrumx-proxy@1.2.6 dev
> nodemon src/index.ts

[nodemon] 2.0.22
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: ts,json
[nodemon] starting `ts-node src/index.ts`
UPDATED_PING_TIME 1704622112460
process.env.ELECTRUMX_PORT 50010
process.env.ELECTRUMX_HOST 127.0.0.1
Listening: http://0.0.0.0:8080
...
```

### 5. 查看本地节点状态

访问：http://localhost:8080/proxy/health 即可查看节点状态，当区块同步完成时页面返回如下内容：

```json
{"success":true,"health":true}
```

### 6. 安装`atomicals-js`

```shell
# cd到想要安装的路径
git clone git@github.com:atomicals/atomicals-js.git
cd atomicals-js
yarn install
yarn run build

# 修改配置文件，使用本地节点
vim .env

# 修改ELECTRUMX_PROXY_BASE_URL，保存即可
ELECTRUMX_PROXY_BASE_URL=http://localhost:8080/proxy

# 创建钱包，导入钱包等等操作请查看官方文档，https://github.com/atomicals/atomicals-js
```

## 参考资料

+ NEXTDAO：https://github.com/Next-DAO/atomicals-electrumx-proxy-docker
+ https://www.ordinalsworld.io/p/dockerpythonatom
