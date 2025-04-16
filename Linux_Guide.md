# Linux 系统基本操作指南

> 本文档记录了 Linux 系统中常用的基本操作，适合新手用户或作为日常使用的速查手册。

---

## 🖥️ 1. 系统信息查看

```bash
uname -a              # 查看系统内核版本等信息
lsb_release -a        # 查看发行版信息（部分系统需先安装）
hostname              # 查看主机名
whoami                # 当前用户名
```

---

## 📁 2. 文件与目录管理

```bash
pwd                   # 显示当前路径
ls                    # 查看当前目录下文件
ls -l /path           # 详细列出指定目录内容
cd /path/to/dir       # 进入目录
mkdir new_dir         # 创建新目录
rm file.txt           # 删除文件
rm -r dir             # 删除目录及其内容
cp file1 file2        # 复制文件
mv old new            # 重命名或移动文件/目录
```

---

## 🔍 3. 文件查看与编辑

```bash
cat file.txt          # 查看文件内容
head -n 10 file.txt   # 查看前 10 行
tail -n 10 file.txt   # 查看后 10 行
tail -f file.txt      # 实时输出文件内容
nano file.txt         # 使用 nano 编辑文件
vim file.txt          # 使用 vim 编辑文件（高级）
```

---

## 📦 4. 软件包管理（以 Debian/Ubuntu 为例）

```bash
sudo apt update       # 更新软件源
sudo apt upgrade      # 升级已安装软件
sudo apt install xxx  # 安装软件
sudo apt remove xxx   # 卸载软件
which xxx             # 查看命令路径
```

---

## 🧠 5. 用户与权限管理

```bash
sudo su               # 以 root 权限执行
chmod +x file.sh      # 给文件添加可执行权限
chown user:group file # 改变文件所有权
```

---

## 🧰 6. 常用系统工具

```bash
top                   # 实时查看系统资源占用
htop                  # 更友好的 top（需安装）
df -h                 # 查看磁盘使用情况
du -sh *              # 查看当前目录每个文件/目录的大小
ps aux | grep python  # 查看 Python 进程
kill -9 PID           # 强制结束进程
```

---

## 🌐 7. 网络命令

```bash
ping www.baidu.com    # 测试网络连接
ifconfig / ip a       # 查看网络信息（推荐用 ip a）
netstat -tunlp        # 查看端口监听（需安装）
curl http://url       # 测试接口、网页等
scp file user@ip:~/   # 拷贝文件到远程服务器
```

---

## ⚙️ 8. 环境变量

```bash
echo $PATH            # 查看 PATH
export VAR=value      # 设置变量（临时）
echo 'export VAR=val' >> ~/.bashrc  # 设置变量（永久）
source ~/.bashrc      # 使修改生效
```

---

## 🧹 9. 清理与其他

```bash
history               # 查看历史命令
clear                 # 清屏
```

---

## 📚 10. 文件压缩与解压

```bash
tar -czvf file.tar.gz folder/    # 压缩
 tar -xzvf file.tar.gz           # 解压
zip -r file.zip folder/          # 压缩为 zip
unzip file.zip                   # 解压 zip
```

---

> 📌 小贴士：可以使用 `alias` 定义常用命令的别名，例如：

```bash
echo "alias ll='ls -alF'" >> ~/.bashrc
source ~/.bashrc
```

---

📝 文档维护者：llh  
🕓 最后更新：2025 年 4 月 16 日

