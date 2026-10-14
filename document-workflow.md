# 指标版与矢量版文档工作流

本文件说明 CFD 科研文档可选的版本派生与格式转换操作。写作规则见 [writing-guide.md](writing-guide.md)。首次介绍、再次推荐及执行条件以主指导 P7 为准。读取本文件不等于要求执行命令。

## 0. 何时使用

首次读取主指导时，agent 只向用户简短展示并推荐流程。维护工作本时不启动格式转换。用户出现格式转换意图时再次推荐，按其明确要求执行所需范围。用户只需要 Markdown 阅读本时，到该节点结束，不生成 TeX/PDF，也不检查或安装编译依赖。

## 1. 完整派生链

```text
指标版工作本 md → 指标版阅读本 md → 矢量版阅读本 md
                       │                    │
                       ▼                    ▼
                 指标版阅读本 tex      矢量版阅读本 tex
                       │                    │
                       ▼                    ▼
                 指标版阅读本 pdf      矢量版阅读本 pdf
```

采用完整流程时，先完成矢量版阅读本的记法与内容核对，再进入两路 TeX/PDF 转换。两份 TeX 各自来自对应的阅读本 Markdown，而不是互相转换。完整流程形成三份 Markdown、两份 TeX 和两份 PDF。不要求每次编辑都生成七个文件。技术内容的唯一维护源是指标版工作本，其余文件为派生产物。对外交付以与工作本一致的核定指标版阅读本为数学基准。

下面是采用完整流程时的目录示例。文件名可以调整，按用户当前任务创建所需文件，不提前建立全部产物。

```text
report/
├── report_index_work.md
├── report_index_reading.md
├── report_vector_reading.md
├── report_index_reading.tex
├── report_vector_reading.tex
├── report_index_reading.pdf
├── report_vector_reading.pdf
├── templates/
│   └── physics-header.tex
├── figures/
├── refs.bib                   # 需要正式文献管理时提供
├── README.md
└── build/                     # 编译中间文件与检查记录
```

## 2. 各节点的职责与核对

1. **指标版工作本 md。** 维护完整技术内容、本地索引、版本与核验记录。区分来源事实、假设、推导和待解决事项。定义指标范围、求和及坐标基底。
2. **指标版阅读本 md。** 按受控写作整理工作本，删去无关的内部信息。保留必要推导、参数、证据和适用条件。技术内容与工作本核对，公式编号保持对应。
3. **矢量版阅读本 md。** 从指标版阅读本转换物理指标。先约定运算符，再检查收缩、转置、几何含义和符号冲突。保留节点、层和时间步等必要标签。两版公式编号保持对应。
4. **两份阅读本 tex。** 各自由对应 Markdown 生成完整独立文档。清点并核对公式、编号、章节、图表和链接。使用相同排版配置，分别保存文件。
5. **两份阅读本 pdf。** 分别编译和检查。渲染页面，检查公式、表格、caption、字体和链接。核对 PDF 及 TeX 与各自 Markdown 的内容。
6. **登记与交付。** README 标明维护源、各版用途和优先级，提供构建说明与必要资源。记法冲突或矢量版有歧义时，以核定指标版为准。

内容核对可以用脚本提取数学片段、编号、图表和引用。文字改写应保留数学片段。矢量转换需要等价性检查，不能要求与指标式逐字相同。清点用于找遗漏，不能替代假设判断、数学等价核对或物理验证。

技术内容修正从指标版工作本开始，并记录哪些派生产物需要更新。只有用户要求更新或转换时，才按指标版阅读本、矢量版阅读本和所需 TeX/PDF 的顺序处理。已有核定节点可以复用，只改表达或排版时不重做无关步骤。排版修正保存到配置或转换记录，不从 PDF 反推正文。影响结论的未解决问题须在阅读本说明。矢量式等价性未核实时，记录并修正，不把该转换称为已核定。

## 3. Markdown 转 TeX

以下是使用 Pandoc 和 XeLaTeX 的一种实现。`templates/physics-header.tex` 是随附的可选排版配置。转换使用 Pandoc 默认 LaTeX 模板，再载入该 header。完整 TeX 文档和 preamble 片段的职责不同，不把完整文档作为另一份完整文档的正文。

用户已要求执行相应转换时，再检查 Pandoc、XeLaTeX、latexmk 及所需 TeX 包是否可用。以下命令在报告项目根目录执行，假设两个阅读本和配套资源已完成审定。配置采用 TeX 随附的 Fandol 字体，减少对特定操作系统字体的依赖。

命令示例展示两版生成方法。用户只要求其中一版时，只运行该版命令。

```bash
set -e
for report_stem in report_index_reading report_vector_reading; do
  pandoc "$report_stem.md" \
    --from=markdown+tex_math_dollars+tex_math_single_backslash+raw_tex \
    --to=latex --standalone --pdf-engine=xelatex \
    --include-in-header=templates/physics-header.tex \
    --resource-path=.:figures \
    --variable=documentclass:article \
    --variable=fontsize:11pt \
    --variable=papersize:a4 \
    --variable=geometry:margin=1in \
    --output="$report_stem.tex"
done
```

要加目录，可加 `--toc`。标题和作者可以使用 Markdown YAML 元数据。不要将维护路径或内部批次写入标题。正式文献可以提供 `refs.bib`，再在转换命令加 `--citeproc --bibliography=refs.bib`，并按实际投稿要求指定 CSL。没有文献库时，已经写在 Markdown 中的普通链接仍按链接转换，不凭空生成 `refs.bib`。

数学公式使用 Pandoc 能识别的 `$...$`、`$$...$$` 或反斜线数学定界符。原始 `\tag{...}` 可在数学环境中保留，但自动编号和交叉引用需要另行明确方案，并检查结果。不要把每行公式机械包成单独环境，也不要自动缩小全部公式以掩盖溢出。

Mermaid、交互 HTML 及部分扩展 Markdown 不会自动成为可用的 TeX 图。先导出 PDF/PNG 等静态图，并在阅读本引用静态文件。需要交互体验时在 caption 或配套说明中给出链接。复制所需图片和参考文件，避免只有本机可访问的绝对路径。

## 4. TeX 转 PDF

```bash
set -e
for report_stem in report_index_reading report_vector_reading; do
  latexmk -xelatex -interaction=nonstopmode -halt-on-error \
    -file-line-error -outdir="build/$report_stem" "$report_stem.tex"
  cp "build/$report_stem/$report_stem.pdf" "$report_stem.pdf"
done
```

两份 TeX 分别有一个 `\documentclass` 和一个 document 环境。已有完整文档直接独立编译。不能将它 `\input` 到另一份已经开始的完整文档中。

处理编译错误、缺字、未定义引用及明显溢出。按发生位置调整长公式、定宽表格或路径断行，保持数学含义。下面的扫描检查 TeX 中的裸希腊字符。命中应逐项更正，没有命中时 `rg` 返回 1 是正常结果。

```bash
rg -n '\p{Greek}' -g '*.tex' .
```

## 5. PDF 检查与交付

使用 PDF 渲染工具查看全部页面，并放大检查长公式、宽表、符号表和流程图所在页。可用以下命令取得文字和页面图像。它们是检查素材，不是自动判定正确的工具。

```bash
mkdir -p build/pdf-check
pdftotext -layout report_index_reading.pdf build/pdf-check/index.txt
pdftotext -layout report_vector_reading.pdf build/pdf-check/vector.txt
pdftoppm -r 120 -png report_index_reading.pdf build/pdf-check/index
pdftoppm -r 120 -png report_vector_reading.pdf build/pdf-check/vector
```

核对两版编号对应、数学内容、必要条件和证据，检查裁切、重叠、缺字及链接目标。对向量转换核对等价性，对文字改写核对信息保全。确认交付文件、相对资源和 README 一起可用。发布到远端后再次核对文件内容及版本说明。

所需依赖与可选配置说明见 [templates/README.md](templates/README.md)。确认用户需要 PDF 后，再按实际报告内容完成编译与页面检查，不因 Markdown 维护而自动执行。
