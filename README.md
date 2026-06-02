项目名称：基于 Word VBA 的学术报告与课程设计自动化排版脚本
1. 脚本的作用与核心功能
该脚本旨在解决学术报告、课程设计或学术论文在 Word 中手动排版繁琐、易出错的问题。通过在 Word VBA 控制台中运行，可实现一键格式化，具体功能包括：
封面保护机制：脚本会自动识别并跳过第1页，确保原有的个性化封面设计、字号及间距不受到任何影响。
标题分级格式化：自动识别并调整一级标题（黑体小二，行距2.0，段前段后0.5行，顶格）、二级标题（楷体三号，行距2.0，段前段后0.5行，顶格）、三级标题（宋体小四，行距2.0，段前段后0.5行，顶格）。
正文自动缩进：自动将所有普通正文段落设置为宋体小四、行距1.5，并自动应用首行缩进2字符（段前空四格）。
图表与说明格式化：自动居中对齐表格并调整表格内文字（宋体五号，行距1.5），同时自动居中对齐所有非第一页的图片说明。
参考文献规范化：自动定位“参考文献”标题后的文献条目，统一调整为宋体五号、行距1.5。
差异化页脚页码：自动将第1节（摘要与目录）的页码格式设为罗马数字（I, II...），将第2节（正文）页码设为从1开始的阿拉伯数字（1, 2...）。
正文页眉自动生成：自动在正文节中添加居中的宋体小五号字体的指定页眉，而封面与目录页无页眉。
全篇去粗与防错：全局应用常规字重（不加粗），并采用动态绑定技术（Late Binding）规避了不同 Office 版本间类型库引用丢失导致的编译错误。
2. 使用环境
操作系统：Windows 10/11 或 macOS（支持 Office 运行环境）。
办公软件：Microsoft Word（建议 2016 及以上版本、Microsoft 365）或支持 VBA 宏运行的 WPS Office 软件。
文档前置条件：在运行脚本前，需要在“摘要/目录”与“正文开始”之间插入一个“分节符（下一页）”，以便脚本能够正确分节应用不同的页码和页眉系统。
3. 使用步骤
第一步：打开需要排版的 Word 文档，将光标定位在正文最前方（例如一、设计要求与课题描述），点击顶部菜单栏的 布局 -> 分隔符 -> 分节符（下一页） 进行分节。
第二步：在 Word 界面中，按下快捷键 Alt + F11 打开 VBA 代码控制台。
第三步：在 VBA 窗口顶部菜单中，点击 插入 -> 模块。
第四步：将完整的 VBA 脚本代码复制并粘贴到右侧新弹出的空白代码编辑器中。
第五步：按下键盘上的 F5 键，或者点击工具栏上绿色的“运行”三角形图标。
第六步：等待数秒后，当弹出“代码运行顺利！所有格式排版已应用完毕”的提示框时，关闭 VBA 窗口即可。

# IoT Intelligent Operations & Maintenance Agent (IoT-O&M Agent)

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)
[![LangGraph](https://img.shields.io/badge/Framework-LangGraph-orange.svg)](https://github.com/langchain-ai/langgraph)
[![Docker](https://img.shields.io/badge/Docker-Supported-blue.svg)](https://www.docker.com/)
[![License-MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

An industrial-grade AI Agent designed to solve the pain points of Internet of Things (IoT) equipment operations and maintenance. It features long/short-term memory management, a dual-engine hybrid knowledge retrieval layer, and a runtime hallucination self-auditing mechanism to automate fault diagnosis and maintenance plan generation.

---

## 🌟 Key Features

- **Advanced Topology Execution (LangGraph)**: Built on a ReAct loop mechanism leveraging multi-node state graphs for intent classification, autonomous tool triggering, and recursive rewriting.
- **Dual-Engine Knowledge Stratum**: 
  - **Unstructured RAG**: High-precision semantic searching inside equipment maintenance manuals using **ChromaDB**.
  - **Structured Data Query**: Correlating real-time IoT device metrics via **SQLite** utilizing complex relational `JOIN` statements.
- **Robustness & Hallucination Mitigation**: Integrated `Runtime Auditor` node to validate factual grounding before rendering the response, reducing LLM hallucination.
- **Enterprise-Grade Observability**: Full-lifecycle tracing using **LangSmith** alongside rigorous Python standard asynchronous exception handling.

---

## 🏗️ System Architecture

The following diagram illustrates the Agent's workflow topology and data routing, powered by **LangGraph**:

```mermaid
graph TD
    %% Base Styling
    classDef stateNode fill:#f9f,stroke:#333,stroke-width:2px;
    classDef dbNode fill:#bbf,stroke:#333,stroke-width:2px;
    classDef toolNode fill:#bfb,stroke:#333,stroke-width:2px;

    %% Workflow Nodes
    Start([User Query / Alert Trigger]) --> StateInit[Initialize Agent State]
    StateInit --> Supervisor{Intent Route}
    
    %% Router Decisions
    Supervisor -->|Manual Lookup| RAG_Node[ChromaDB RAG Node]:::stateNode
    Supervisor -->|Live Status| SQL_Node[SQLite Metric Node]:::stateNode
    
    %% Vector & Relational DBs
    RAG_Node <--> DB_Chroma[(ChromaDB Vector Store)]:::dbNode
    SQL_Node <--> DB_SQL[(SQLite Relational DB)]:::dbNode
    
    %% Execution & Review Loop
    RAG_Node --> Auditor{Runtime Auditor}
    SQL_Node --> Auditor
    
    Auditor -->|Hallucination Detected| RewriteNode[Recursive Rewrite Node]:::stateNode
    RewriteNode --> Supervisor
    
    Auditor -->|Verified & Grounded| Response[Format Actionable Solution]
    
    %% Monitoring
    Response --> LangSmith([LangSmith Tracing & Logging])

    %% Node Types Legend
    style Supervisor fill:#ffb,stroke:#333,stroke-width:2px;
    style Auditor fill:#ffb,stroke:#333,stroke-width:2px;
