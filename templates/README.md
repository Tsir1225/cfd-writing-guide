# 可选 TeX 配置

本目录的配置仅用于用户已要求的 TeX/PDF 转换。首次读取写作指导或维护 Markdown 工作本时，不启动转换、编译或安装。

## physics-header.tex

`physics-header.tex` 是 preamble 片段，没有 documentclass 或 document 环境。通过 Pandoc 的 `--include-in-header` 使用，命令见 [document-workflow.md](../document-workflow.md)。

配置提供 ctex/Fandol、表格支持、英文图表标题和可选的原文块。完整文档结构以及数学、图片、链接和长表辅助功能由 Pandoc 默认 LaTeX 模板提供。页尺寸和页边距由转换命令指定，避免重复加载 geometry 时出现选项冲突。

使用时需要 Pandoc、XeLaTeX、latexmk 及相应 TeX 包，包括 ctex/Fandol、array、booktabs、xcolor 和 fancyvrb。Pandoc 会按内容使用其他所需包。Fandol 减少对特定操作系统字体的依赖，仍需在实际使用环境中确认包和字体可用。

用户未要求格式转换时，无需检查这些依赖。该配置不包含期刊专属格式，也不直接导出 Mermaid 或交互 HTML。正式论文按投稿要求选择文献样式和排版模板。
