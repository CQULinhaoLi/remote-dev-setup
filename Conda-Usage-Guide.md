# Conda 使用指南

Conda 是一个开源的包管理和环境管理系统，支持 Windows、macOS 和 Linux 平台，广泛用于 Python 科学计算和数据科学项目。本文档旨在提供一套实用的 Conda 使用指南，涵盖安装、环境管理、包管理与配置优化。

## 1. Conda 安装

推荐安装 Miniconda（轻量版本）而非 Anaconda：

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
bash miniconda.sh -b -p ~/miniconda3
~/miniconda3/bin/conda init
source ~/.bashrc
```

## 2. Conda 环境管理

### 2.1 创建新环境
```bash
conda create -n my_env python=3.12
```

### 2.2 激活环境
```bash
conda activate my_env
```

### 2.3 停用环境
```bash
conda deactivate
```

### 2.4 删除环境
```bash
conda env remove -n my_env
```

### 2.5 克隆环境
```bash
conda create -n new_env --clone old_env
```

### 2.6 导出与重建环境
```bash
# 导出
conda env export > environment.yml

# 重建
conda env create -f environment.yml
```

## 3. 包管理

### 3.1 安装包
```bash
conda install numpy pandas
```

### 3.2 移除包
```bash
conda remove numpy
```

### 3.3 更新包
```bash
conda update pandas
```

### 3.4 查看已安装包
```bash
conda list
```

## 4. Conda 镜像源配置（推荐使用清华源）

编辑或创建 `~/.condarc` 文件：
```yaml
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - defaults
show_channel_urls: true
```

## 5. 自定义环境和包缓存位置

若根分区空间不足，可通过 `.condarc` 配置：
```yaml
envs_dirs:
  - /data1/llh/conda/envs
pkgs_dirs:
  - /data1/llh/conda/pkgs
```

## 6. 常见问题

### 6.1 `Solving environment` 卡住

可尝试添加 `--no-update-deps --force-reinstall` 或使用 `mamba`（conda 的更快替代品）。

### 6.2 环境无效 / 初始化失败
```bash
conda init
source ~/.bashrc
```

### 6.3 与 pip 协作

尽量优先使用 `conda install`，若需使用 pip，可先激活环境再执行：
```bash
pip install some_package
```

## 7. 建议

- 将基础环境设为模板，使用 `conda create --clone` 克隆用于不同项目；
- 将环境导出为 `YAML` 文件，便于迁移与分享；
- 定期清理不再使用的环境与包缓存。

---

本指南适用于 Conda 常规使用，如需结合服务器配置、pip 缓存管理等场景，建议参考配套文档。

