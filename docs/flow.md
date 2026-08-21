# 发布流程图（make-knowledge-cards）

同一套流程的三种视图：流程图、泳道图、时间线。在 GitHub 上本文件会被自动渲染为图。

## 1. 流程图（Flowchart）

```mermaid
flowchart TD
    A[阶段一 · Skill 开发<br/>init_skill.py / SKILL.md<br/>openai.yaml / 校验 / 测试]
    B[阶段二 · 开源前安全自查<br/>扫描敏感信息<br/>.gitignore 排除 .workbuddy]
    C[阶段三 · Git 初始化<br/>git init -b main<br/>commit 62eeca8]
    D[阶段四 · GitHub CLI 安装登录<br/>下载 gh 解压 + PATH<br/>gh auth login → mintao121]
    E[阶段五 · 发布到 GitHub<br/>gh repo create --public<br/>push（gh auth setup-git 修复）]
    A --> B --> C --> D --> E
```

## 2. 泳道图（Swimlane）

按角色分三栏：我（agent 执行）、gh（GitHub CLI 自动动作）、你（浏览器里手动完成）。

```mermaid
flowchart TD
    subgraph ME[我做的 / agent]
        S1[Skill 开发：骨架·校验·测试]
        S2[安全自查 + .gitignore]
        S3[Git 初始化：commit 62eeca8]
        S4[安装 gh：解压 + PATH + setup-git]
        S5[gh repo create + push 修复]
    end
    subgraph GH[gh 做的 / GitHub CLI]
        G4[auth login 设备码流程]
        G5[推送传输到 GitHub]
    end
    subgraph YOU[你在浏览器做的]
        U4[完成设备码授权]
        U5[填 About：描述·Topics]
    end
    S1 --> S2 --> S3 --> S4 --> S5
    S4 --> G4 --> U4
    S5 --> G5
    S5 -.-> U5
```

## 3. 时间线（Timeline）

```mermaid
timeline
    title make-knowledge-cards 发布时间线
    阶段一 : Skill 开发（本地）
    阶段二 : 开源前安全自查
    阶段三 : Git 初始化 commit 62eeca8
    阶段四 : gh 安装 + 浏览器授权
    阶段五 : 发布到 GitHub（公开）
```

## 关键坑

`gh auth login` 之后必须再跑一次 `gh auth setup-git`，把 GitHub 的凭据助手指向
`gh.exe auth git-credential`，否则 `git push` 会卡在凭据选择器（`credential.helper helper-selector`）。
