---
id: meeting-minutes-whisper-local
name: 会议纪要 (Meeting Minutes)
version: 2.2.0
description: 将会议录音或文本转化为结构化纪要，提取核心共识、对标问题、修改方案与后续行动，支持证据定位与质量核验。
author: Local
---
# 会议纪要 (meeting-minutes)

用于将“录音/文本”快速转化为符合政务/业务标准的结构化会议纪要。本技能采用“分层编排 + 证据约束”设计。

## 触发方式
- **指令触发**：`/meeting-minutes` 或 `/会议复盘` (可附带文件路径)
- **隐式触发**：直接上传录音、文档或粘贴会议文本。

## 能力目录 (Commands)
| 指令 | 职责说明 | 对应文件 |
| :--- | :--- | :--- |
| `/meeting-minutes` | **流程编排器**：解析输入 -> 提炼清洗 -> 汇总出稿 | [commands/meeting-minutes.md](commands/meeting-minutes.md) |
| `/纪要核验` | **质量闸门**：对生成的纪要进行事实核对与完整性检查 | [commands/minutes-verify.md](commands/minutes-verify.md) |

## 规则知识库 (Rules)
规则独立于执行流程，确保修改标准不影响主程序稳定性：
- `rules/minutes-rules.md`：纪要提炼的底线约束、证据契约与行文规范。
- `rules/minutes-template.md`：标准输出大纲与排版约束。

## 路由策略 (强制)
- **音频** (`.wav/.m4a/.mp3/.flac/.aac`)：调用本地 Whisper 转写后分析。
- **文档**：
  - `.docx` -> 调用 `docx` 技能。
  - `.pdf` -> 调用 `pdf` 技能。
  - `.txt/.md/.srt/.vtt` -> 直接读取。
- **环境限制**：精准匹配路由，禁止探测环境（如测试 pandoc/libreoffice），禁止输出多余的调试过程。

## 最终交付约定
- **文件格式**：标准 Markdown (`.md`) 文件。
- **命名规范**：`<原文件名>-会议纪要.md` (若无来源名则用 `YYYYMMDD-HHmm-会议纪要.md`)。
- **文件操作**：默认禁止执行 `del/rm` 清理临时文件，除非用户明确要求。