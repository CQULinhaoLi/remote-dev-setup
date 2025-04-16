# VS Code 配置远程服务器
## 1.1 安装 Remote - SSH 插件
启动 VS Code，打开扩展（快捷键 Ctrl+Shift+X），搜索 “Remote - SSH”，安装 Microsoft 提供的插件。

## 1.2 配置 SSH 连接
### 方法一：直接通过命令连接
按下 F1 或 Ctrl+Shift+P，输入并选择 Remote-SSH: Connect to Host...；

输入 `ssh username@ip_address`（例如：`ssh llh@192.168.1.100`）进行连接，会提示输入密码。

### 方法二：编辑 SSH 配置文件
打开命令面板，执行 Remote-SSH: Open SSH Configuration File...；

选择配置文件（如 `~/.ssh/config`），添加如下配置：
```ssh
Host myserver
    HostName xx.xx.xx.xx
    User llh
    Port 22
```
以后可直接通过 Remote-SSH: Connect to Host... 选择 `myserver` 连接。

## 1.3 打开服务器上的文件夹
连接成功后，在 VS Code 中点击左上角的“打开文件夹”，进入服务器指定的项目目录。

# 在服务器上安装 Miniconda
## 2.1 下载 Miniconda 安装脚本
在 VS Code 远程终端中运行：
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
```
## 2.2 安装 Miniconda
假设你希望将 Miniconda 安装在你的 home 目录下（例如 `/home/llh/miniconda3`）：
```bash
bash miniconda.sh -b -p ~/miniconda3
```
参数说明：

`-b`：静默安装（无需交互）

`-p`：指定安装路径

## 2.3 初始化 Conda
```bash
~/miniconda3/bin/conda init
source ~/.bashrc
```
如果需要在其他 shell（如 zsh）中使用，请修改相应的配置文件（例如 `~/.zshrc`）。

# Conda 环境管理与定制
## 3.1 默认环境位置与问题
默认情况下，新环境会被创建在 `~/miniconda3/envs` 下，可能会占用 root 分区（`/home`）空间。如果空间有限，建议将环境和包缓存移至其他磁盘，例如 `/data1/llh/conda`。

## 3.2 修改 .condarc 配置文件
在用户主目录下创建或编辑 `~/.condarc` 文件，加入以下内容：
```yaml
envs_dirs:
  - /data1/llh/conda/envs

pkgs_dirs:
  - /data1/llh/conda/pkgs
```
这将使以后新建的环境和下载的包缓存存放在 `/data1/llh/conda` 下。

如果你已经有旧环境在默认路径中，可以选择克隆（迁移）或重建。

## 3.3 克隆基础环境
你希望将 `pytorch_py_3_12` 环境作为 PyTorch 基础环境，根据不同项目需要再创建其他环境。

### 删除旧环境（可选操作）
如果之前的环境不需要保留，可以删除：
```bash
conda env remove -n pytorch_py_3_12
```
### 重新创建环境到新路径
由于 `.condarc` 配置生效后，新的环境将自动创建在 `/data1/llh/conda/envs` 内。例如：
```bash
conda create -n pytorch_py_3_12 python=3.12
```
使用 `conda info --envs` 检查确认新环境路径正确。

### 克隆基础环境用于项目（模板环境）
从基础环境克隆新环境，便于管理不同的包：
```bash
conda create --name my_new_env --clone pytorch_py_3_12
```
激活克隆后的新环境：
```bash
conda activate my_new_env
```
在该环境中根据项目需要安装额外包或移除不需要的包。

### 导出与重建环境（可选）
导出当前环境：
```bash
conda env export > pytorch_py_3_12.yml
```
重建环境：
```bash
conda env create -f pytorch_py_3_12.yml
```
# Pip 缓存目录配置
默认 pip 缓存位于 `~/.cache/pip`。为了避免占用 root 分区空间，我们将 pip 缓存迁移到大容量数据盘上。

## 4.1 设置临时缓存目录
在终端中执行：
```bash
export PIP_CACHE_DIR=/data1/llh/conda/pip_cache
```
该设置只对当前会话有效。

## 4.2 设置永久缓存目录
将上述命令写入 `~/.bashrc`（或对应配置文件），以便每次启动终端自动加载：
```bash
echo 'export PIP_CACHE_DIR=/data1/llh/conda/pip_cache' >> ~/.bashrc
source ~/.bashrc
```
## 4.3 清除旧 pip 缓存
如果需要释放空间：
```bash
rm -rf ~/.cache/pip
```
或使用 pip 命令：
```bash
pip cache purge
```
# 安装包（PyTorch 案例）
## 5.1 在新环境中激活并安装 PyTorch
激活环境：
```bash
conda activate pytorch_py_3_12
```
使用 pip 安装（示例以 CUDA 12.6 支持为例）：
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126
```
注意：安装过程中如果遇到“No space left on device”错误，请检查临时目录和缓存目录是否被正确重定向到有足够空间的分区（例如 `/data1/llh`）。

## 5.2 若下载大文件时空间不足
你可以在安装命令中指定临时目录：
```bash
TMPDIR=/data1/llh/tmp pip install --no-cache-dir torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126
```
确保 `/data1/llh/tmp` 存在：
```bash
mkdir -p /data1/llh/tmp
```
# 项目数据与文件存放策略
建议将所有项目、数据集、模型保存都放置在数据盘上（例如 `/data1/llh/`）下的单独目录结构，以降低 root 分区使用率。示例如下：
```bash
/data1/llh/
├── conda/          # Conda 环境及包缓存（已配置在 .condarc 中）
├── pip_cache/      # pip 缓存目录（通过 PIP_CACHE_DIR 设置）
├── projects/       # 存放各个项目代码和运行日志
├── datasets/       # 数据集存放目录
├── models/         # 模型保存目录
└── logs/           # 日志文件目录
```
在代码中，可直接指定保存位置，如：
```python
# 保存模型
torch.save(model.state_dict(), "/data1/llh/models/my_model.pt")

# 下载数据集时指定目录
from torchvision import datasets, transforms
mnist_data = datasets.MNIST(root="/data1/llh/datasets/mnist", train=True, download=True, transform=transforms.ToTensor())
```
# 其他注意事项
权限：确保 `/data1/llh` 相关目录属于你（用户 `llh`），例如：
```bash
sudo chown -R llh:llh /data1/llh
```
定期清理：注意定期清理 pip 缓存和临时文件，使用 `pip cache purge` 和清理 `/tmp` 文件；

环境管理：尽量使用 YAML 文件导出环境配置，方便在不同机器和版本间迁移。

# 总结
本指南详细记录了：

- VS Code 远程开发配置：设置 SSH 连接、打开服务器目录；
- Miniconda 安装与初始化：下载、安装、环境初始化；
- Conda 环境的迁移与管理：通过 `.condarc` 配置将环境与包缓存存放到 `/data1/llh/conda` 中，克隆环境作为基础；
- pip 缓存目录重定向：设置环境变量 `PIP_CACHE_DIR`，并清理旧缓存；
- 安装 PyTorch：在环境中使用 pip 安装 PyTorch 及依赖，并处理大文件下载时的磁盘空间问题；
- 项目数据存储：如何将项目数据、模型、日志保存到 `/data1/llh/` 下。
