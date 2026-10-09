# 2025 年食品公司半年报问答知识库

作业 3 · 方向 A。选取 10 家食品公司的 **2025 年半年度报告全文**，从[巨潮资讯](https://www.cninfo.com.cn/)的官方 PDF 建立问答库。报告清单及原文链接见 [reports.json](report_qa/reports.json)。

## 实现与结果

- 按 PDF 页提取文字；将表格按版面空白切成行和列，并给文本块附上公司、章节、期间、PDF 页码。
- 用中文 BGE 向量模型和 BM25 分别检索，合并排名；DeepSeek 根据召回块回答，引用可跳转到官方 PDF 页。
- 10 份报告共 1,644 页、5,241 个文本块。10 道题中 8 题正确、1 题错误、1 题部分正确；[逐题召回与人工核对](report_qa/evaluation.json)。

跨公司题的主要问题是召回块集中在少数公司；表格中合并单元格和跨页内容也需要人工核对。真实结果与改进方向写在[一页结论](docs/conclusion.pdf)中，页面效果见[问答截图](docs/qa.png)。

## 运行

需要 Python、系统命令 `pdftotext`（Poppler）和 [requirements.txt](requirements.txt) 中的依赖。在已具备这些依赖的环境中，从本目录运行：

```bash
python -m report_qa.build
python -m streamlit run report_qa/app.py --server.port 8504
```

浏览器打开 `http://localhost:8504/`。下载的 PDF 和索引保存在 `~/.unravel/finance-homework/`，不放进 Git 仓库。`build` 只需在首次运行或更新报告清单后执行。

将 `.env.example` 复制为 `.env`，填入 `DEEPSEEK_API_KEY`；`.env` 已被忽略。当前机器的 `.env` 已配置，密钥不会显示在代码、截图或报告中。

要重新运行 10 题并生成原始记录：

```bash
python -m report_qa.evaluate
```

重新运行会覆盖 `report_qa/evaluation.json` 中的人工判定，提交前需再次对照 PDF 原文核对。
