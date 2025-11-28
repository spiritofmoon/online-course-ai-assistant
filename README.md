# 🤖 网课AI答题助手 - 本地模型版

> 基于本地大语言模型的智能网课答题脚本

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Tampermonkey](https://img.shields.io/badge/Tampermonkey-4.0+-green.svg)](https://www.tampermonkey.net/)

## 📖 项目简介

本项目是基于 [isMobile](https://greasyfork.org/users/1131905-ismobile) 的开源项目 [超星学习通网课小助手](https://greasyfork.org/scripts/488083) 将原有的在线题库查询功能替换为本地部署的AI大模型自动答题。作者3090显卡使用的是qwen3-8b，请结合自身算力选择合适的模型。

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

---

## 📦 安装指南

### 步骤1：准备AI模型

#### 方案A：使用本地已有模型（推荐）

如果你已经下载了Qwen或其他HuggingFace格式的模型：

1. 记住你的模型路径，例如：
   - `D:\models\Qwen2.5-7B-Instruct`
   - `E:\AI\qwen\qwen2.5-14b-instruct`

#### 方案B：从HuggingFace下载

```python
# 在 ai_answer_service_local.py 中设置为HF模型ID
"model_path": "Qwen/Qwen2.5-7B-Instruct"  # 首次运行自动下载
```

**推荐模型：**
| 模型 | 大小 | 内存需求 | 适合 |
|------|------|----------|------|
| Qwen2.5-1.5B | ~2GB | 4-8GB | 低配置电脑 |
| Qwen2.5-7B | ~4GB | 8-16GB | 推荐配置 |
| Qwen2.5-14B | ~8GB | 16GB+ | 高配置追求准确率 |

### 步骤2：安装Python依赖

```bash
# 基础依赖
pip install fastapi uvicorn transformers torch accelerate

# GPU加速（如果有NVIDIA显卡）
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### 步骤3：配置AI服务

编辑 `ai_answer_service_local.py`：

```python
MODEL_CONFIG = {
    # ⭐ 修改为你的模型路径
    "model_path": r"D:\models\Qwen2.5-7B-Instruct",
    
    # 设备配置（auto=自动选择GPU/CPU）
    "device_map": "auto",
    
    # 精度配置（GPU用float16，CPU改为float32）
    "torch_dtype": torch.float16,
    
    # 生成参数（可选调整）
    "temperature": 0.3,  # 降低随机性
    "top_p": 0.9,
    "max_new_tokens": 256
}
```

### 步骤4：启动AI服务

```bash
# 方式1：直接运行
python ai_answer_service_local.py

# 方式2：后台运行（Linux/Mac）
nohup python ai_answer_service_local.py > service.log 2>&1 &
```

看到以下输出说明启动成功：
```
✓ 模型加载成功！
设备: cuda:0
✓ 服务启动成功！
API地址: http://localhost:5000
```

### 步骤5：安装Tampermonkey脚本

1. 安装 [Tampermonkey](https://www.tampermonkey.net/) 浏览器扩展
2. 点击Tampermonkey图标 → 管理面板 → 添加新脚本
3. 复制 `jiaoben.js` 的全部内容
4. 粘贴到编辑器中
5. 保存（Ctrl+S）

### 步骤6：开始使用

1. 访问网课页面（如超星学习通）
2. 页面右侧会出现脚本控制面板
3. 点击"开始答题"
4. 等待AI自动完成答题

---

## ⚙️ 配置说明

### AI服务配置参数

```python
MODEL_CONFIG = {
    "model_path": "模型路径",
    "device_map": "auto",          # auto/cpu/cuda:0
    "torch_dtype": torch.float16,   # float16(GPU)/float32(CPU)
    "temperature": 0.3,             # 0.1-0.9，越低越稳定
    "top_p": 0.9,                   # 0.8-1.0
    "max_new_tokens": 256,          # 50-512
}
```

### 性能优化

#### GPU显存不足？
```python
# 启用8位量化（节省50%显存）
"load_in_8bit": True

# 或4位量化（节省75%显存，准确率略降）
"load_in_4bit": True
```

#### 只有CPU？
```python
"device_map": "cpu",
"torch_dtype": torch.float32,
```

#### 答题速度慢？
```python
# 使用更小的模型
"model_path": "Qwen/Qwen2.5-1.5B-Instruct"

# 减少生成长度
"max_new_tokens": 128
```

### 脚本配置

在网页脚本控制面板中可以调整：
- 答题间隔（防止检测）
- 自动提交阈值
- 是否使用相似度匹配

---

## 🔧 故障排除

### 问题1：模型加载失败

**症状：**
```
✗ 模型加载失败: ...
```

**解决方案：**
1. 检查模型路径是否正确
2. 确认模型文件完整（需要有 `config.json` 等文件）
3. 检查Python内存是否足够

### 问题2：GPU显存不足

**症状：**
```
RuntimeError: CUDA out of memory
```

**解决方案：**
```python
# 方案1：使用量化
"load_in_8bit": True

# 方案2：使用CPU
"device_map": "cpu"
"torch_dtype": torch.float32

# 方案3：使用更小的模型
"model_path": "Qwen/Qwen2.5-3B-Instruct"
```

### 问题3：无法连接AI服务

**症状：** 脚本提示"AI请求失败"

**解决方案：**
1. 检查服务是否运行：访问 http://localhost:5000
2. 检查端口是否被占用：`netstat -ano | findstr 5000`
3. 检查防火墙设置
4. 确认Tampermonkey已添加 `@connect localhost`

### 问题4：所有题型都识别为简答题

**原因：** 网页题型为繁体中文，未在映射表中

**解决：** 已在第4959-4967行添加繁体支持：
```javascript
"單選題": "0",
"多選題": "1",
"填空題": "2",
"判斷題": "3",
"簡答題": "4"
```

### 问题5：答案不准确

**调整参数：**
```python
"temperature": 0.1,  # 降低创造性，提高稳定性
"top_p": 0.8,        # 限制候选词范围
```

**或使用更大的模型：**
```python
"model_path": "Qwen/Qwen2.5-14B-Instruct"
```

---

## 📊 性能对比

| 对比项 | 在线题库 | 本地AI |
|--------|---------|--------|
| **准确率** | 95%+ | 70-90% |
| **响应速度** | 1-2秒 | 3-10秒 |
| **成本** | 需付费 | 完全免费 |
| **覆盖率** | 有限 | 任意题目 |
| **网络依赖** | 需要 | 不需要 |
| **隐私性** | 低 | 高 |

---

## �️ 技术栈

### 后端服务
- **FastAPI** - 高性能Web框架
- **Transformers** - HuggingFace模型加载
- **PyTorch** - 深度学习框架
- **Uvicorn** - ASGI服务器

### 前端脚本
- **Tampermonkey** - 浏览器脚本管理
- **Vue 3** - 响应式UI框架
- **Element Plus** - UI组件库
- **Pinia** - 状态管理

---

## 📁 项目结构

```
├── jiaoben.js                    # Tampermonkey用户脚本
├── ai_answer_service_local.py    # Python AI服务
├── README.md                     # 项目说明
├── LICENSE                       # MIT许可证
│
├── 启动服务.bat                  # Windows快速启动（可选）
├── 测试服务.bat                  # 服务测试脚本（可选）
│
└── docs/                         # 文档目录（可选）
    ├── 本地模型部署指南.md
    ├── 快速上手.md
    └── 使用指南.md
```

---

## 🤝 贡献指南

欢迎提交问题和贡献代码！

### 如何贡献

1. Fork 本项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

### 问题反馈

- **Bug报告**：请使用 [Issue](链接到你的Issues页面)
- **功能建议**：欢迎在 [Discussions](链接到你的Discussions页面) 讨论

---

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

---

## � 联系方式

- **作者**：spiritofmoon
- **GitHub**：[你的GitHub地址]
- **Email**：[你的邮箱]（可选）

---

## 🌟 Star History

如果这个项目对你有帮助，请给一个 ⭐️ Star，谢谢！

---

## 📝 更新日志

### v0.0.1 (2025-11-28)
- ✨ 完全替换题库API为本地AI模型
- ✨ 支持Qwen系列模型
- ✨ 添加繁体中文题型识别
- 🔧 优化答案解析逻辑
- 📝 完善文档和使用指南
- 🗑️ 移除所有付费题库相关代码

---

<p align="center">Made with ❤️ by spiritofmoon</p>
<p align="center">基于 <a href="https://greasyfork.org/scripts/488083">isMobile的开源项目</a> 改造</p>
