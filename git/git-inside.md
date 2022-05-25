# git-inside

git 内部原理

## `.git` 目录

```
├── *config               配置信息(比如本地repo是否大小写敏感, remote端repo的url, 用户名邮箱等) 
├── description           无需关心（仅供GitWeb程序使用）
├── *HEAD                 目前被检出的分支
├── index                 保存暂存区信息
│
│
├── hooks/                钩子脚本（执行Git命令时自动执行一些特定操作）
├── info/                 包含一个全局性排除文件
│   └── exclude           放置不希望被记录在 .gitignore 文件中的忽略模式
├── logs/                 记录所有操作
├── *objects/             存储所有数据内容
│   ├── SHA-1/            保存git对象的目录（三类对象commit, tree和blob）
│   ├── info/
│   └── pack/             
└── *refs/                存储指向数据（分支、远程仓库和标签等）的提交对象的指针
    ├── heads/           
    ├── remotes/         
    └── tags/            
```

## `object` 目录

```
.git/objects
├── 40
│   └── fa006a9f641b977fc7b3b5accb0171993a3d31
├── 5b
│   └── dcfc19f119febc749eef9a9551bc335cb965e2
├── 67
│   └── f0856ccd04627766ca251e5156eb391a4a4ff8
├── 87
│   └── db2fdb5f60f9a453830eb2ec3cf50fea3782a6
├── ac
│   └── f63c316ad27e8320a23da194e61f45be040b0b
├── info
└── pack
```

### 对象

blob, tree, commit 三种对象：

![img](https://github.com/datawhalechina/faster-git/raw/main/lecture05/imgs/objects_1.jpg)

对象->哈希值：

- 前两位为子目录
- 余下的字符为文件名

`git cat-file -t <hash>` 查看文件类型

Git最初向磁盘存储对象时采用"松散"对象格式；但为了节省空间和提高效率，Git会时不时将多个对象打包成一个称为"包文件"。

`git gc` 可以打包存入`pack`文件夹

### `refs` 引用

用文件名替代SHA值，引用类型有`heads（提交）`, `remotes（远程）`, `tags（标签）`

#### HEAD

本地分支的最后一次提交，多少个分支就有多少的Heads

#### REMOTES

远程分支的最后一次提交，只读

#### TAGS

指向某个commit或其他任何类型

#### STASH

工作区缓存

## config

### 引用规范

```
[remote "origin"]
	url = https://github.com/datawhalechina/faster-git.git
	fetch = +refs/heads/*:refs/remotes/origin/*
```

`git remote add origin`

- 获取远端 `refs/heads/` 下的所有引用
- 将其写入本地的 `refs/remotes/origin/` 中
- 更新本地的 `.git/config` 文件

### 环境变量

`--system` / `--global` / `--local`

系统 / 用户 / 项目

`git config --list`