# MongoDB 的安装

当前脚本是为了在测试环境中，快速安装实例使用，请不要应用于生产环境；在测试环境中，如果需要保留数据，请谨慎使用脚本中的 `uninstall` 操作；[点此](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/)查看官方安装手册。

### 操作过程

#### 准备工作

##### 克隆脚本

如果没有 `git`、`tree`、`zip` 等工具，请先安装。

```bash
[fsyyft@kvm-centos7-mongodb tmp]# /usr/bin/yum -y install git
[fsyyft@kvm-centos7-mongodb tmp]# /usr/bin/yum -y install tree
[fsyyft@kvm-centos7-mongodb tmp]# /usr/bin/yum -y install zip
```

```bash
[fsyyft@kvm-centos7-mongodb tmp]# cd /tmp/
[fsyyft@kvm-centos7-mongodb tmp]# /usr/bin/rm -rf ./deploy
[fsyyft@kvm-centos7-mongodb tmp]# /usr/bin/git clone https://github.com/fsyyft/deploy
```

##### 软件安装

```bash
[fsyyft@kvm-centos7-mongodb tmp]# cd ./deploy/data/shell/
[fsyyft@kvm-centos7-mongodb shell]# /usr/bin/chmod a+x ./deploy 
[fsyyft@kvm-centos7-mongodb shell]# ./deploy yum_install_mongodb
```

#### 实例安装

使用 `/data/database/mongodb/deploy` 脚本进行安装，直接运行脚本可以看到相关帮助信息。

```bash
[fsyyft@kvm-centos7-mongodb tmp]# cd /data/database/mongodb/
[fsyyft@kvm-centos7-mongodb mongodb]# ./deploy 
安装单实例：
    /data/database/mongodb/deploy install {端口号} single
    /data/database/mongodb/deploy install 2741 single
安装配置集群（不包含配置）：
    /data/database/mongodb/deploy install {端口号} config {集群名称}
    /data/database/mongodb/deploy install 2701 config configs
安装数据集群（不包含配置）：
    /data/database/mongodb/deploy install {端口号} shard {集群名称}
    /data/database/mongodb/deploy install 2711 shard shard1
安装路由（不包含配置）：
    /data/database/mongodb/deploy install {端口号} mongos {配置集群信息}
    /data/database/mongodb/deploy install 2700 mongos configs/127.0.0.1:2701,127.0.0.1:2702,127.0.0.1:2703
卸载实例（包含删除数据）：
    /data/database/mongodb/deploy uninstall {端口号}
    /data/database/mongodb/deploy uninstall 2742
```

##### 安装一个单实例

使用脚本进行安装。

```bash
[fsyyft@kvm-centos7-mongodb mongodb]# ./deploy install 2741 single
```

进行简单的测试。

```bash
[fsyyft@kvm-centos7-mongodb mongodb]# /usr/bin/mongo --port 2741
MongoDB shell version v4.0.4
connecting to: mongodb://127.0.0.1:2741/
> use test;
switched to db test
> db.test.insert({"test": "hello"});
WriteResult({ "nInserted" : 1 })
> db.test.find();
{ "_id" : ObjectId("5bff6302b63b8c3737230de9"), "test" : "hello" }
> exit
bye
```

实例的目录结构如下：

```bash
[fsyyft@kvm-centos7-mongodb mongodb]# /usr/bin/tree -ug -L 3 ./2741
./2741
├── [mongod   mongod  ]  conf
│   └── [mongod   mongod  ]  mongodb.2741.conf
├── [mongod   mongod  ]  data
│   ├── [mongod   mongod  ]  admin
│   │   ├── [mongod   mongod  ]  collection
│   │   └── [mongod   mongod  ]  index
│   ├── [mongod   mongod  ]  config
│   │   ├── [mongod   mongod  ]  collection
│   │   └── [mongod   mongod  ]  index
│   ├── [mongod   mongod  ]  diagnostic.data
│   │   └── [mongod   mongod  ]  metrics.2018-11-29T03-52-25Z-00000
│   ├── [mongod   mongod  ]  journal
│   │   ├── [mongod   mongod  ]  WiredTigerLog.0000000001
│   │   ├── [mongod   mongod  ]  WiredTigerPreplog.0000000001
│   │   └── [mongod   mongod  ]  WiredTigerPreplog.0000000002
│   ├── [mongod   mongod  ]  local
│   │   ├── [mongod   mongod  ]  collection
│   │   └── [mongod   mongod  ]  index
│   ├── [mongod   mongod  ]  _mdb_catalog.wt
│   ├── [mongod   mongod  ]  mongod.lock
│   ├── [mongod   mongod  ]  sizeStorer.wt
│   ├── [mongod   mongod  ]  storage.bson
│   ├── [mongod   mongod  ]  test
│   │   ├── [mongod   mongod  ]  collection
│   │   └── [mongod   mongod  ]  index
│   ├── [mongod   mongod  ]  WiredTiger
│   ├── [mongod   mongod  ]  WiredTigerLAS.wt
│   ├── [mongod   mongod  ]  WiredTiger.lock
│   ├── [mongod   mongod  ]  WiredTiger.turtle
│   └── [mongod   mongod  ]  WiredTiger.wt
├── [mongod   mongod  ]  logs
│   └── [mongod   mongod  ]  mongodb.2741.log
└── [mongod   mongod  ]  tmp
    ├── [mongod   mongod  ]  mongodb.2741.pid
    └── [mongod   mongod  ]  mongodb-2741.sock
```

进行实例删除。

```bash
[fsyyft@kvm-centos7-mongodb mongodb]# ./deploy uninstall 2741
```