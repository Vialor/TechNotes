> 2026/04/09

# 定义

## 结构定义

Skills是一个文件夹，其**目录结构**如下：

skill\-name/  
├── SKILL.md \# Required: 核心文件  
├── scripts/ \# Optional: 可执行代码  
├── references/ \# Optional: 文档，需要时载入  
└── assets/ \# Optional: templates, images, fonts

SKILL.md文件必须包含一种metadata叫做**frontmatter**，frontmatter = name + description

&emsp;&emsp;description: skill是干嘛的？什么时候使用？触发短语是什么？

SKILL.md文件的主体内容有两类：

&emsp;&emsp;**Reference content：**当前工作的所需知识，Skill甚至可以只提供知识，不提供SOP

&emsp;&emsp;**Task content：**具体任务的操作步骤

## 功能定义

一种上下文工程手段，帮助Agent以一种节约上下文的方式获取特定领域的**能力**。

Skill和工具的区别？：Skill和工具是正交关系，前者花费模型资源、更聚焦业务技能和场景编排，后者不花费主模型资源、提供来自决定论系统的原语操作。二者的接口是暴露给一个非决定论系统，并且应该尽可能采用拟人（ergonomic）的输出。

具体来说，Skills对于上下文来说是一个**三层加载结构**：

Level 1: 加载所有Skills的Metadata \(通常就是frontmatter，~100 tokens每Skill\)

Level 2: 根据 Metadata 或者用户直接指令（如Claude Code里是slash command）触发加载某SKILL.md \(不到500行和5k tokens\)

Level 3: 按需加载references和scripts \(无限制\)

**执行流程**：

    1. 上下文窗口包含核心系统提示词，以及所有已安装skill的metadata

    2. 模型根据用户输入触发特定skill：意思是模型理解使用skill的必要性，并通过bash tool读取对应skill的SKILL.md

    3. 模型选择进一步读取references里面的特定文件

    4. 模型拿到了足够完成任务的领域技能，继续工作

# 怎么写Skills

## Frontmatter

避免尖括号，容易被模型当作注入指令

name: 仅限小写字母，数字和短横线\-，至少包含一个动作，最多64 chars

description: 简洁到1024 chars以内，祈使句，关注用户意图而非具体实现，为了准确描述触发范围时宁可pushy（可以写"就算用户不显式提到某些相关领域的名词也要触发"）

    pushy的意思应该是多写点触发条件，而不是扩大触发范围。

allowed\-tools: Read, Grep, Glob 规范当此skill激活时只能使用的工具

## Instructions：常见patterns

### Gotchas

一个列表，包含特定环境的*违反常理的*事实

&emsp;&emsp;*违反常理的*：不要放泛泛的建议，要放那些不这么说就会犯错的点

&emsp;&emsp;调整Gotcha的位置，让模型在遇到提及情况前就读过这个列表

### **输出格式模板**

````markdown
## Report structure

Use this template, adapting sections as needed for the specific analysis:

```markdown
# [Analysis Title]

## Executive summary
[One-paragraph overview of key findings]

## Key findings
- Finding 1 with supporting data
- Finding 2 with supporting data

## Recommendations
1. Specific actionable recommendation
2. Specific actionable recommendation
```
````

### Checklists for multi\-step workflows

通过粘贴checklist到回复中，帮助模型很用户跟踪进度（这个最好是走线性流程，不要搞太复杂的逻辑）

````markdown
## PDF form filling workflow

Copy this checklist and check off items as you complete them:

```
Task Progress:
- [ ] Step 1: Analyze the form (run analyze_form.py)
- [ ] Step 2: Create field mapping (edit fields.json)
- [ ] Step 3: Validate mapping (run validate_fields.py)
- [ ] Step 4: Fill the form (run fill_form.py)
- [ ] Step 5: Verify output (run verify_output.py)
```

**Step 1: Analyze the form**

Run: `python scripts/analyze_form.py input.pdf`

This extracts form fields and their locations, saving to `fields.json`.

......
````

### Validation loops

```markdown
## Editing workflow

1. Make your edits
2. Run validation: `python scripts/validate.py output/` (a script, a reference checklist, or a self-check)
3. If validation fails:
   - Review the error message
   - Fix the issues
   - Run validation again
4. Only proceed when validation passes
```

### Plan\-validate\-execute 

对于批量操作和破坏性操作，先计划，再验证，最后执行

```markdown
## PDF form filling

1. Extract form fields: `python scripts/analyze_form.py input.pdf` → `form_fields.json`
   (lists every field name, type, and whether it's required)
2. Create `field_values.json` mapping each field name to its intended value
3. Validate: `python scripts/validate_fields.py form_fields.json field_values.json`
   (checks that every field name exists in the form, types are compatible, and
   required fields aren't missing)
4. If validation fails, revise `field_values.json` and re-validate
5. Fill the form: `python scripts/fill_form.py input.pdf field_values.json output.pdf`
```

## References文件

避免references深层嵌套，保持从SKILL.md开始就一层深。

对于嵌套references，Claude很可能 `head -100` ，忽略多余行数。所以对于超100行的reference文件，也建议把目录放在顶部，这样帮助Claude预览，然后选择要不要继续深入读取。

## Scripts文件

* 一定要分清执行代码和读代码的区别：

    * **Execute the script** \(most common\): "Run `analyze_form.py` to extract fields"

    * **Read it as reference** \(for complex logic\): "See `analyze_form.py` for the field extraction algorithm"

* 代码把报错情况分类好，报错原因用自然语言说清楚，不要把问题踢给模型去解决

* 文档应该说明配置参数的合理性以避免魔法数字 "voodoo constants" \(Ousterhout's law\)。要是你都不知道什么参数是正确的，Claude怎么知道？

让未来LLM调参是基于“语义”，不是数字。用户可能问：The requests sometimes timeout. Increase reliability.

Claude看到代码：TIMEOUT = 47。问题来了：47 是什么？seconds?milliseconds?有啥意义？Claude无法推理。

好例子：

```python
# HTTP requests typically complete within 30 seconds
# Longer timeout accounts for slow connections
REQUEST_TIMEOUT = 30

# Three retries balances reliability vs speed
# Most intermittent failures resolve by the second retry
MAX_RETRIES = 3
```

# 开发迭代Skill

## 测试Description触发

Should\-trigger queries 8~10条

&emsp;&emsp;考虑维度：语气（正式，随意，错别字），显式程度（点名skill，不点名skill），细节（有的短，有的上下文繁重），复杂度（单步任务和多步任务而skill只涉及其中一个步骤）

Should\-not\-trigger queries 8~10条

&emsp;&emsp;最有价值的是 near\-misses，涉及相同的关键词或概念，但要的skill其实不同

每个query跑3次，计算trigger rate。使用6\-4开的调试/验证合集。一般五次迭代以内都能搞定，除非queries搞太难了。

## 开发Skill前：Evaluation\-driven development

在写大量文档**之前**，先创建评估：

1. **Identify gaps:** 在没有 Skill 的情况下，让 Claude 跑一些有代表性的任务，记录具体失败点或缺失的上下文

2. **Create evaluations:** 构建** 3 个能检验这些缺口的场景**

3. **Establish baseline:** 衡量 Claude 在没有 Skill 时的表现

4. **Write minimal instructions:** 只写**刚好足以弥补这些缺口、并通过评估**的内容

5. **Iterate:** 执行评估，与基线比较，然后持续改进

**动机：**确保你真的在解决问题而不是虚空打靶

## 开发Skill：用Claude本身迭代

### 基本策略

1. 用 Claude A 打磨和完善 Skill

2. 用 Claude B 实际使用 Skill 执行真实工作，并把结果带回给 Claude A

**为什么有效**：Claude知道怎么写有效的agent指令，以及agent需要什么信息。Claude模型本身也知道Skill的格式和结构，甚至无需额外提示词。

### 创建和迭代Skill

1. 不用Skill先完成任务，过程中你自然会提供提示词等

2. 发现可复用的模式

3. 让Claude A创建Skill

4. 审查简洁性：Claude A 有没有加入不必要的解释？

5. 优化信息架构：让 Claude A 更有效地组织内容。比如：“把表结构单独整理到一个参考文件里。以后我们可能还会增加更多表。”

6. 在相似任务上测试：用 Claude B（一个全新的、已加载该 Skill 的实例）在相关用例上测试这个 Skill。观察 Claude B 是否能找到正确信息、正确应用规则，并成功完成任务。

7. 基于观察迭代：如果 Claude B 表现吃力或漏掉了什么，就带着具体问题回到 Claude A。比如：“Claude 使用这个 Skill 时忘了按日期筛选 Q4。我们是不是该加一节，专门讲日期筛选模式？”

## 创建可验证的中间输出

旧：analyze → execute → verify

新：analyze → create plan file\(changes.json\) → validate plan → execute → verify

**解释：**对于复杂开放任务，采取plan\-validate\-execute策略，先计划，再验证，最后执行。

**动机：**

1. **尽早发现错误**：验证能在变更真正落地之前发现问题

2. **可由机器验证**：脚本能提供客观的验证结果

3. **可逆的规划过程**：Claude 可以反复迭代方案，而不碰原始内容

4. **调试更清晰**：错误信息会直接指向具体问题

# 参考文章

[claude.com](https://claude.com/blog/equipping-agents-for-the-real-world-with-agent-skills)

[bibek\-poudel.medium.com](https://bibek-poudel.medium.com/the-skill-md-pattern-how-to-write-ai-agent-skills-that-actually-work-72a3169dd7ee)

[platform.claude.com](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)

[agentskills.io](https://agentskills.io/skill-creation/best-practices)

[code.claude.com](https://code.claude.com/docs/en/skills)
