# CT 软件开发流程规范

将需求确认、界面设计、开发、验证、本地部署和交付流程封装为 Codex Skill。[查看完整规范](skills/ct-dev-workflow/SKILL.md)

## 安装

在 Codex 对话中发送以下指令。

```text
$skill-installer install https://github.com/TristenChenTao/ct-dev-workflow/tree/master/skills/ct-dev-workflow
```

## 使用

在新的 Codex 对话中调用 `$ct-dev-workflow`，并描述要处理的开发任务。项目长期采用时，在 `AGENTS.md` 中声明本项目使用 `ct-dev-workflow`，并记录 Git 远端、本地启动方式与地址、验收命令与 UI 尺寸、发布和回退入口。

规范按任务类型要求使用 `ponytail`、`frontend-design`、`ui-ux-pro-max` 和 `web-design-guidelines`。安装入口见 [Skill 使用](skills/ct-dev-workflow/SKILL.md#skill-使用)。
