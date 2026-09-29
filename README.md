# 荷塘雨课堂 Agent Skill

让能操作浏览器的 agent 在你已登录的 Chrome 中持续查看指定荷塘雨课堂，发现实时习题或投票后，理解题意、简短作答，并检查是否提交成功。

**检测负责发现题目，agent 负责回答。** 不需要另配模型 API Key，也不要求安装自动答题用户脚本。适用于课程允许的 AI 辅助场景，请遵守所在课程的使用规则。

## 使用前提

- 一个支持加载 `SKILL.md`、操作浏览器并持续运行的 agent。本项目在 Codex 桌面端的 `cua_repl` 浏览器工具上实践过。
- Chrome 已登录你的荷塘雨课堂，浏览器连接可用。
- 在任务中明确课程和操作范围：只提醒、填好等你确认，或直接提交。

这是 **agent 操作指南**，不是浏览器扩展，也不是可以独立运行的后台程序。其他 agent 可以参考技能文件，但需要适配自己的工具；不能假定所有平台都能直接运行。

## 安装到 Codex

将这个仓库的 `skills/hetang-classroom-agent` 文件夹放入你的 Codex skills 目录。以下命令在目标尚不存在时安装，不覆盖已有版本：

```bash
git clone https://github.com/liuxin2560/hetang-classroom-agent.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
test ! -e "${CODEX_HOME:-$HOME/.codex}/skills/hetang-classroom-agent" && \
  cp -R hetang-classroom-agent/skills/hetang-classroom-agent \
    "${CODEX_HOME:-$HOME/.codex}/skills/hetang-classroom-agent"
```

如果已安装，请让 agent 检查差异后更新。安装后可在新会话中尝试调用 `$hetang-classroom-agent`；未被识别时，直接让 agent 阅读该文件夹中的 `SKILL.md`。

## 示例指令

直接作答：

> 使用 $hetang-classroom-agent。进入我 Chrome 里的荷塘雨课堂“目标课程”，持续盯着本次课堂，出现习题或投票时由你回答并提交。填空只写关键词，简答一两句，按题目语言回答。不要创建定时任务，每 10 秒检查页面，下课或我说停就结束。

只提醒：

> 使用 $hetang-classroom-agent 盯着“目标课程”，有新题时通知我，暂时不要填写或提交。

填好后确认：

> 使用 $hetang-classroom-agent 查看“目标课程”的新题，帮我填入简短答案，每题等我确认后再提交。

## 方法与经验

1. 复用用户已登录的浏览器，核对课堂标题和课堂标识。
2. 在当前会话中以短间隔检查页面，不把“持续盯课”偷换成分钟级定时唤醒。
3. 综合原生作答入口、倒计时、表单和可见习题提醒检测新题；提醒可能截断题干，必要时截图。
4. 由 agent 理解完整题目后回答，不能固定选项兜底。
5. 确认原生“已完成”及提交内容。提交中不算成功，未打分也不代表答案正确。
6. 跟回正在放映的内容，直到课堂明确结束或用户叫停。

曾遇到的具体问题及操作示例见 [浏览器操作经验](skills/hetang-classroom-agent/references/cua-workflow.md)。完整工作流在 [SKILL.md](skills/hetang-classroom-agent/SKILL.md)。

## 验证范围

2026-09-29，在一堂真实荷塘课堂中，agent 经 Chrome 原生页面完成了三道限时英文主观题，均确认“已完成”。期间使用可见习题提醒辅助检测，未由原用户脚本生成或提交答案。

这个结果验证了该次主观题操作流程，不代表所有题型、学校站点和工具版本都兼容。十秒检测仍有工具和推理延迟，无法保证不会错过极短时限题。请保持 agent、Chrome 及其连接运行；会话结束后本 skill 本身不会在后台继续工作。

公开仓库不包含课堂截图、真实题目回答记录、学生信息、Cookie 或 API Key。

## 来源与许可

思路参考 [ZaytsevZY/yuketang-helper-auto](https://github.com/ZaytsevZY/yuketang-helper-auto) 的课堂提醒功能，并整理了实际浏览器操作中的经验。本项目独立编写技能说明，不包含该项目源码，也不需要它作为运行依赖。

本项目与清华大学、雨课堂及 OpenAI 无官方关联。原创内容采用 [MIT License](LICENSE)。
