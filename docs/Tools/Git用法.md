# Git Usage

## 如何修改 hosts

使用 ping 网站，比如[这个](http://ping.chinaz.com/)检查 IP 地址的通讯状况。比如测试`github.com`，找到一个可以 ping 通的 IP 地址：

![u1688z](http://qny.mrpwei.cc/uPic/u1688z.png)

接着，打开终端，使用 vim 打开 hosts 文件，修改 github.com 的 DNS：

```
sudo vim /etc/hosts
```

![5rdi7z](http://qny.mrpwei.cc/uPic/5rdi7z.png)

一般添加好 github.com 和 raw.githubusercontent.com 等经常挂的域名就行，其它的看情况添加。

完整的列表：

```
# Github Hosts
# domain: github.com
140.82.113.4 github.com
140.82.114.9 nodeload.github.com
140.82.112.5 api.github.com
140.82.112.10 codeload.github.com
185.199.108.133 raw.github.com
185.199.108.153 training.github.com
185.199.108.153 assets-cdn.github.com
185.199.108.153 documentcloud.github.com
140.82.114.17 help.github.com

# domain: githubstatus.com
185.199.108.153 githubstatus.com

# domain: fastly.net
199.232.69.194 github.global.ssl.fastly.net

# domain: githubusercontent.com
185.199.108.133 raw.githubusercontent.com
185.199.108.154 pkg-containers.githubusercontent.com
185.199.108.133 cloud.githubusercontent.com
185.199.108.133 gist.githubusercontent.com
185.199.108.133 marketplace-screenshots.githubusercontent.com
185.199.108.133 repository-images.githubusercontent.com
185.199.108.133 user-images.githubusercontent.com
185.199.108.133 desktop.githubusercontent.com
185.199.108.133 avatars.githubusercontent.com
185.199.108.133 avatars0.githubusercontent.com
185.199.108.133 avatars1.githubusercontent.com
185.199.108.133 avatars2.githubusercontent.com
185.199.108.133 avatars3.githubusercontent.com
185.199.108.133 avatars4.githubusercontent.com
185.199.108.133 avatars5.githubusercontent.com
185.199.108.133 avatars6.githubusercontent.com
185.199.108.133 avatars7.githubusercontent.com
185.199.108.133 avatars8.githubusercontent.com
# End of the section
```

## 如何配置 GitHub

1. 进入 https://github.com/settings/keys
2. 如果页面里已经有一些 key，就点「delete」按钮把这些 key 全删掉。如果没有，就往下看
3. 点击 New SSH key，你需要输入 Title 和 Key，但是你现在没有 key，往下看
4. 打开 Git Bash
5. 复制并运行 `rm -rf ~/.ssh/*` 把现有的 ssh key 都删掉，这句命令行如果你多打一个空格，可能就要重装系统了，建议复制运行。
6. 运行 `ssh-keygen -t rsa -b 4096 -C "你的邮箱"`，注意填写你的邮箱！
7. 按回车三次
8. 运行 `cat ~/.ssh/id_rsa.pub`，得到一串东西，完整的复制这串东西
9. 回到上面第 3 步的页面，在 Title 输入名称，如「我的第一个 key」
10. 在 Key 里粘贴 8 中你复制的那串东西
11. 点击 Add SSH key
12. 回到 Git Bash，运行 `ssh -T git@github.com`，你可能会看到这样的提示：
    ![图片](https://upload-images.jianshu.io/upload_images/9351608-2e1ffd46978b8934.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

输入 yes 回车。

然后如果你看到 `Permission denied (publickey).` 就说明你失败了，请回到第 1 步重来，是的，回到第 1 步重来；如果你看到 `Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.` 就说明你成功了！

来源：https://www.jianshu.com/p/3bedd17343d9

## 如何配置 Git

```
git config --global user.name 你的英文名   #此英文名不需要跟GitHub账号保持一致
git config --global user.email 你的邮箱    #此邮箱不需要跟GitHub账号保持一致
git config --global push.default matching
git config --global core.quotepath false
git config --global core.editor "vim"
```
