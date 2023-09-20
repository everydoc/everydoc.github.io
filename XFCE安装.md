# XFCE安装



在CentOS 7上安装XFCE桌面环境，可以按照以下步骤进行：

1. 使用root权限登录CentOS 7系统。

2. 执行以下命令以更新系统软件包：

   ```shell
   yum update -y
   ```

3. 安装x window system

   > yum -y install "X Window system"
   
3. 执行以下命令以安装XFCE桌面环境：

   ```shell
   yum groupinstall -y "Xfce"
   ```

4. 配置

   ```shell
   cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
   ```

   

4. 安装完成后，执行以下命令以启动XFCE桌面环境：

   ```shell
   startxfce4
   ```

   或者，您也可以重启系统以启动XFCE桌面环境。

5. 如果您想在登录时选择XFCE桌面环境，请按照以下步骤进行：

   - 安装lightdm管理器：

     ```shell
     yum install -y lightdm
     ```

   - 将lightdm设置为默认的显示管理器：

     ```shell
     systemctl enable lightdm
     systemctl set-default graphical.target
     ```

   - 重启系统以应用更改。

现在，您已经在CentOS 7上成功安装并启动了XFCE桌面环境。如果您在登录时选择了XFCE桌面环境，您将进入一个简单、快速、可定制的桌面环境。