# GIT教程 #
    ## 全局配置： ##
        ### 基本信息： ###
            * git config --global user.name "userName"    # 全局用户名
            * git config --global user.email "emailAddr"  # 全局邮箱
        别名创建：
            * git config --global alias.st status         # 给命令配置别名，该配置之后，可以使用 git st 来代表命令  git status
            * git config --global alias.unstage 'reset HEAD' # git unstage test.py 相当于 git reset HEAD test.py
            * git config --global alias.last 'log -1'     # 用 git last 来查看最近的一次提交，类似的有git ci =git commit， branch=br 等等

    仓库创建和管理仓库：
        * git init                                    # 在对应的目录下执行该语句
        * git remote add origin gitUrl                # 将本地的git内容，加载到远程仓库的本地暂存区
        * git push -u origin master                   # 与上一条命令联合使用，将本地的git内容提交到远程仓库中去（首次使用-u，会将本地master分支和远程master分支关联）
        * git push origin master                      # 通过上一条命令，本地和远程分支关联后，再次提交就不需要加上 -u 参数了
        * git clone gitUrl                            # 克隆远程仓库内容到本地；git支持https,但通过ssh支持的原生git协议速度更快
    
    分支创建和相关操作：
        * git checkout -b dev                         # 切换到dev分支，-b表示创建并切换,相当于 git branch dev & git checkout dev
        * git branch dev                              # 创建dev分支
        * git checkout dev                            # 切换到dev分支 (也可以是master分支)
        * git checkout -- fileName                    # 丢弃工作区的当前文件的所有修改（资源管理器中删除后可以使用该命令恢复）
        * git branch -D featrueX                      # 强制删除还没有被合并的分支
        * git branch                                  # 查看所有的分支内容，当前分支前会有 * 号标识
        * git rebase                                  # 如果各种分支提交合并，会有分叉，提交历史将不再是一条直线，使用rebase将提交记录变基
        * git branck -d dev                           # 删除dev分支
        * git remote rm origin                        # 删除本地已有的远程库
        * git remote add gitee gitUrl                 # 指定关联的远程库的名称为gitee，默认不写，是为origin
        * git push gitee master                       # 向gitee指定的仓库推送
        


    同步、提交、合并和删除等基本操作：
        * git add file1 file2 ...                     # 增加一个或多个文件到本地暂存区stage
        * git commit -m "add files"                   # 提交缓存区的文件， -m 用于指定提交说明
        * git diff fileName                           # 查看fileName对应文件与仓库的不同
        * git rm fileName                             # 删除文件，可以使用git reset 恢复到指定版本，再使用git checkout恢复（其他方式是否更简单未尝试）
        * git reset --hard HEAD^                      # 表示回退到上一个版本，HEAD表示当前，一个^表示上一个，一次类推，HEAD~100表示前100个
        * git reset --hrad commitId[唯一标识]          # 另外，可以记录版本的commitId前几个值，可以直接回退到指定版本
        * git reset HEAD fileName                     # 回退指定文件到当前版本库的版本内容，（对于已经git add 的文件，可以使用这种方式）
        * git merge dev                               # 将当前分支同dev分支进行合并，如果是Fast forward ，说明只是修改了HEAD执行，该分支删除后会丢弃分支信息
        * git merge --no-ff -m "with no-ff mode" dev  # 如果要强制禁用Fast forward模式，git会在合并时生成一个新的commit，这样就可以从历史中看出分支信息
        * git remote                                  # 查看远程仓库信息, -v用于显示更相信内容，如果没有push的信息，说明没有提交的权限
        * git pull                                    # 从远程拉取最新内容，冲突时，需要再本地处理后提交
        * git branch --set-upstream-to=origin/dev dev # 多人协同时，可能分支不一样，所以会导致分支拉取异常，使用该方式指定本地dev与远程dev分支关联

    相关的仓库本本信息处理：
        * git tag v1.0                                # 对分支打标记（标签是针对一次commit来确定的，不是分支！！！），默认是HEAD
        * git tag                                     # 查看标记信息（所有的）
        * git tag v0.9 commitId                       # 对已经提交的某个版本进行打标记处理
        * git show tagId                              # 查看某次标记的详细信息
        * git tag -a v1.1 -m "version 1.1 tag" commitId  # -a用于指定标签名称，用-m指定对该标签描述，commitId指定提交的那个版本
        * git tag -d v1.0                             # 打错了标签，可以进行删除（标签不主动推送只会存储再本地，不会提交到远程，可以放心删除）
        * git push origin v1.0                        # 推送某个标签到远程
        * git push origin --tags                      # 推送全部的标签
        * git push origin :refs/tags/v1.0             # 删除远程的标签，这个删除需要先删除本地，然后再从远程删除，这是删除远程命令

    忽略文件的相关操作：
        * git add -f App.class                        # .gitignore中配置忽略的文件，可以通过-f来强hi提交
        * git check-ignore -v App.class               # 用于查看是哪一行的内容忽略了该文件

    
    暂存工作区：
        * git stash                                   # 储藏当前工作现场（用于当前工作未完成时，需要紧急修复bug时，开分支处理）
        * git stash list                              # 查看工作现场储存列表
        * git stath pop                               # 恢复最近的stash现场并删除该stash，相当于 git stash apply & git stash drop
        * git stash apply stash@{x}                   # 将当前工作现场恢复成x的stash版本，stash可以有多个


    其他命令：
        * git status                                  # 仓库状态，如果有未提交内容或者非版本内文件，会有提示
        * git log                                     # 查看提交记录，--pretty=oneline用于在单行内显示
        * git log --graph --pretty=oneline --abbrev-commit  # 查看分支合并情况， --graph用于查看分支合并图
        * git reflog                                  # 记录每一个操作命令

        
    # 用于显式的高亮部分git log的内容
    * git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

    # git配置文件放在 .git/config 目录中，用户的配置在用户目录下的   .gitconfig 文档中

    # 搭建一个git服务器，主要使用ubuntu或者debian
    sudo apt-get install git            # 安装git
    sudo adduser git                    # 创建一个git用户，用来运行git服务
    # 搜集所有需要登陆的用户的公钥，id_rsa.pub文件，把所有的公钥导入到 /home/git/.ssh/authorized_keys文件中，一行一个
    # 初始化git仓库，假设是/srv/sample.git，在/srv目录下输入下列命令初始化仓库
    sudo git init --bare sample.git
    # 修改owner用户
    sudo chown -R git:git sample.git
    # 禁用shell登陆，可以通过编辑 /etc/passwd 文件完成，修改一下内容
    git:x:1001:1001:,,,:/home/git:/bin/bash  -> git:x:1001:1001:,,,:/home/git:/user/bin/git-shell
    

    ## 公钥管理，可以使用 Gitosis 工具来管理
    ## 权限管理，可以使用 Gitolite  来实现

