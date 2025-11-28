# 🤖 网课AI答题助手 - 本地模型版

> 基于本地大语言模型的智能网课答题脚本

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Tampermonkey](https://img.shields.io/badge/Tampermonkey-4.0+-green.svg)](https://www.tampermonkey.net/)

## 📖 项目简介

本项目是基于 [isMobile](https://greasyfork.org/users/1131905-ismobile) 的开源项目 [超星学习通网课小助手](https://greasyfork.org/scripts/488083) 将原有的在线题库查询功能替换为本地部署的AI大模型自动答题。作者3090显卡使用的是qwen3-8b，请结合自身算力选择合适的模型。国内用户推荐modelscope下载语言模型。

### 🎯 支持平台

- ✅ 超星学习通（测试通过）
- ⚠️ 知到智慧树（未测试）
- ⚠️ 其他平台（未测试）

### ⚠️ 免责声明

**本项目仅供学习研究使用，请勿用于违反学术诚信的行为。使用本项目产生的任何后果由使用者自行承担，与开发者无关。**

---
以下内容为AI生成，仅供参考

## 🚀 快速开始

### 前置要求

- Python 3.8 或更高版本
- 至少 8GB 内存（推荐 16GB）
- Tampermonkey 浏览器扩展
- 本地Qwen模型文件（或其他兼容模型）

### 30秒快速部署

```bash
# 1. 克隆项目
git clone https://github.com/你的用户名/你的项目名.git
cd 你的项目名

# 2. 安装Python依赖
pip install fastapi uvicorn transformers torch accelerate

# 3. 修改模型路径
# 编辑 ai_answer_service_local.py，第38行：
# "model_path": r"你的模型路径"

# 4. 启动服务
python ai_answer_service_local.py

# 5. 安装Tampermonkey脚本
# 在浏览器中安装 jiaoben.js
```

1. 安装 [Tampermonkey](https://www.tampermonkey.net/) 浏览器扩展
2. 点击Tampermonkey图标 → 管理面板 → 添加新脚本
3. 复制 `jiaoben.js` 的全部内容
4. 粘贴到编辑器中
5. 保存（Ctrl+S）





## 📜 开源许可

本项目采用 [MIT License](LICENSE) 开源协议。

### 第三方致谢

本项目基于以下开源项目改造：
- [超星学习通网课小助手](https://greasyfork.org/scripts/488083) by [isMobile](https://greasyfork.org/users/1131905-ismobile)
- [Qwen](https://github.com/QwenLM/Qwen) by Alibaba Cloud

感谢所有开源贡献者！

---

## ⚖️ 免责声明

1. **学术诚信**：本项目仅供学习研究使用，请勿用于违反学术诚信或任何违法行为
2. **使用责任**：使用本项目产生的任何后果由使用者自行承担
3. **无担保**：软件按"原样"提供，不提供任何明示或暗示的担保
4. **版权尊重**：请尊重原作者的劳动成果，使用时保留版权声明

**请在合法合规的前提下使用本项目。**


## 🌟 Star History

如果这个项目对你有帮助，请给一个 ⭐️ Star，谢谢！


