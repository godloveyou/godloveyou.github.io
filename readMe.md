## 我的博客管理流程 
参考 (http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more)

1. 日常修改
在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理：
    * 必须在dev分支下进行
	* 依次执行git add .、git commit -m “…”、git push origin dev指令将改动推送到GitHub（此时当前分支应为dev）；
	* 然后才执行hexo generate -d发布网站到github的master分支上，也就是发布了网站到服务器了.
	  虽然两个过程顺序调转一般不会有问题，不过逻辑上这样的顺序是绝对没问题的（例如突然死机要重装了，悲催….的情况，调转顺序就有问题了）

2 本地资料丢失
当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：

	* 使用git clone git@github.com:godloveyou/godloveyou.github.io.git拷贝仓库（默认分支为dev）；
	* 在本地新拷贝的godloveyou.github.io文件夹下通过Git bash依次执行下列指令：
	    npm install hexo、npm install、npm install hexo-deployer-git
	    （记得，不需要hexo init这条指令）。
	
   (国内一般使用淘宝的镜像，因为npm实在是太慢了)
3. 博客使用了NeXT主题参考文档
http://theme-next.iissnan.com/getting-started.html


4. 常用指令
git checkout dev; 切换到dev分支
git branch;  查看当前分支


hexo g #等价于 hexo generate
hexo s #等价于 hexo server


