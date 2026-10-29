# SceneCall

项目范围为视频生成与编辑：文案、分镜、素材、动画、运镜、声音、字幕、剪辑、审片修复和成片导出。英语知识视频从[制作 SOP](docs/english-knowledge-sop.md)开始，配有工作表、写稿提示词和英文简报示例。

新创作统一从[意图识别与流程分配](docs/creation-intent.md)开始：`scenecall create plan "创作要求" --out 新目录`。按任务、题材、语言、时长、声音和画面要求选择现有流程与必要能力，再准备工程；仅文案、局部修改和完整制作分别处理。[学习成果总览](docs/learning-evolution.md)整理全部主要演进，机器目录归为8类26项，后续学习经 `knowledge register/check` 归档，交付用 `create audit` 记录实际采用。当前暂停继续采集学习。

准备后按 `stage-prompts/` 执行当前阶段。作者场景、整片风格和图解在渲染前用 `create check` 对照真实输入，导出时携带 `--intent creation-plan.json` 再次检查；只改运镜不会重写文案，时长上限也不会被当成必须填满的目标。检查匹配只证明配置与时间线一致，实际动画和文案仍须审阅。

原创知识动画采用[逐句设计与试镜流程](docs/knowledge-authoring.md)，后续制作对标[外洗样片的动画质量基准](docs/knowledge-authoring.md#动画质量基准完美外洗样片)；可编辑示例见[三维外洗](examples/knowledge-shuffle-3d/README.md)与[石拱样片](examples/knowledge-arch/storyboard.md)。具体作品资产留在示例工程，通用渲染器不按每个主题增加模块。

批量学习参考片使用[十条一轮的双任务流程](docs/video-learning/README.md)：每条并发独立盲剪 A 与全片复刻 R，隔离工程、封存后比较，再制作原创改进 B；十条顺序推进，轮末做新题验收并总结。它是执行规范，未新增自动调度器或强制隔离沙箱。

科普与通用素材剪辑现已提供独立入口：`storyboard build` 渲染来源约束的动态图解，`studio` 打开分镜制作台，`edit studio` 打开多轨时间线与按词审阅界面，`edit` 完成裁切、变速、叠加、混音、按词剪辑和素材组装，`package` 生成可校验的交付 ZIP。这些入口不依赖对照的六个项目，也不自动启动 TTS。见[独立剪辑使用说明](docs/editing.md)、[图解与配音绑定](docs/storyboard.md)和[逐项目能力及尚未移植项](docs/capability-migration-2026-10-15.md)。

SceneCall 是一个本地优先、可恢复、可编辑的影视解说剪辑 CLI。配置好本地转写模型、文案来源和 TTS 后，`movie auto` 可以从一部本地电影自动生成解说文案、配音、字幕和剪辑成片，全程无需人工 Review 或批准。文案支持本机已登录的 Codex CLI，也支持本地或远端模型的兼容接口；配音可使用服务器上的 Qwen3-TTS。项目、素材、剪辑和节目检查保留在本地。

自动流程已通过[真实 MKV 电影片段全流程验证](docs/real-movie-codex-smoke-2026-10-02.md)：3 分钟原片经本机 ASR、Codex 和服务器 Qwen3-TTS，导出 37 秒、3 段配音、14 条字幕的成片，重跑复用缓存。另有 HTTP/Codex 两种来源的受控集成测试、[早期短样本](docs/codex-movie-smoke-2026-10-01.md)和[独立配音测试](docs/qwen3-tts-smoke-2026-10-01.md)。完整长片和真实本地文案模型仍待实测，ASR 存在术语误识及长静音对齐问题。当前尚未加入视觉理解；选镜不能保证每句话对应实际动作。已有配音可通过 `movie finish` 进行 ASR 时间戳引导的字幕校准和像素烧录，尚非音素级强制对齐。

最新完成[十支 YouTube 参考解说的研究与项目验证](docs/youtube-commentary-study-2026-10-02.md)：参考字幕统计进入文案上下文，配音后按真实音长选择多个原片位置。导出 28.5 秒、8 个取景窗口的真实试片；中途服务和连接中断经显式恢复后完成，技术检查与缓存重跑通过，不能据此宣称可完全无人值守。

针对试片文案流水账的问题，新增 [`movie write` 独立写稿](docs/movie-writing.md)：可补充带时间码的故事观察与 ASR 纠错，围绕人物和变化改稿，保存可审读的多版本正文。它不依赖 TTS，不替换现有成片；措辞检查和技术测试均不代表叙事质量合格。

科普视频现有 [`storyboard write/check/build` 文案与动态图解流程](docs/storyboard.md)：来源简报进入本机 Codex 或本地兼容模型，支持旁白与文字主导两种呈现，按阅读和动作分配镜头时长。16类本地图解包括多节点中继/阻断、跨阶段资源收支与共同基准剖面，生成素材进入现有项目素材库与时间线，不依赖服务器TTS或对照项目。[35条系列科普片逐条学习记录](docs/research/science-series-20261015/README.md)保留文案、图解、剪辑证据与适用边界；[五版比较与能力取舍](docs/video-comparison-integration-2026-10-15.md)记录先前整合依据。

## 一键生成解说成片

安装 `analysis` 依赖，准备已授权的本地 CTranslate2 模型，并按实际服务填写[配置示例](examples/automatic-movie.yaml)。模型、音色和授权声明只需配置一次；API 密钥由运行环境注入，配置文件只保存环境变量名。

使用当前电脑已登录的 Codex 时，选择 [Codex 配置](examples/automatic-movie-codex.yaml)；使用自己启动的本地文案服务时，选择[本地模型配置](examples/automatic-movie-local-model.yaml)。两者都可沿用服务器 TTS，详见[本地文案接入说明](docs/local-narrative.md)。Codex 在本机运行，但通过 ChatGPT 登录时的推理仍在云端。

```bash
uv sync --extra analysis
uv run scenecall movie auto /absolute/path/to/video.mp4 \
  --project ./output/movie-auto \
  --config ./examples/automatic-movie.yaml \
  --json
```

命令自动完成：本地转写 → 原创第三人称解说 → 外部 TTS → 根据实际配音时长剪辑 → 分句字幕 → 节目技术检查 → MP4 导出。返回结果给出 `program.mp4`、`script.json`、`subtitles.srt`、来源清单和发布记录的路径。默认横屏 16:9，支持配置竖屏或双画幅；BGM 可选，AI 生图/生视频非必要。

自动成功的发布状态是 `AUTOMATIC`，并明确记录 `human_reviewed=false`。正常流程不要求 `movie review`、`movie approve` 或 `eval approve`；原人工审阅链路作为可选方式继续兼容。配置字段、字幕播放与恢复方式见[自动剪辑解说指南](docs/automatic-movie.md)。

## 已实现的核心边界

- 带单调 `revision` 的 `scenecall.yaml` 项目配置；写入通过 `flock` 串行化并用 Compare-And-Swap 拒绝陈旧保存，SQLite 还锚定不可变项目 ID 作为预算 namespace，默认同时启用 9:16 与 16:9。
- 强制填写权利声明的本地素材导入、规范项目 CAS 路径，以及分析/渲染入口的大小与 SHA-256 完整性检查。
- SQLite WAL、稳定请求键、阶段租约、幂等复用，以及本地阶段与远端任务的显式 `UNCERTAIN` 对账；Provider job 只以 SQLite 为权威，JSON sidecar 仅供阅读。
- 证据化 `ScriptBeat`/`Claim`/`VisualCue` 校验与自有 Timeline IR。
- `movie auto` 根据全部带时间码的 ASR 片段调用本机 Codex CLI 或 OpenAI-compatible `/chat/completions` 生成原创叙事，并调用 `/audio/speech` 获取 PCM WAV。Codex 使用已有登录态、结构化输出和只读执行；两种来源共用引用校验。先测量配音音长再安排画面；成功结果缓存复用，未知或失败请求不会盲目重复提交。
- 显式、可选的本地电影分析旁路：faster-whisper 只加载本地 CTranslate2 模型，输出词级时间码并校正音轨起点；MovieIndex 绑定精确转写与镜头区间，镜头检测不可用时明确降级为全片单场景。
- 人工路径的 `movie create` 草稿保持 `UNREVIEWED`，完成具名审核后提升计划。抽取式/普通人工编辑计划使用 origin schema v1，proposal 辅助计划使用绑定 Proposal 与 Review 的 origin schema v3。自动路径独立记录 `automatic_origin`、模型与源转写绑定，结构检查通过后生成计划，不伪造人工 reviewer。
- 可选的 `movie rewrite prepare|import|inspect` 提供证据约束的外部模型交换桥；`movie rewrite doctor|run-local` 则以固定参数调用显式本地 `llama-cli` 与单文件 GGUF。两条路径都只产生待人工审核的 NarrativeProposal，不能直接提升为 Claim。`movie review prepare|import|inspect` 将逐 Beat、逐 assertion 和逐 Claim 的人工结论固化为 CAS + SQLite 权威制品；新的 proposal 辅助审批只接受 `movie approve --review-id`，直接组合 `--proposal-id` 与 `--review-file` 会阻断。
- 可解释、可审批的 `StyleProfile`；默认提取 ffprobe 技术特征，并在可选 PySceneDetect 可用时增加镜头分布与快切指标。外部 Observation 只有五个显式命名空间下的有限数值/布尔特征能进入画像，自由文本和结构值会被丢弃。
- 按 Beat 绑定授权本地节目音频；`accepted-extractive` 电影计划可显式从已审阅原片音轨提取所选范围，其他计划可导入人工录音或显式调用可选 Piper CLI 合成。可另行绑定一条具有 `soundtrack-editing` 权利的本地 BGM，循环被展开为可审计 Timeline 片段，渲染时执行旁白轨触发 ducking 与双遍 EBU R128 loudnorm，输出 48 kHz stereo AAC 和 `mov_text` 字幕轨。
- GPT Image 2、Seedream、Seedance、Kling 适配器、预算门禁和本地 Provider 任务记录；支持把授权本地图片作为受审计的生成参考输入。
- Timeline JSON、最小 OTIO JSON、SRT、来源清单、静音 FFmpeg 预览与本地旁白 MP4 导出；正式节目媒体先成为不可变 CAS 候选，经 Eval 和审批后按原字节复制到导出包，不在发布阶段重新渲染。
- 结构 Eval，以及对不可变候选执行一次完整音视频解码的节目级 Eval；覆盖媒体流合同、时长/音画同步、响度/True Peak、黑帧/冻帧/静音和字幕内容/时序/阅读速度。自动路径按通过的 Program Eval 放行，人工路径保留具名批准；两者均校验发布输入指纹。
- 对已批准 StyleProfile 中可解释的镜头节奏和明确字符句长特征计算有限风格距离；高偏差仅产生 WARNING。
- 失败隔离的本地批处理和机器可读 `--json` 输出。

AI 生图/生视频是可选增强，不属于 `run`、Eval 或本地旁白渲染的必经依赖。只有使用者显式执行 `asset generate` 才会进入 Provider 路径；当前成片未采用的生成实验不进入素材权利/来源闭包，但其潜在费用仍保守占用项目预算。生成素材只能作为说明性或合成镜头，不能证明原剧情发生。适配器通过 Mock/契约测试不等于真实账号已接通；当前四家 Provider 均未完成真实付费 smoke test。

完整的[能力矩阵](docs/capability-matrix.md)列出了已实现、部分实现与未实现项。

TTS 暂停时，使用 [本地精修流程](docs/offline-finishing.md) 复用已有配音，校准、烧录字幕并重新导出，无需启动 TTS 或文案模型。 已完成[28.5 秒真实配音离线精修验证](docs/offline-finishing-validation-2026-10-03.md)。

## 安装

要求：macOS/Linux、Python 3.12、FFmpeg/ffprobe。推荐使用 [uv](https://docs.astral.sh/uv/)：

```bash
uv sync --extra dev
uv run scenecall --help
```

可选媒体依赖：

```bash
uv sync --extra dev --extra media
```

本地电影转写是独立的可选 Extra，不会被默认安装：

```bash
uv sync --extra dev --extra analysis
```

`analysis` 只提供 faster-whisper Python 运行时。SceneCall 不下载模型；`movie` 命令必须接收一个已经在本地、且有权用于 ASR 分析的 CTranslate2 模型目录。需要多镜头 MovieIndex 时再安装 `media` Extra；否则索引会明确降级为单场景，转写仍可执行。

本地剧情改写不需要 Python Extra，但需要使用者另行安装兼容的 `llama.cpp`，并显式提供 `llama-cli` 可执行文件与有权用于改写的本地单文件 GGUF。可执行文件必须是原生 ELF 或 Mach-O thin/fat 普通文件；shebang 脚本、命令包装器和其他格式会被拒绝。SceneCall 不接受模型仓库 ID，不下载 GGUF，也不开放任意命令模板或额外 shell 参数。`doctor` 会直接对源 GGUF 做全量哈希、签名和状态复验，不复制模型；`run-local` 只有在缓存未命中时才检查临时磁盘空间，并在私有 `0700` 临时目录中各复制一次可执行文件和 GGUF 后运行，所以需要额外容纳约一份模型大小的临时空间。已知成功/失败路径结束时会清理快照。它设置离线相关环境变量并固定调用参数，但不会创建 OS 级网络沙箱；若需要可验证断网，应在容器、防火墙或隔离主机中执行。

Piper 不属于 SceneCall 的默认依赖。需要本地 TTS 时，在一个明确的 Python 运行时中另行安装 `piper-tts`、下载有权使用的声音模型，并保留同目录的 `<voice>.onnx.json`。SceneCall 不会自动下载模型：

```bash
python3 -m pip install piper-tts
python3 -m piper.download_voices YOUR_VOICE_NAME
```

该安装步骤只说明官方 Piper CLI 的运行前提，不代表任何声音模型的授权、听感或生产可用性已由 SceneCall 验证。

## 最小本地工作流（可选人工路径）

下述人工路径无需外部模型或媒体生成 Provider 配置。预算默认是 `0`，它限制 `asset generate` 的生图/生视频请求；`movie auto` 的外部 LLM/TTS 调用不使用该预算账本，费用按所选服务计收：

```bash
uv run scenecall init demo --path ./demo --json
```

导入有权使用的原片和风格参考。SceneCall 只记录使用者声明，不替使用者核验版权：

```bash
uv run scenecall ingest ./movie.mp4 \
  --project ./demo \
  --role source \
  --declared-by "your-name" \
  --license owned \
  --json

uv run scenecall ingest ./reference.mp4 \
  --project ./demo \
  --role reference \
  --declared-by "your-name" \
  --license licensed-reference \
  --json
```

### 从本地电影生成待审阅草稿（可选）

如果不想从空白 `NarrationPlan` 开始，可以显式运行本地分析旁路。先检查指定 Python 运行时、FFmpeg/ffprobe 和本地模型：

```bash
uv run scenecall movie doctor \
  --model /absolute/path/to/authorized-ctranslate2-model \
  --model-declared-by "model-rights-owner" \
  --model-license "licensed-asr-model" \
  --python-executable "$(command -v python3)" \
  --json
```

`doctor` 会检查完整本地模型树与运行时，但不代表该模型的授权、转写准确率或长片性能已通过 SceneCall 验证。适配器固定 `local_files_only=true`、`word_timestamps=true`、`vad_filter=false`，并在子进程中启用 Hugging Face/Transformers 离线环境变量；模型路径不会被写入项目。

需要先单独查看转写与镜头索引结果时，运行：

```bash
uv run scenecall movie analyze \
  --project ./demo \
  --model /absolute/path/to/authorized-ctranslate2-model \
  --model-declared-by "model-rights-owner" \
  --model-license "licensed-asr-model" \
  --python-executable "$(command -v python3)" \
  --language zh \
  --json
```

只有一个 source 时会自动选择；多个 source 必须加 `--source-asset-id`。选中的素材必须是带音轨的视频，且权利用途包含 `narration-editing`。转写会记录指定音轨的容器时间起点，将词级时间码偏移回原片时间轴。转写和 MovieIndex 文档写入项目 CAS，并绑定 SHA-256、随机 integrity lineage 和单调 epoch；修复或重建制品不会复活旧批准。`.scenecall/state.db` analysis ledger 是 TranscriptArtifact/MovieIndexArtifact 唯一权威，metadata JSON 只是单向可读镜像。升级前仅有 sidecar、没有 ledger 行的旧记录会 fail-closed，需重新运行 `movie analyze` 或 `movie create`。

生成可编辑草稿使用相同本地分析配置；已验证的相同请求会复用本地制品：

```bash
uv run scenecall movie create \
  --project ./demo \
  --model /absolute/path/to/authorized-ctranslate2-model \
  --model-declared-by "model-rights-owner" \
  --model-license "licensed-asr-model" \
  --python-executable "$(command -v python3)" \
  --language zh \
  --target-duration-seconds 90 \
  --json

uv run scenecall movie inspect DRAFT_ID \
  --project ./demo \
  --output ./movie-draft.review.json \
  --json
```

`movie create` 只抽取原对白，将候选分散到全片时间轴，所有 Beat 均保持 `UNREVIEWED`。它不会把 ASR 文本当成剧情事实，不会推断人物、因果、时序或身份，也不会创建 `NarrationPlan`。

如果需要模型辅助改写，先导出一个不含 Provider 凭据、也不会触发模型执行的交换包：

```bash
uv run scenecall movie rewrite prepare DRAFT_ID \
  --project ./demo \
  --output ./movie-rewrite.packet.json \
  --json
```

可以将 packet 交给使用者自行选择、运行并承担数据处理责任的外部多模态或文本模型。模型必须按 packet 的 `output_contract` 与[严格 proposal v1 JSON 合同](docs/workflow.md#44-证据约束的剧情改写可选)返回 proposal 文件；SceneCall 不提供隐式网络上传，也不会把生图/生视频 Provider 当作改写模型。随后导入并检查提案：

```bash
uv run scenecall movie rewrite import DRAFT_ID ./movie-rewrite.proposal.json \
  --project ./demo \
  --json

uv run scenecall movie rewrite inspect PROPOSAL_ID \
  --project ./demo \
  --json
```

也可以显式选择本地 `llama.cpp` 路径。先用 `doctor` 检查可执行文件、GGUF、模型权利和运行时，再用 `run-local` 执行固定的文本 proposal 合同：

```bash
uv run scenecall movie rewrite doctor \
  --model /absolute/path/to/authorized-model.gguf \
  --model-declared-by "model-rights-owner" \
  --model-license "licensed-local-story-model" \
  --model-version "your-safe-model-alias" \
  --llama-cli-binary /absolute/path/to/llama-cli \
  --json

uv run scenecall movie rewrite run-local DRAFT_ID \
  --project ./demo \
  --model /absolute/path/to/authorized-model.gguf \
  --model-declared-by "model-rights-owner" \
  --model-license "licensed-local-story-model" \
  --model-version "your-safe-model-alias" \
  --llama-cli-binary /absolute/path/to/llama-cli \
  --json
```

该运行器不会接受任意 `--command`，SceneCall 本身不会发起模型下载或网络请求，也不会自动进入 `run`/batch/Eval/export。`doctor` 对 help 中必需选项做精确 token 匹配；`run-local` 的单一 deadline 覆盖 stdin 写入、stdout/stderr 排空和进程退出，瞬时 `EAGAIN`/`EINTR` 会重试，stdin 未完整交付就收到 `EPIPE` 则 fail-closed。缓存未命中时实际推理只读取私有快照，并把快照字节的哈希写入运行绑定。不过，这些检查只证明文件形态与本次执行绑定，不证明二进制来自官方 `llama.cpp`、动态库依赖闭包未变化或进程受到 OS 级断网。完整边界见[工作流](docs/workflow.md#45-第一方本地-llamacpp-运行器可选)。

prepare/import/inspect/run-local 的内容结果都保持 `requires_human_review=true`、`plan_created=false`、`factual_claims_created=0` 和 `provider_jobs_created=0`；doctor 不生成内容，`provider_jobs_created` 也为 `0`。Schema、来源和时间码检查只能拒绝结构上无依据的输入，不能证明模型理解正确；提案文字和 assertion 在人工批准前都不是 SceneCall 的事实 Claim。当前本地运行器只有适配器与受控伪运行时契约测试，尚无真实 `llama-cli`/GGUF smoke。

审批有三种互斥模式：

1. 快速验证链路时显式使用 `--accept-extractive`。它只承认“这个时间段能听到这句对白”，不承认任何剧情解读：

```bash
uv run scenecall movie approve DRAFT_ID \
  --project ./demo \
  --by "human-reviewer" \
  --accept-extractive \
  --confirm-transcript-reviewed \
  --confirm-source-clips-reviewed \
  --confirm-claims-reviewed \
  --json
```

2. 选择人工审阅时，先编辑 `movie inspect --output` 生成的副本：只修改每个 Beat 的 `reviewed_narration`、`reviewed_claims`、`included` 和 `review_status`，不改来源、时间码、候选或制品绑定。然后导入该审阅副本：

```bash
uv run scenecall movie approve DRAFT_ID \
  --project ./demo \
  --by "human-reviewer" \
  --review-file ./movie-draft.review.json \
  --confirm-transcript-reviewed \
  --confirm-source-clips-reviewed \
  --confirm-claims-reviewed \
  --json
```

3. 对导入的 NarrativeProposal，先生成专用的完整审阅模板：

```bash
uv run scenecall movie review prepare DRAFT_ID \
  --project ./demo \
  --proposal-id PROPOSAL_ID \
  --output ./narrative-review.json \
  --json
```

模板本身不是权威，也不能直接审批。审核者必须填写非空 `reviewed_by`，把四项 attestations（包括 `narration_claims_complete`）全部确认为 `true`，并对每个 DraftBeat 作 `ACCEPTED`、`EDITED` 或 `REJECTED` 决定。每个 proposal assertion 必须且只能分类一次为 `SUPPORTED`、`MODIFIED` 或 `REJECTED`；每个最终 Claim 也必须且只能由一个 assertion 决定或 `added_claims` 显式补充项解释。完成后导入并检查不可变 Review：

```bash
uv run scenecall movie review import DRAFT_ID ./narrative-review.json \
  --project ./demo \
  --proposal-id PROPOSAL_ID \
  --json

uv run scenecall movie review inspect REVIEW_ID \
  --project ./demo \
  --json
```

最后只用 Review ID 提升计划；`--by` 是独立的最终批准者，允许与 Review 中的 `reviewed_by` 不同，现有三项确认仍然必填：

```bash
uv run scenecall movie approve DRAFT_ID \
  --project ./demo \
  --review-id REVIEW_ID \
  --by "human-reviewer" \
  --confirm-transcript-reviewed \
  --confirm-source-clips-reviewed \
  --confirm-claims-reviewed \
  --json
```

新的 proposal 辅助审批不再接受 `--proposal-id + --review-file` 直连；该组合会提示先执行 `movie review import` 并改用 `--review-id`。普通、无 proposal 的 `movie inspect --output` 审阅副本仍可通过 `--review-file` 提交，`--accept-extractive` 也仍可用；两者都不能与 `--review-id` 混用。三种模式都必须具名审核并显式确认转写、原片片段和 Claim。Review 模式产生 `NarrationPlan` schema v2 + origin schema v3，分别记录 Review 的 `reviewed_by` 与最终 `movie approve --by` 的 `approved_by`，支持双人复核，并绑定精确 Proposal、Review 与草稿不可变 basis；后续 compile/Eval/export 会重验整个闭包。旧 origin v1 抽取式/普通编辑计划仍可读取；旧 origin v2 proposal 计划虽可解析，但没有不可变 Review，发布会 fail-closed，必须重新审阅和批准。

`ACCEPTED` 只表示严格保留模型建议：字段统一去除首尾空白后，词间空格、大小写、标点与零宽字符变化都算编辑，且该 Beat 的全部 assertion 必须为 `SUPPORTED`、不能另加 Claim。Assertion 只能引用本 Beat 的 EvidenceCandidate；`editorial`/`interpretive` 不能以 `SUPPORTED` 直接变成事实 Claim。终态来源/权利复验与 Plan 写入共享项目 metadata mutation lock，遵守 `save_asset` 的权利更新不能插入提交窗口；这不等于防御同一 OS 用户直接修改文件。

这条链路证明的是“具名审阅/批准字符串留下了完整、可追溯且绑定当前输入的人工决定”，不是人员身份认证或电子签名，也不是语义真值、版权/许可真实性、原创性或无逐字复用证明。当前没有原创性 hard gate；该门禁推迟到取得授权 corpus 并用隔离 Golden Set 完成阈值校准后再启用。

这条分析、改写与审批路径的 `provider_jobs_created` 固定为 `0`；它不读媒体生成 Provider 配置，也不会触发 AI 生图/生视频。生图/生视频仍只由使用者显式运行 `asset generate` 才进入。

若计划通过 `--accept-extractive` 审批，可显式复用分析时选定的原片音轨，并按每个已批准证据区间绑定 Beat：

```bash
uv run scenecall narration extract-source \
  --project ./demo \
  --json
```

该命令默认只补齐尚未绑定音频的 Beat；`--replace` 才替换全部现有绑定。它提取一次完整的已选节目音轨以便稳定缓存，但 Timeline 只播放审核过的区间，并按 Transcript 记录的时间轴 offset 校正源位置。输出 `synthetic=false`，不是 Piper/TTS、人工旁白或干净对白 stem；原片节目混音可能同时包含对白、音乐和音效。编辑审阅得到的计划不允许使用该快捷方式，因为改写文案不一定与原声一致。命令不会读取 Provider 配置，`provider_jobs_created` 固定为 `0`。

其他计划若要在 SceneCall 内生成带声 MP4，可导入已录制或由外部 TTS 生成的本地旁白音频。该角色不会成为剧情证据，也不会配置或调用任何媒体生成 Provider：

```bash
uv run scenecall ingest ./narration.wav \
  --project ./demo \
  --role narration \
  --declared-by "your-name" \
  --license owned \
  --json
```

可选 BGM 同样必须是明确授权的本地音频。它不进入剧情源或风格参考集合，默认只取得 `soundtrack-editing` 用途：

```bash
uv run scenecall ingest ./music.wav \
  --project ./demo \
  --role music \
  --declared-by "music-rights-owner" \
  --license licensed-soundtrack \
  --json
```

需要用本地图片约束支持该能力的生成模型时，以独立角色导入；该 Asset 不会加入剧情源或风格 ReferenceSet：

```bash
uv run scenecall ingest ./generation-reference.png \
  --project ./demo \
  --role generation-reference \
  --declared-by "your-name" \
  --license licensed-generation-input \
  --json
```

学习并批准选用风格画像。参考素材必须具有 `style-analysis` 用途，并能被 ffprobe 识别为有限正时长的视频；即使提供外部 `--observations` 也不能绕过该门禁。未提供 `--observations` 时，PySceneDetect 可用且成功会得到 `technical-and-cut-structure`；依赖缺失、场景检测为空或失败则显式降级为 `technical-only`。ffprobe 只验证容器、流和时长，不是逐帧完整解码。可额外通过 `--transcripts` 导入与每个视频 ID、SHA-256 和时长绑定的字幕或 ASR 转写，提取中文字符密度、覆盖率和可用的标点句长统计；画像只保存抽象统计，不保存原文。详见[参考片学习与自动剪辑](docs/reference-learning.md)：

```bash
uv run scenecall style learn cinematic-fast \
  --project ./demo \
  --json
uv run scenecall style inspect --project ./demo --json
uv run scenecall style approve STYLE_PROFILE_ID --project ./demo --json
```

选中的画像会进入 `movie auto` 文案上下文，并用于把较长的转写证据区间压缩成多个原片窗口。更新画像后会重新编排文案和画面；配音仍以实际合成音长为准。它不包含视觉语义识别、逐句音画强制对齐或创作者声音模仿。

如果不使用上述 `movie` 草稿审批旁路，可以生成证据脚本模板。该命令会以 `BLOCKED` 和退出码 `2` 结束，同时写出模板；这是防止系统虚构剧情事实的有意设计：

```bash
uv run scenecall plan --project ./demo --json
```

将模板中的素材 ID、时间码和文字替换为真实值，或从 [`examples/narration-plan.example.json`](examples/narration-plan.example.json) 开始。若已通过 `movie approve` 生成计划，不要再运行 `plan --script` 覆盖它。若使用人工/外部旁白，在根节点 `narration_audio` 中按 Beat 绑定上述 Asset；也可以先导入不含 `narration_audio` 的计划，再显式调用本地 Piper 合成。`accepted-extractive` 电影计划还可使用前述 `narration extract-source`，但普通手工计划和编辑审阅计划不能使用。需要音乐时，再用可选 `background_music` 绑定一个 `music` Asset 及循环、增益、淡入淡出和 ducking 参数。以上本地路径都无需配置生图/生视频 Provider：

```bash
uv run scenecall plan \
  --project ./demo \
  --script ./narration-plan.json \
  --json
uv run scenecall narration synthesize \
  --project ./demo \
  --model /absolute/path/to/authorized-voice.onnx \
  --python-executable "$(command -v python3)" \
  --declared-by "voice-rights-owner" \
  --voice-license "licensed-voice-model" \
  --json
uv run scenecall run --project ./demo --resume --json
uv run scenecall eval doctor --json
uv run scenecall eval run \
  --project ./demo \
  --platform vertical-9x16 \
  --scope program \
  --json
uv run scenecall eval approve EVAL_RUN_ID \
  --project ./demo \
  --by "human-reviewer" \
  --json
uv run scenecall export --project ./demo --platform vertical-9x16 --render --json
```

`eval run` 默认是 `--scope structural`，适合只导出 Timeline/OTIO/SRT/来源包；上述人工流程的 `export --render` 必须先运行 `--scope program` 并人工批准。`movie auto` 则自动执行同样的节目技术检查并记录策略放行。节目级 Eval 按稳定渲染请求键取得不可变 CAS 候选，同一有效请求复用候选。人工批准和自动放行都绑定 scope、候选 ID/SHA-256、渲染请求键与输入指纹；导出只复制对应精确字节，并核对 `release.json`。

批准和正式导出都会从当前项目输入重新运行对应 scope 的确定性评估，不信任可编辑的 Eval JSON。`ProgramCandidate` schema v3 除 SHA-256 外还绑定随机 `artifact_integrity_lineage` 与单调 `artifact_integrity_epoch`：同一完整性历史中的损坏修复会推进 epoch；完整性记录被删除后，即使以相同 SHA-256 字节重建 CAS，也会生成新 lineage。两种情况都会使旧候选与旧审批 stale，旧 schema v2 candidate 直接 fail-closed。lineage + epoch 同时出现在节目输出、`sources.json` 和 `release.json` 的 candidate provenance 中。Manifest、Timeline、Plan、相关 Asset、Provider 预算账本、StyleProfile/ReferenceSet、候选或 FFmpeg 完整版本任一变化，都必须重新 Eval 和审批。结构与节目 scope 使用独立 latest 指针，避免一次结构检查覆盖可发布成片的节目级证据。

当前评估套件为 `structural-program-narrative-review-v5`。它会使旧套件批准 stale，并在发布前重新验证 v3 origin 所绑定的 Proposal 与 NarrativeReview；这仍是结构/来源一致性门禁，不是剧情语义或原创性评估。

`sources.json` 人工路径保持 schema v7，自动路径使用 schema v8 和 `automatic_narration_origin`。手工计划的 `narration_plan_origin` 为 `null`；电影草稿审批生成的计划通过该 origin 导出 NarrativeReview 不可变绑定。自动计划记录模型、脚本和源转写绑定；外部 TTS 与 Piper 均有独立语音来源，原片音轨另记 `source_audio_extraction_dependencies`。导出不会把模型路径、review 本地路径或凭据写入来源清单。

导出在原子 rename 前会回读 `release.json`，并末端复核包内 MP4、Eval 和 Approval 绑定。如果 rename 已完成、但随后父目录 `fsync` 失败，CLI 返回 `UNCERTAIN` 和退出码 `3`；完整导出目录可能已经存在。当前没有 `export reconcile` 命令，必须先按错误消息中的目标路径人工核对 `release.json`、文件哈希与候选，再决定是否重试，不能把该结果当成确定失败。

`narration synthesize` 默认只补缺失 Beat；`--replace` 才替换全部绑定，并保留计划中已有的 BGM。它先从指定 Python 运行时读取 `piper-tts` 包版本，再通过标准输入传文案；项目记录运行时版本及文案/模型/配置哈希，而不保存声音模型路径。短输出补静音到 Beat 时长，长输出直接阻断且不截词、不修改计划。`--render` 仍要求每个 Beat 都有连续、足时长的本地旁白片段；若存在 BGM，会先按 Timeline 拼接、应用增益/淡入淡出和可选 sidechain ducking，再对最终节目音频执行双遍 `-16 LUFS` 目标 loudnorm。节目级 Eval 要求实测响度为 `-16 ±1 LUFS`、True Peak 不高于 `-1.0 dBTP`；这仍不是广播认证。该实现不隐式运行 TTS、自动挑音乐或添加音效；字幕烧录由 `movie finish --burn-font` 显式选用。

## 可选 Provider 快速配置

凭据只从环境变量读取，不通过 CLI 参数写入项目：

| Provider | 当前凭据环境变量 | 模型/Endpoint |
|---|---|---|
| GPT Image 2 | `OPENAI_API_KEY` | 默认 `gpt-image-2`，可覆盖 |
| Seedream | `ARK_API_KEY` | 必须在项目中配置模型或 Endpoint ID |
| Seedance | `ARK_API_KEY` | 必须在项目中配置模型或 Endpoint ID |
| Kling | `KLING_ACCESS_KEY`、`KLING_SECRET_KEY` | 必须在项目中配置模型 |

例如：

```bash
uv run scenecall configure \
  --project ./demo \
  --budget-limit 20 \
  --budget-currency CNY \
  --json
uv run scenecall provider configure seedream \
  --project ./demo \
  --model YOUR_ENDPOINT_ID \
  --account-profile ark-prod-2026q3 \
  --unit-price 0.25 \
  --currency CNY \
  --json
uv run scenecall provider doctor --project ./demo --json
```

`account_profile` 是非敏感账号/凭据世代别名，不是 Key、邮箱或账号 ID。四家首方 Provider 的提交、对账、获取和取消都要求该字段；凭据或账号轮换时必须换一个新别名。SceneCall 使用项目本地 `0600` 随机 HMAC Key 绑定当前凭据字节，不保存凭据或裸哈希；即使环境变量名不变，值变化也会让旧任务在网络前因 binding mismatch 阻断。但该机制只能证明“凭据字节相同”，不能证明两个凭据是否属于同一厂商账号/租户，真实身份 API 与重新 smoke 仍是 live 发布门禁。

`provider doctor` 当前只检查本地 Endpoint、模型、`account_profile` 和凭据环境变量是否齐备，并固定返回 `"live_smoke_tested": false`；它不验证账号权限、区域可用性、真实返回格式或计费。提交付费请求前请阅读[Provider 与真实 smoke 门禁](docs/providers.md)。

参考图只能使用上述 `ingest` 返回的 Asset ID，不接受裸 URL：

```bash
uv run scenecall asset generate \
  --project ./demo \
  --provider seedream \
  --kind image \
  --prompt "保持授权参考图的色彩关系，生成非纪实过场" \
  --reference-asset-id GENERATION_REFERENCE_ASSET_ID \
  --aspect-ratio 9:16 \
  --json
```

当前最多传入 4 张参考图，每张只支持不超过 20 MiB 的 PNG/JPEG/WebP。SceneCall 会在预算预留和网络请求前重验 CAS、SHA-256、有效 `generation-reference`/`all` 权利和文件签名，再仅在内存中构造 data URI。Seedream/Seedance 适配器已声明参考输入能力，但厂商是否接受当前 data URI 载荷仍未经真实付费 smoke 验证。

## 本地制作台与动态图解

本地能力已补齐六类转场、逐字/弹性字幕、多画幅持久队列、31 个 HTML 模板、长文分段、HTML/React/3D/SVG/Blender/Manim、自动调色/LUT/HDR 与两遍响度。动效制作台迁入 216 个组件并支持自建 React 卡片、实时参数预览、主题/语言和独立导出；中文口播提供源绑定的候选审阅。服务器 TTS 和新模型/图库仍暂停或跳过。

见[本机制作指南](docs/local-production.md)和[逐项执行记录](docs/local-capability-completion-2026-10-15.md)。

### 可选原生 HyperFrames

新增 `visual hyperframes`，直接使用独立锁定的 HyperFrames 0.8.171，支持原生嵌套合成、视频入场/裁切、HTML 音轨混合、参数变体、并行渲染和官方时间线编辑器。输出经过帧数/画幅/帧率与完整解码检查，注册为 SceneCall 资产，可继续在已有 `edit` 时间线中剪辑；保留来源、缓存与正式片的分镜复核。

```bash
scenecall visual hyperframes doctor --json
scenecall visual hyperframes scaffold ./native-demo --json
scenecall visual hyperframes build ./native-demo/hyperframes.json --project ./demo --json
scenecall visual hyperframes preview ./native-demo/hyperframes.json --port 8835
```

首次使用执行 `scenecall visual hyperframes setup`。原生工程需要使用 HyperFrames 的 HTML/时间线约定，现有 HTML/Remotion 默认路径保持原行为。见[融合方案、使用与真实验收](docs/hyperframes-integration.md)。

## CLI 命令概览

| 范围 | 命令 |
|---|---|
| 项目 | `init`、`configure`、`ingest`、`status` |
| 一键自动解说 | `movie auto VIDEO --project OUTPUT --config CONFIG` |
| 独立写稿 | `movie write --project PROJECT --config CONFIG [--brief BRIEF]` |
| 电影草稿、改写与审阅 | `movie doctor|analyze|create|inspect`、`movie rewrite prepare|import|inspect|doctor|run-local`、`movie review prepare|import|inspect`、`movie approve` |
| 风格、脚本与节目音频 | `style learn|inspect|approve`、`plan`、`narration extract-source|synthesize` |
| 执行与恢复 | `run`、`stages list|retry|resolve` |
| 生成素材 | `provider list|configure|doctor`、`asset generate|review` |
| 远端任务 | `jobs list|reconcile|fetch|cancel` |
| 质量与导出 | `eval doctor|run|report|approve`、`improve`、`export` |
| 批处理 | `batch run|status|resume` |
| 原生 HTML 时间线 | `visual hyperframes setup|doctor|scaffold|check|direction-check|build|preview` |

`configure` 当前支持项目预算及其 ISO 4217 币种、语言、输出平台和最多改进轮数；`jobs fetch` 用于刷新支持该能力的异步任务并将成功输出安全物化到本地 CAS。SceneCall 不做汇率换算，Provider 估价币种与项目预算币种不一致时会阻止提交。首方单价必须是有限正数；零估价、缺失或非法币种都按 unknown cost 默认阻断，只有显式 `--allow-unknown-cost` 才会保留项目全部剩余预算，负数、NaN、Inf 则始终阻断。Provider 费用在所有任务状态（包括 `CANCELED`、`BLOCKED`、`UNCERTAIN`）下均保守计入；SQLite 中只要存在任一 Provider liability 记录，`configure --budget-currency` 就会在 Manifest 锁内直接 `BLOCKED`，不会写入新币种。Eval 中的 `provider_reserved_cost` 是潜在扣费上界，不是厂商实际账单回执。

## 文档

- [全自动剪辑、外部配音与字幕](docs/automatic-movie.md)
- [架构、项目目录与状态语义](docs/architecture.md)
- [工作流、证据脚本、风格、恢复与批处理](docs/workflow.md)
- [Provider 配置、任务生命周期与真实 smoke 门禁](docs/providers.md)
- [安全、权利与合成素材边界](docs/security-and-rights.md)
- [当前能力矩阵](docs/capability-matrix.md)
- [开发与验证](docs/development.md)

## 开发验证

```bash
uv run pytest
uv run ruff check .
uv run mypy src/scenecall
```

测试结果能证明本地确定性代码、NarrativeReview 完整性/CAS/SQLite/origin v3 合同、ASR 适配器/伪进程合同、剧情 `llama.cpp` 适配器/伪运行时合同、抽取式原片音轨来源合同、伪 Piper 执行器和 Provider Mock 契约。单样本真实 smoke 已分别跑通本地 ASR 与正式 `narration extract-source`；后者完成 37 个 Beat 的来源绑定、Program Eval 与 DRAFT 导出，Provider job 仍为 0。当前机器没有发现可用的 `llama-cli`/GGUF，因此不能把剧情运行器的契约测试写成真实本地 LLM smoke。Review 契约测试也不能替代真人身份验证、语义/版权/原创性判断或授权 corpus + Golden Set 校准。详见[真实本地模型 smoke](docs/real-local-model-smoke-2026-08-12.md)和[开发与验证](docs/development.md)。

## 明确不做

- 公开来源导入仅接受明确指定的单个 URL 和权利声明；不绕过登录、DRM 或版权控制。
- 不复刻作者姓名、固定台词、脸、声纹或未经授权的声音。
- 不自动发布内容，也不把离线评分描述为真实完播率或商业效果。
- 不把 ASR 或自动生成的叙事视为已经视觉核实或人工审阅的事实；模型只能依据输入对白组织解说，尚无独立语义判真能力。
- 不在远端任务结果未知时自动再次提交付费请求。
- 不把静音预览称为带声成片，不把剧情/Piper 适配器契约测试称为真实模型 smoke，不把本地双遍 loudnorm 或确定性节目 Eval 称为广播认证，也不把配置检查称为真实 Provider 接通。
- 不把软字幕内容/时序检查称为像素安全区验证，不把技术 Eval 称为剧情语义、生成伪影或平台效果评估。
- 不把不可变 NarrativeReview 称为身份认证、事实真值、版权/许可真实性或原创性证明；在授权 corpus 与 Golden Set 校准完成前，不宣称已有原创性 hard gate。

2026-10-16：科普分镜新增落地骨骼人物推箱、匀速斜面计算及真实高程三角网（相机、线框、表面路径与深度遮挡），并增加读条件—演动作—留观结果的逐步时间控制。见[人物与三维高程使用说明](docs/storyboard.md)和[35条第二轮研究及证据边界](docs/research/science-series-deep-20261016/README.md)。直接连续观看与听审仍未完成，未将ASR、逐帧差分或关键抽帧冒充完整听审。

2026-10-16 差异化制作：本地资料核对、理解路径和导演提纲、动态预演、实图与图解合成、多人持物交接、船闸水位机制、数据曲线与DEM导入、手机构图、动作声音、修订影响与系列反馈已接入。`scenecall science --help`列出工具；`storyboard build --draft`输出完整时长预演。见[使用说明](docs/science-direction.md)、[实施与验收清单](docs/differentiation-development-20261016.md)和`examples/lock-explainer-storyboard.json`。公开图片采集为显式可选联网步骤；本地渲染不依赖云模型、TTS或其他六个对照项目。
