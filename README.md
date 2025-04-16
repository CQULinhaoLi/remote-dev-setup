# remote-dev-setup

🚀 一个远程开发环境配置指南，汇统了使用 **VS Code Remote-SSH**、**Miniconda** 和 **PyTorch** 在 Linux 服务器上高效搭建开发环境的全过程。

本项目帮助你快速完成远程开发环境的配置与优化，解决磁盘空间占用、环境管理混乱等问题，适用于科研、AI 开发、Python 工程等场景。

---

## 📁 项目结构

```
remote-dev-setup/
├— VSCode_Remote_Setup.md   # VS Code 远程连接配置指南
├— Linux_Guide.md           # 服务器端 Miniconda、conda、pip、PyTorch 安装与管理指南
└— LICENSE                  # 项目许可协议（MIT）
```

---

## ✨ 功能亮点

- ✅ 配置 VS Code Remote-SSH 实现服务器远程开发
- ✅ 安装并初始化 Miniconda，管理 Python 环境
- ✅ 自定义 conda 环境与 pip 缓存路径，避免占满 root 分区
- ✅ 安装并配置支持 CUDA 的 PyTorch
- ✅ 项目结构规范，便于代码、模型和数据集管理
- ✅ 提供常用命令、配置模板与问题排查建议

---

## 🚀 快速开始

1. 克隆本项目：

   ```bash
   git clone https://github.com/CQULinhaoLi/remote-dev-setup.git
   cd remote-dev-setup
   ```

2. 按顺序阅读并参考以下文档进行配置：

   - 📄 [`VSCode_Remote_Setup.md`](./VSCode_Remote_Setup.md)：配置 VS Code 远程连接服务器
   - 📄 [`Linux_Guide.md`](./Linux_Guide.md)：在服务器上安装与管理 Miniconda/conda/pip/PyTorch

---

## 💡 推荐目录结构

建议将环境、代码、数据等统一存放在大容量数据盘（如 `/data1/llh/`）下：

```
/data1/llh/
├— conda/      # Conda 环境与包缓存
├— projects/   # 项目代码
├— datasets/   # 数据集
├— models/     # 模型保存
└— logs/       # 日志信息
```

---

## 🧍 项目许可协议

本项目基于 [MIT License](./LICENSE) 开源协议，你可以自由使用、修改和分发。

---

如果你觉得本项目有用，欢迎★Star 或 Fork，并提出意见或贡献内容！

