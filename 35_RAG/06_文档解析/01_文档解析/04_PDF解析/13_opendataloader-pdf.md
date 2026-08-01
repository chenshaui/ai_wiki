# 不要云端，纯 CPU 跑文档解析，每秒100页，碾压 mineru

## 01 每秒 100 页，纯 CPU 跑

PDF 转 Markdown 这件事，做到今天还没解决好。

MinerU 慢到 5 秒一页，marker 要 53 秒一页，markitdown 只转文字不转排版。

**opendataloader\-pdf** 做到什么程度？

- • 纯 CPU 跑，**每秒 60 页**，多核多进程**超 100 页/秒**

- • Apple M4 基准测试，0\.015 秒一页

- • 复杂表格、扫描件、多栏布局——自动启用 hybrid 模式

- • 不要 GPU、不要云端、连 API Key 都省了

▲ 16\.8K Star，Apache 2\.0，LangChain 已原生集成。

## 02 精度对比：全面领先

纯模式比 mineru 快 **400 倍**，hybrid 比 mineru 快 **12 倍**，精度还更高。

## 03 怎么做到的？

混合识别管线：简单页直走文本提取（0\.015 秒），复杂页自动走 hybrid 识别（0\.46 秒）。**不暴力 OCR，不一刀切。**

- • 数字 PDF → 直读，秒级出 Markdown

- • 扫描 PDF → 自动 OCR，80\+ 语言

- • 多栏布局 → XY\-Cut\+\+ 算法保阅读顺序

- • 表格 → 自动识别，93% 准确率

- • 页眉页脚 → 智能过滤

每个元素都有精确的位置信息——页码、bounding box、坐标。

## 04 RAG 友好度

PDF 解析器在 RAG 场景里最大的问题是**不保真**。LLM 回答了某个数据，你不知道它到底从 PDF 哪一行拿到的。

opendataloader\-pdf 的解决方式：**每个元素都绑定精确位置。**

输出里每一个段落、每一行、每一张表格，都带着它在原始 PDF 中的坐标和页码。LLM 回答时引用哪一段，你能追溯到原始 PDF 的那一行。

还有一层**安全过滤**——自动检测并滤除 PDF 里的隐性 prompt injection。

## 05 安装

```Plain Text
pip install opendataloader-pdf
```

```Plain Text
import opendataloader_pdf as odl

result = odl.convert(
    input_path="report.pdf",
    output_dir="output/",
    format="markdown,json"
)
```

批量处理：

```Plain Text
# 一次处理多个 PDF
opendataloader-pdf file1.pdf file2.pdf folder/

# 扫描件自动 OCR
opendataloader-pdf --force-ocr --ocr-lang "ch_sim,en" scan.pdf

# 复杂表格 hybrid 模式
opendataloader-pdf --hybrid docling-fast complex.pdf
```

LangChain 也可以直接调用：

```Plain Text
from langchain_opendataloader_pdf import OpenDataLoaderPDFLoader
loader = OpenDataLoaderPDFLoader("report.pdf")
docs = loader.load()
```

## 06 写在最后

PDF 解析跑在 GPU 上，这件事本来就不合理。

**opendataloader\-pdf 证明了纯 CPU 方案也能把速度和精度都做到第一。**

LangChain 原生集成——RAG 管线的标配。

项目地址：https://github\.com/opendataloader\-project/opendataloader\-pdf
文档：https://opendataloader\.org





https://mp\.weixin\.qq\.com/s/24I2CreFyBQ9ZMBpKGXjwg

