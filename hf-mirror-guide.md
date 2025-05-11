
# Hugging Face 镜像站常见操作指南（hf-mirror.com）

## ✅ 前置准备

### 1. 安装 `aria2`（`hfd.sh` 依赖它来实现多线程高速下载）

在 Debian/Ubuntu 系统中安装：

```bash
sudo apt update
sudo apt install -y aria2
```

### 2. 下载并配置 `hfd.sh` 工具

```bash
wget https://hf-mirror.com/hfd/hfd.sh
chmod +x hfd.sh
```

设置环境变量（每次新终端都需要设置，建议加进 `.bashrc` 或 `.zshrc`）：

```bash
export HF_ENDPOINT=https://hf-mirror.com
```

---

## 📦 模型相关操作

### 下载模型（例如 `bert-base-uncased`）：

```bash
./hfd.sh bert-base-uncased
```

> 📁 模型将被下载到：
>
> * 默认路径：`~/.cache/huggingface/hub/models--bert-base-uncased`
> * 如果你设置了 `HF_HOME`，则是：`$HF_HOME/hub/models--bert-base-uncased`

---

## 🔤 Tokenizer 下载

Tokenizer 会随着模型一同下载，通常不需要单独下载。若需要手动加载，可以通过如下方式：

```python
from transformers import AutoTokenizer
tokenizer = AutoTokenizer.from_pretrained("/path/to/models--bert-base-uncased")
```

---

## 📚 数据集相关操作

### 下载 Hugging Face 数据集（例如 `imdb`）：

```bash
./hfd.sh imdb --dataset
```

> 📁 数据集将被下载到：
>
> * 默认路径：`~/.cache/huggingface/datasets/imdb`
> * 或：`$HF_HOME/datasets/imdb`

你可以像这样使用本地路径加载数据集：

```python
from datasets import load_dataset

dataset = load_dataset("/your/local/path/to/imdb")
```

---

## 📁 文件目录结构参考

```bash
.huggingface/
└── hub/
    └── models--bert-base-uncased/
        ├── config.json
        ├── pytorch_model.bin
        └── tokenizer.json
└── datasets/
    └── imdb/
        ├── dataset_info.json
        ├── train/
        ├── test/
```

---

## 🧼 清除缓存（如需重新下载）

```bash
rm -rf ~/.cache/huggingface/*
# 或者清除你设置的 HF_HOME 路径下的内容
rm -rf $HF_HOME/*
```

---

## 🔎 常见问题

### Q: 下载后怎么加载模型？

```python
from transformers import AutoModel

model = AutoModel.from_pretrained("/path/to/models--bert-base-uncased")
```

### Q: 数据集下载失败？

确保 `--dataset` 参数加在数据集名后，并确保设置了 `HF_ENDPOINT`。

---

