
# AI Vocabulary Parser: From PDF to Anki

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![DeepSeek API](https://img.shields.io/badge/DeepSeek-API-green.svg)](https://platform.deepseek.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

将任何杂乱排版、甚至扫描版的PDF单词书（如四六级、托福词汇），通过AI能力自动解析为结构化的CSV文件，并一键导入 [Anki](https://apps.ankiweb.net/) 进行高效记忆。

**核心理念**：人类定义规则，AI执行解析。解决传统爬虫或正则表达式无法处理复杂、非结构化单词排版的问题。

## ✨ 核心特性

- **🤖 AI驱动解析**：集成DeepSeek API（兼容OpenAI接口），智能识别单词、音标、释义、短语、例句等字段
- **📄 抗噪能力强**：专门处理真实世界的非规则排版，包括：
  - 音标位置不固定（同行/换行/括号包裹）
  - 多种释义格式（数字编号/符号/混合文本）
  - 夹杂记忆法、同义词、真题来源等冗余信息
  - 图片型PDF（需配合OCR，预留接口）
- **⏱️ 稳健的处理策略**：
  - 防截断：自动分块输入，避免超限
  - 防遗漏：逐条/分批循环调用，确保完整
  - 续写机制：CSV断点续传，意外中断不丢数据
- **⚡ 多种性能模式**：单线程（稳定）、批量（快速）、多线程（极限）

## 🚀 快速开始

### 环境准备

1. **获取DeepSeek API密钥**：
   - 访问 [DeepSeek官网](https://platform.deepseek.com/) 注册
   - 新用户赠送500万tokens免费额度

2. **安装依赖**：
```bash
pip install pymupdf openai pandas
```

3. **配置API密钥**：
```python
DEEPSEEK_API_KEY = "sk-your-api-key-here"
```

### 基础用法

```python
from parser import VocabParser

parser = VocabParser(api_key="your_api_key", mode="single")  # single/batch/multi_thread
parser.process_pdf(
    pdf_path="四级单词.pdf",
    output_csv="my_vocab.csv"
)
```

### 运行压力测试

```bash
python test_parser.py --stress-test
```

内置12种复杂排版问题测试样本，用于验证AI解析能力。

## 📊 性能与成本估算（3000词实测）

| 处理模式 | 预计耗时 | API调用次数 | 推荐场景 |
|:---|:---|:---|:---|
| 单线程 | 25-35分钟 | 3000次 | 稳定可靠，推荐 |
| 批量（batch=10） | 8-10分钟 | 300次 | 排版规整时使用 |
| 多线程（10并发） | 3-5分钟 | 3000次 | 急用且API支持高并发 |

**成本参考**：DeepSeek API约 ¥1元/百万tokens。处理3000词约消耗20-30万tokens，**总成本低于 ¥0.1元**。

## 📖 处理逻辑示例

原始文本（杂乱排版）：
```
consume kənˈsjuːm 【动词】①消耗，消费 ②吃，喝 ③充满（感情）
短语：be consumed with jealousy
（例句：The car consumes a lot of fuel.）
```

AI输出JSON：
```json
{
  "word": "consume",
  "pronunciation": "/kənˈsjuːm/",
  "definition": "动词：①消耗，消费 ②吃，喝 ③充满（感情）",
  "phrase": "be consumed with jealousy",
  "sentence": "The car consumes a lot of fuel."
}
```

## 📁 项目结构

```
.
├── parser.py             # 核心解析逻辑
├── pdf_extractor.py      # PDF文本提取与分块
├── ai_client.py          # DeepSeek API封装
├── config.py             # 配置文件
├── test_parser.py        # 压力测试脚本
├── requirements.txt      # 依赖列表
└── README.md             # 项目说明
```

## 🤝 贡献

欢迎提交Issue或Pull Request。



