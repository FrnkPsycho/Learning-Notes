# git-flow

| 分⽀名称                  | 作⽤                                   | ⽣命周期               | 提交or合并                                                   | 起⽌点                                                 |
| ------------------------- | -------------------------------------- | :--------------------- | ------------------------------------------------------------ | ------------------------------------------------------ |
| Feature分⽀               | ⽤于某个功能                           | 临时分⽀、开发 阶段    | 可提交代码                                                   | 由Develop分⽀产⽣， 最终合并到Develop分⽀              |
| Develop分⽀               | 记录历史开发功能                       | 贯穿整个项⽬           | 不能提交，由Feature分 ⽀、Bugfix分⽀、Release 分⽀、Hotfix分⽀合并代码 | 整个项⽬                                               |
| Release分⽀               | ⽤于本次Release 如⽂档、测试、 bug修复 | 临时分⽀、发版阶段     | 可提交代码                                                   | 由Develop分⽀产⽣， 最终合并到Develop 分⽀和Master分支 |
| Hotfix分⽀                | ⽤于解决线上bug                        | 临时分⽀、紧急修复阶段 | 可提交代码                                                   | 由Master分⽀产⽣， 最终合并到Develop 分⽀和Master分支  |
| Master（Production） 分⽀ | 记录历史发布版本                       | 贯穿整个项⽬           | 不能提交，由Release、Hotfix分⽀合并代码                      | 整个项⽬                                               |

Commit标准：

[https://www.conventionalcommits.org/zh-hans/v1.0.0/](https://www.conventionalcommits.org/zh-hans/v1.0.0/)

## Develop阶段

- 创建新功能Feature分支，起名`feature-<name>`

    `git checkout -b feature-xxx develop`

- 加入功能commit message采用`feat: `，bug修复用`fix`，紧急修复`hotfix`

- 并入develop分支，删除功能分支

    ```sh
    git checkout develop
    git merge --no-ff feature-xxx
    git branch -d feature-xxx
    ```

## Release阶段

- 创建发布Release分支，起名`release-<version>`

- 再进行bug修复

- 并入master分支，删除release分支

    ```sh
    git checkout master
    git merge --no-ff release-v0.1
    git branch -d release-v0.1
    ```

## Hotfix情况

- 创建hotfix分支
- 修复bug
- 并入master分支和develop分支



## commit 规范

![git commit](https://github.com/datawhalechina/faster-git/raw/main/lecture07/git-commit.png)

- commit message - 提交的内容相关描述
- author & committer - 作者及提交者
- changed files - 修改的文件
- hash & parent - 提交内容的hash及在提交树上的位置



```
<type>(<scope>): <short summary>
  │       │             │
  │       │             └─⫸ Summary in present tense. Not capitalized. No period at the end.
  │       │
  │       └─⫸ Commit Scope: animations|bazel|benchpress|common|compiler|compiler-cli|core...
  │
  └─⫸ Commit Type: build|ci|docs|feat|fix|perf|refactor|test
```

### 自动校验规范

`.git/hooks` 中`prepare-commit-msg` 及 `commit-msg` 的sample后缀名去掉