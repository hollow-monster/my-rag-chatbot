啊，我看到截图了，非常抱歉！这是因为我在上一次回复时，把整个文档包在了一个大的代码块里，而文档内部又包含了几个小的 `bash` 代码块。这种“代码块套代码块”的格式让聊天界面的渲染器“晕头”了，导致显示断层。

为了让你能完美复制，我这次**直接把内容输出**，不套外层的代码框了。

请你在项目根目录下新建 `README.md` 文件，然后直接全选复制下面这条分割线以下的所有内容：

---

# 🤖 RAG 智能客服助手 (RAG Intelligent Chatbot)

这是一个基于 **RAG (检索增强生成)** 技术构建的本地化智能客服问答系统。本项目允许用户将个人的本地知识库（如 TXT 文档）上传至向量数据库，并利用大语言模型（LLM）基于这些私有知识进行精准作答，有效避免了大模型的“幻觉”问题。

项目具备完整的**多轮对话记忆**能力与丝滑的**流式打字机输出**体验，并提供了基于 Streamlit 的 Web 可视化界面。

---

## ✨ 核心特性

* **📚 本地知识库建库：** 支持 TXT 文本的一键上传与切分，利用 MD5 算法自动实现“防重复录入”。
* **🔍 语义检索：** 接入 Chroma 向量数据库，通过词向量化（Embeddings）实现高精度的相似度匹配。
* **🧠 长期记忆管理：** 自定义本地 JSON 历史记录存储模块，让大模型拥有上下文连贯的多轮对话能力。
* **⚡ 流式输出体验：** 采用 Server-Sent Events (SSE) 类似机制，实现字字输出的打字机视觉效果。
* **🌐 Web 可视化交互：** 包含“知识管理端”与“用户对话端”双后台 Web 界面。
* **☁️ 云端兼容：** 内置 `pysqlite3` 补丁，完美支持一键部署至 Streamlit Community Cloud。

---

## 🛠️ 技术栈 (Tech Stack)

* **前端框架：** [Streamlit](https://streamlit.io/)
* **大语言模型应用框架：** [LangChain (LCEL语法)](https://www.langchain.com/)
* **向量数据库：** [ChromaDB](https://www.trychroma.com/)
* **LLM & Embedding 模型：** 阿里云百炼 [DashScope (通义千问 Qwen3.7-Max)](https://www.google.com/search?q=https://help.aliyun.com/zh/dashscope/)

---

## 📂 项目文件结构

* `app_qa.py`：主程序入口，面向用户的**智能客服对话 Web 界面**。
* `app_file_uploader.py`：后台工具，面向管理员的**TXT文件上传与向量化 Web 界面**。
* `rag.py`：核心业务逻辑，使用 LCEL 构建的 RAG 数据流水线（包含检索、提示词模板、记忆拼装与 LLM 调用）。
* `knowledge_base.py`：知识库服务，负责文本的读取、按规则切块（Chunking）以及存入向量数据库，含 MD5 去重逻辑。
* `vector_stores.py`：检索服务，负责连接本地 Chroma 数据库并实例化 Retriever（检索器）。
* `file_history_store.py`：记忆服务，通过继承 `BaseChatMessageHistory` 实现的本地 JSON 文件对话记录持久化。
* `config_data.py`：全局配置文件，统管所有模型名称、切割长度、匹配阈值及存储路径。

---

## 🚀 快速开始 (Quick Start)

### 1. 克隆与环境配置

确保你的电脑已安装 Python 3.8+ 版本。在项目根目录下，安装依赖清单：

```bash
pip install -r requirements.txt

```

### 2. 配置 API 密钥

本项目默认使用阿里云通义千问的大模型服务。在使用前，请确保在部署环境的系统变量或 Streamlit Secrets 中配置你的 API Key：

```bash
DASHSCOPE_API_KEY="sk-你的真实密钥"

```

*(注：为保护数据安全，请勿将写有明文密钥的代码上传至公开仓库！)*

### 3. 运行本地服务

本项目包含两个独立的前端入口，请在命令行中按需启动：

**启动知识入库端（上传资料）：**

```bash
streamlit run app_file_uploader.py

```

**启动智能对话端（客服聊天）：**

```bash
streamlit run app_qa.py

```

---

## 🤝 后续优化方向 (To-Do)

* [ ] 增加对 PDF, Word (docx), CSV 表格等更多文件格式的解析支持。
* [ ] 引入 BM25 关键词检索，实现与向量检索结合的“混合检索 (Hybrid Search)”。
* [ ] 丰富前端 UI，增加知识片段溯源展示（展示大模型回答的具体参考来源）。