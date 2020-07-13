# git 使用总结
[git官网](https://git-scm.com/)
## 0.安装与配置
### 0.1安装
#### 0.1.1一键安装脚本
### 0.2配置
#### 0.2.1配置远程仓库
1. 生成ssh秘钥ssh-keygen -t rsa -C "youremail@example.com"<本地仓和远程仓之间数据传输是通过ssh加密的>
2. 复制~/.ssh/id_rsa.pub文件key
3. 复制的key配置到远程仓库的用户设置中
4. 验证ssh -T $url  
**!可能无法解析域名**
#### 0.2.2需要注意的几点问题
* 在linux下安装时,需要创建属于自己的用户,因为在配置远程仓库时,秘钥的分配是对应用户的


## 1.工作流
### 1.1测试
#### 1.1.1清洁代码
linux系统下,测试一般不会改变提交代码,但是要不断git pull代码,更新到最新代码.在保持  
代码原始未修改的情况下,需要还原代码到最新的状态同时清除未受控制的文件.  
* git reset --hard HEAD <这个命令使得,工作区和暂存区回到版本库HEAD状态>
* git clean -df <删除本地仓中未受控的文件>
#### 1.1.2下载远程仓的分支代码
* git clone -b  xxx  $url
### 1.2编码
#### 1.2.1拉取远程仓代码<已配置远程仓库>
* 命令:git clone
* git clone $url // 默认master分支
* git clone -b branchname $url // 指定分支
#### 1.2.2创建本地分支
* git checkout -b newbranch // 创建并切换到本地分支

- git branch newbranch // 创建本地分支
- git checkout newbranch // 切换到本地分支

##### 1.2.2.1创建历史版本分支 
* git checkout -b newbranch $SHA-1
##### 1.2.2.2将当前分支重置到历史版本
* git reset --hard<硬重置丢弃本地变更> $SHA-1 // reset会丢弃历史版本之后的版本
* git push -f 强制更新远程仓<本地指向版本比远程库旧>
##### 1.2.2.3撤销某个历史版本所做修改
* git revert -n $SHA-1
##### 1.2.2.4注意
***!!!不论有几个本地仓,本地只能有一个mater,其他的任务都必须是branch***


## 2.需要注意的几点问题
### 2.1行尾符
* LINUX : LF
* MAC   : CR
* WINDOWS : CR LF
```
git提供几种选择
# 提交时转换为LF，检出时转换为CRLF
git config --global core.autocrlf true

# 提交时转换为LF，检出时不转换
git config --global core.autocrlf input

# 提交检出均不转换
git config --global core.autocrlf false
```
*windows编码的文本源码文件,不能直接放到linux执行*
*安全设置*
```
# 拒绝提交包含混合换行符的文件
git config --global core.safecrlf true

# 允许提交包含混合换行符的文件
git config --global core.safecrlf false

# 提交包含混合换行符的文件时给出警告
git config --global core.safecrlf warn
```
### 2.2与远程仓数据交互文件大小限制
#### 2.2.1大文件存储在LFS
这种情况下,必须安装git lfs才能正确clone成功
#### 2.2.2远程仓库文件上传设置限制
远程仓库可以设置push文件的大小的最大值
#### 2.2.3克隆不完全,本地磁盘空间不足<不会提示>
