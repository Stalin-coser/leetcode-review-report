# LeetCode Review Report Skill

[中文](#中文) | [English](#english)

---

## 中文

`leetcode-review-report` 是一个面向 Codex 的 LeetCode 解法审查 skill。它会先根据题号核对题目、约束和示例，再审查用户提交的完整代码；只有在代码被确认正确后，才会自动生成中文 Markdown 结题报告。

### 主要特性

- **先审查，后报告**：代码存在错误、信息不完整或关键约束无法确认时，不生成结题报告。
- **支持连续复审**：用户提交修正版后，继续检查新版本，直到能够确认正确性。
- **保留用户实现**：报告以用户最终确认正确的代码和核心思路为主体，不擅自替换成另一种解法。
- **验证边界清晰**：区分静态分析、编译、运行、示例测试、暴力对拍和平台提交结果。
- **报告目录可配置**：支持本次请求指定、项目配置文件、环境变量和默认目录。

### 工作流程

```text
题号 + 用户代码
       ↓
核对题目、示例与约束
       ↓
审查逻辑、边界与复杂度
       ↓
代码有误 ──→ 说明原因与修改建议 ──→ 等待修正版并复审
       ↓ 正确
生成中文 Markdown 结题报告
       ↓
校验报告并返回绝对路径
```

### 安装

将本仓库克隆或复制到 Codex 的 skills 目录：

```text
$CODEX_HOME/skills/leetcode-review-report
```

如果没有设置 `CODEX_HOME`，通常可以使用：

```text
~/.codex/skills/leetcode-review-report
```

安装后，可在新任务中显式调用 `$leetcode-review-report`，也可以让 Codex 根据描述自动选择它。

### 使用示例

```text
使用 $leetcode-review-report 审查 LeetCode 2779。下面是我的 Java 代码：

class Solution {
    // ...
}
```

如果代码尚未正确，skill 会指出具体问题并等待修正版，不会提前创建报告。

### 配置报告目录

skill 按以下优先级选择输出目录：

1. 用户在当前请求中明确指定的目录；
2. 项目根目录 `.codex/leetcode-review.json`；
3. 环境变量 `LEETCODE_REPORT_DIR`；
4. 项目根目录下的 `reports/`。

项目配置示例：

```json
{
  "reportDirectory": "reports/leetcode"
}
```

相对路径按项目根目录解析，绝对路径保持不变。若选定目录无法创建或写入，skill 会停止并说明原因，不会静默改写到其他位置。

### 报告内容

默认文件名：

```text
题号-中文题名-YYYY-MM-DD.md
```

报告包含题目信息、题目概述、解题思路、算法步骤、正确性分析、复杂度、用户代码解析、示例与边界情况、验证记录、易错点和可选优化。

### 项目结构

```text
leetcode-review-report/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
└── references/
    └── report-spec.md
```

### 校验

可使用 Codex 内置的 skill 校验器检查结构和 frontmatter：

```text
python <skill-creator-path>/scripts/quick_validate.py <path-to>/leetcode-review-report
```

---

## English

`leetcode-review-report` is a Codex skill for reviewing user-submitted LeetCode solutions. It verifies the problem, constraints, and examples before reviewing the complete solution, and it generates a Chinese Markdown completion report only after the code has been confirmed correct.

### Key features

- **Review before reporting**: no report is generated while the code is incorrect, incomplete, or missing constraints required for a correctness decision.
- **Iterative re-review**: corrected submissions are reviewed again until correctness can be established.
- **Preserves the user's solution**: the final report is based on the user's verified code and original approach instead of silently replacing it.
- **Clear evidence boundaries**: static analysis, compilation, execution, example tests, differential testing, and online-judge results are reported separately.
- **Configurable output directory**: supports a per-request destination, project configuration, an environment variable, and a project default.

### Workflow

```text
Problem number + user code
             ↓
Verify statement, examples, and constraints
             ↓
Review logic, edge cases, and complexity
             ↓
Incorrect ──→ Explain the cause and suggest a focused fix
             │                     ↓
             └──────── Review the corrected submission
             ↓ Correct
Generate the Chinese Markdown completion report
             ↓
Verify the report and return its absolute path
```

### Installation

Clone or copy this repository into the Codex skills directory:

```text
$CODEX_HOME/skills/leetcode-review-report
```

If `CODEX_HOME` is not set, the usual fallback is:

```text
~/.codex/skills/leetcode-review-report
```

After installation, invoke `$leetcode-review-report` explicitly in a new task or let Codex select it automatically from its description.

### Usage example

```text
Use $leetcode-review-report to review LeetCode 2779. Here is my Java solution:

class Solution {
    // ...
}
```

If the solution is not yet correct, the skill explains the concrete issue and waits for a corrected version instead of creating a premature report.

### Configure the report directory

The output directory is resolved in this order:

1. A directory explicitly provided in the current request;
2. `.codex/leetcode-review.json` in the project root;
3. The `LEETCODE_REPORT_DIR` environment variable;
4. `reports/` under the project root.

Project configuration example:

```json
{
  "reportDirectory": "reports/leetcode"
}
```

Relative paths are resolved from the project root, while absolute paths are preserved. If the selected directory cannot be created or written, the skill stops and reports the error instead of silently falling back elsewhere.

### Report contents

Default filename:

```text
<problem-number>-<Chinese-title>-YYYY-MM-DD.md
```

The report covers problem metadata, an overview, the solution idea, algorithm steps, correctness, complexity, a walkthrough of the user's code, examples and edge cases, validation evidence, common pitfalls, and optional improvements.

### Repository structure

```text
leetcode-review-report/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
└── references/
    └── report-spec.md
```

### Validation

Use the validator bundled with Codex's `skill-creator` skill to check the folder structure and frontmatter:

```text
python <skill-creator-path>/scripts/quick_validate.py <path-to>/leetcode-review-report
```
