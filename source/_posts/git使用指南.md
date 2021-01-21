# vscode与github远程仓库的连接
- 先在github上建立新的仓库
  - 出现fatal: not a git repository (or any of the parent directories): .git， 则是缺少.git文件 输入git init 即可解决
- git remote add github + http链接
- 分支状态可在左下角查看
- git checkout 切换分支
# vscode 直接下载github项目
- 复制需要下载的github仓库链接
- 切换到目标目录
- vscode终端输入git clone + url
- 

# 疑问
git fetch

# hexo.io
- 博客格式设置时，在_config.yml里找到对应项目进行修改
hexo g -d 