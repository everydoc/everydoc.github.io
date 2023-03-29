# centos 7 install pip

您可以按照以下步骤在 CentOS 7 上安装 pip：

更新您的系统：
```shell
sudo yum update
```
安装必要的软件包：
```shell
sudo yum install epel-release
sudo yum install python-pip
```
验证 pip 是否安装成功：
```shell
pip --version
```
如果一切顺利，您应该能够看到安装的 pip 版本号。

如果出现错误，您可能需要安装一些其他的软件包或更新您的系统。