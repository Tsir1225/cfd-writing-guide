# cfd-writing-guide

面向计算流体力学（computational fluid dynamics，CFD）研究者及其 agent 的科研写作指导。适用于理论推导、模型说明、数值方法、验证与比较报告，以及面向合作者的阅读版本。内容包括受控语言、指标与矢量记法、线图和热图，以及按需启用的格式转换工作流。

## 快速开始

只使用写作指导时，无需安装软件或编译文档。下载 [writing-guide.md](writing-guide.md)，或克隆本仓库：

```bash
git clone https://github.com/Tsir1225/cfd-writing-guide.git
```

将下列提示交给你的 agent，并提供指导文件：

> 请阅读 writing-guide.md，并将其作为后续 CFD 科研文档的写作规则。首次读取时，简短介绍并推荐文档工作流。维护工作本时，不自动派生阅读本或启动格式转换。用户出现格式转换意图时，再次推荐，并按已明确要求的格式和范围执行。

推荐的完整链为：**指标版工作本 md → 指标版阅读本 md → 矢量版阅读本 md → 两种阅读本 tex → 两种阅读本 pdf**。这表示版本依赖顺序，不要求每次写作生成全部七个文件。只需要 Markdown 阅读本时，完成该文件即可。

## 文件与入口

本仓库按用途保留少量入口文件。写作指导可以单独分发。需要操作命令或可选排版配置时，再提供对应资源。

```text
cfd-writing-guide/
├── README.md
├── writing-guide.md
├── document-workflow.md
├── templates/
│   ├── README.md
│   └── physics-header.tex
├── LICENSE
├── CONTRIBUTING.md
└── .gitignore
```

- [写作指导](writing-guide.md)：八项规则，包含线图与热图的专门小节。
- [文档工作流](document-workflow.md)：可选的派生顺序、转换命令和检查节点。
- [模板说明](templates/README.md)：可选 TeX 配置的用途、依赖与启用条件。
- [贡献指南](CONTRIBUTING.md)：如何报告问题、提出或修订规则。

阅读指导不启动转换或依赖安装。只有用户要求 TeX/PDF 时，才按相应任务检查和使用 Pandoc、XeLaTeX 等工具。

## 规则的适用范围

这些规则是本指南的默认写作约定。用户明确要求、研究项目既有约定及目标期刊格式要求优先。发现冲突时，说明冲突并按当前任务解决。

指标记法优先、英文句长、摘要结构和实验表格用词等属于可调整的写作约定，不作为物理定律或所有期刊的统一要求。采用规则时保留它们的适用条件。

本项目提供可供 agent 遵循的文档规则与操作说明，没有内置自动审查器。规则中的核对步骤仍需由作者及其工具执行。英文阅读本采用 ASD-STE100 的受控写作要求。中文借用清晰表达原则。本仓库不附带 ASD-STE100 完整标准或词典，相关资料可从 [ASD-STE100 官方网站](https://www.asd-ste100.org/) 取得。

## 维护与贡献

欢迎通过 Issue 提供问题和例子，或通过 Pull Request 提交修改。新增规则需说明适用范围、理由及与现有规则的关系，具体要求见 [CONTRIBUTING.md](CONTRIBUTING.md)。

维护者的工作档案、分发 ZIP、操作系统文件和编译中间文件不纳入版本库。公开阅读本不依赖这些材料或维护者的电脑。

## 许可证

本仓库的文档和模板统一采用 [MIT License](LICENSE)。转载、修改或分发时保留许可证要求的版权与许可说明。外部链接所指资料遵循各自的许可。
