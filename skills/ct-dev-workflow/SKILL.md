---
name: ct-dev-workflow
description: 执行采用 CT 软件开发流程规范的项目开发任务，包括需求确认、UI 设计、实现、验证、本地部署和交付。项目在 AGENTS.md 中采用本规范，或用户明确要求按本规范工作时使用。
---

# CT 软件开发流程规范

各项目在 `AGENTS.md` 中引用本 Skill，并填写 Git 远端、本地启动方式与地址、验收命令与 UI 尺寸、发布和回退入口。以用户最新明确要求为准，开始前核对实际源码与环境。

## Skill 使用

开工前确认本次任务必用的 Skill 已安装且可用。**缺少必用 Skill 时，暂停任务，告知用户缺少的 Skill 名称，请用户先安装。确认可用后再继续。**

| Skill | Git 仓库 | 安装方式 |
| --- | --- | --- |
| `ponytail` | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) | 从 Git 安装 [skills/ponytail](https://github.com/DietrichGebert/ponytail/tree/main/skills/ponytail) |
| `frontend-design` | [anthropics/skills](https://github.com/anthropics/skills) | 从 Git 安装 [skills/frontend-design](https://github.com/anthropics/skills/tree/main/skills/frontend-design) |
| `ui-ux-pro-max` | [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | 使用 [上游 CLI](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill#using-cli-recommended) 安装（含数据和脚本） · [Skill 目录](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill/tree/main/.claude/skills/ui-ux-pro-max) |
| `web-design-guidelines` | [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) | 从 Git 安装 [skills/web-design-guidelines](https://github.com/vercel-labs/agent-skills/tree/main/skills/web-design-guidelines) |

## 需求确认

**所有需求先在对话中用简短文字说明预期结果，经用户明确确认后再开发。** 涉及 UI 时，还须确认当前版本的效果图。

### 涉及 UI 的需求

- **所有 UI 组件必须沿用项目内同类组件的样式，优先直接复用现有组件。** 首次新增的组件类型也须遵循项目现有设计规范。
- **设计全新页面时，必须先使用 Skill `frontend-design`。** 根据页面的使用者、业务任务和项目设计规范，明确内容层级、页面布局与视觉方向，再细化方案。
- 所有 UI 改动（含全新页面、文案和微调）均须用 Skill `ui-ux-pro-max` 细化方案，出图前按 Skill `web-design-guidelines` 的规则检查。效果图覆盖受影响界面和关键交互状态。**用户明确确认当前效果图后才开发**，改稿后重新检查、出图并确认。
- 实现后用 Skill `web-design-guidelines` 获取最新规则并审查改动文件。按确认稿及项目规定的尺寸核对实际渲染、可访问性、长文本、空值和错误状态。修复本次引入的问题，记录检查范围、规则获取情况和待补项。
- 审批列表、待办、通知直达详情、处理页及关联弹窗默认适配并验收桌面和手机。
- 业务确认和删除须弹窗二次确认，含表单内尚未保存的删除。录入或详情弹窗之后仍须最终确认。确认弹窗保留返回修改或取消及右上角关闭。取消、关闭或按 Esc 保留原数据和输入。写入前复核权限与状态，防止重复提交。纯提示弹窗只保留右上角关闭和「知道了」等确认动作。

## 开发

- **所有编码工作必须使用 Skill `ponytail`**，优先复用现有实现，以最小改动完整交付。
- 开工核对范围、授权、分支、工作区和依赖。按相关源码、调用者和项目文档定位，只扩大到受影响的范围。
- 独立任务用独立工作树，连续任务复用原工作树。建工作树前核对基线和未提交依赖，按需带入依赖。保留并行改动，只整合和暂存本次差异，避免整体清理、重置或覆盖现有修改。
- 新增模块、调整目录、共享接口或依赖时，按项目架构约定审查。日常修改沿用现有边界，避免夹带重构、全局格式化或依赖升级。

## 验证与本地部署

**任何代码改动默认都要部署到日常本地环境，无需用户另行要求。**

1. **先测试。** 在隔离环境按影响范围验证。涉及权限、数据写入或跨模块流程时，覆盖正常和失败情况。出现新改动、失败或未解决疑点时，再扩大或重复检查。
2. **再部署并验证。** 按项目方式让日常本地环境实际加载本次改动，能热加载时优先热加载。通过实际入口确认功能生效后再交付。

纯文档任务只检查内容、引用和格式。

## 操作与凭据

- 写真实数据、改数据库表结构、导入真实文件、修改数据库权限、提交、推送和上线分别按当前任务授权执行。已有明确授权直接沿用，历史一次性授权只适用于当时的任务。
- 优先复用项目授权的私密配置与最小权限账号。按项目指定位置排查凭据，显式检查可能被 Git 忽略的配置文件。仅在所需配置确实缺失、认证被明确拒绝或需要轮换时向用户确认。
- 凭据只通过批准的私密渠道获取，存放在受限私密配置或进程环境中。检查与交付仅记录配置路径、字段名和是否已设置。凭据严禁进入源码、普通文档、日志、终端输出、前端和 Git。私密文件、原始附件、日志和任务产物遵守 Git 忽略规则。
- 数据查询采用字段白名单、参数绑定和后端授权。服务端按权限决定可返回的敏感字段。

## 提交、推送与上线

- **提交**按授权，仅提交本次差异，检查暂存内容和验证结果。提交标题、正文及合并、回退提交说明使用简体中文。
- **推送**按授权，未指定分支时默认推送到远端 `master`。**所有推送说明必须使用中文。** 整合最新远端改动并复验，沿用现有远端和连接方式，遵守分支保护与钩子，非强制推送，完成后核对远端提交。
- **上线**须单独授权。推送或合并会自动触发上线时，须在执行前核对上线授权。未指定发布来源时，必须从远端 `master`（如 `origin/master`）选取已核验的提交。锁定提交 SHA，按项目流程准备回退、构建和部署，核验线上版本、功能和地址。

## 交付与文档维护

交付时分别报告改动、验证、提交、推送、本地部署、线上部署和未验证项，附已核验的地址或运行入口。长期规则在对应文档维护，更新受影响的项目说明。逐次验证、发布和交接记录写对应专题，历史运行状态和授权仅供追溯。
