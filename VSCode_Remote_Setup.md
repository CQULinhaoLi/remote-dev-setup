# VS Code 配置远程服务器和 Conda 环境管理全过程

## 1. 通过 VS Code 连接服务器

### 1.1 安装 Remote - SSH 插件
启动 VS Code，打开扩展窗口 (快捷键 Ctrl+Shift+X)，搜索 "Remote - SSH"，安装 Microsoft 提供的插件。

### 1.2 配置 SSH 连接

**方法一：直接通过命令连接**
- 按 F1 或 Ctrl+Shift+P
- 输入 "Remote-SSH: Connect to Host..."
- 输入 `ssh llh@192.168.1.100` (按照实际地址)，按提示输入密码

**方法二：编辑 SSH 配置文件**
- F1 > Remote-SSH: Open SSH Configuration File...
- 选择 `~/.ssh/config`，添加以下内容：

```ssh
Host myserver
    HostName 192.168.1.100
    User llh
    Port 22
```

之后即可通过 myserver 名称连接。

### 1.3 打开服务器上的项目
连接成功后，在 VS Code 中点击“打开文件夹”，选择项目目录即可。

---

## 2. 在服务器上安装 Miniconda

### 2.1 下载 Miniconda 安装脚本
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
```

### 2.2 安装 Miniconda
```bash
bash miniconda.sh -b -p ~/miniconda3
```
参数说明：
- `-b`：静默安装
- `-p`：指定安装路径

### 2.3 初始化 Conda
```bash
~/miniconda3/bin/conda init
source ~/.bashrc
```

---

## 3. Conda 环境管理

### 3.1 默认环境路径问题
Conda 默认会将环境保存到 `~/miniconda3/envs`，如果 home 分区空间有限，可移至 `/data1/llh/conda` 等大容量目录。

### 3.2 配置 .condarc 文件
```yaml
# ~/.condarc
envs_dirs:
  - /data1/llh/conda/envs

pkgs_dirs:
  - /data1/llh/conda/pkgs
```

### 3.3 重创环境
```bash
conda create -n pytorch_py_3_12 python=3.12
```

### 3.4 克隆基础环境
```bash
conda create --name my_project_env --clone pytorch_py_3_12
conda activate my_project_env
```

---

## 4. pip 缓存目录重定向

### 4.1 设置临时缓存目录
```bash
export PIP_CACHE_DIR=/data1/llh/conda/pip_cache
```

### 4.2 持久生效
```bash
echo 'export PIP_CACHE_DIR=/data1/llh/conda/pip_cache' >> ~/.bashrc
source ~/.bashrc
```

### 4.3 清除旧缓存
```bash
rm -rf ~/.cache/pip
# 或
pip cache purge
```

---

## 5. 安装 PyTorch 示例

### 5.1 pip 安装 PyTorch (CUDA 12.6)
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126
```

### 5.2 空间不足处理
```bash
mkdir -p /data1/llh/tmp
TMPDIR=/data1/llh/tmp pip install --no-cache-dir torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126
```

---

## 6. 项目和数据管理建议

```
/data1/llh/
├── conda/          # Conda 环境和包缓存
├── projects/       # 项目代码
├── datasets/       # 数据集
├── models/         # 模型
├── logs/           # 日志
```

### 示例代码
```python
# 保存模型
torch.save(model.state_dict(), "/data1/llh/models/my_model.pt")

# 指定数据路径
from torchvision import datasets, transforms
mnist = datasets.MNIST(root="/data1/llh/datasets/mnist", download=True, transform=transforms.ToTensor())
```

---

## 7. 其他提示
- 相关目录权限确认：
```bash
sudo chown -R llh:llh /data1/llh
```
- 定期清理 pip 缓存，时间直接删除 `/tmp` 文件
- 建议使用 YAML 文件导出/重建环境，便于跨机跨环境迁移

---

## 8. 总结

本文档包括：
- VS Code 配置 SSH 连接服务器
- 服务器上安装 Miniconda 和 Conda 初始化
- 通过 `.condarc` 设置 Conda 环境和包缓存位置
- pip 缓存目录重定向
- PyTorch 安装示例以及空间问题处理
- 项目/数据/模型目录组织提议

这是一套面向服务器远程开发和 AI 项目开始设置的基础文档，适合放置在 GitHub README 或 Wiki 中便于实时检索。
