# Skill 封装到 GitHub 上传 SOP v1

更新时间：2026-04-18 02:10 GMT+8

## 适用场景
当需要把一个 OpenClaw skill 从本地整理成可公开 GitHub 仓库时，默认按这套流程执行。

## 标准流程

### Phase 1：整理 skill repo
1. 确认 repo root 最小结构：
   - `SKILL.md`
   - `references/`
   - `assets/`
   - `_meta.json`（可选）
   - `skill-repo.json`
2. 补齐公开仓库基础文件：
   - `README.md`
   - `LICENSE`
   - `.gitignore`
3. README 首屏必须可读，避免乱码与内部路径泄露。
4. 额外补一份 `POST.md`，预留仓库发布贴文案。

### Phase 2：初始化独立 git 仓库
1. 在 skill repo 根目录执行：
   - `git init`
   - `git add .`
2. 若本地未配置 git identity，则仅在 repo 内配置：
   - `git config user.name "OpenClaw Bot"`
   - `git config user.email "openclaw-bot@local.invalid"`
3. 首次 commit：
   - `Initial public skill repo for <repo-name>`

### Phase 3：打通 GitHub 推送链路
1. 优先检查是否已有 GitHub CLI / 浏览器 / SSH 能力。
2. 若 `github.com:443` 不通但 `ssh.github.com:443` 可通，则优先走 **SSH over 443**。
3. 生成 SSH key（如无）：
   - `ssh-keygen -t ed25519 -C "openclaw-bot@local.invalid"`
4. 让用户只做一次账号侧动作：
   - 把公钥加到 GitHub SSH keys
5. 配置 `~/.ssh/config`：
   - `Host github.com`
   - `HostName ssh.github.com`
   - `Port 443`
   - `User git`
   - `IdentityFile ~/.ssh/id_ed25519`
   - `IdentitiesOnly yes`
6. 验证：
   - `ssh -T git@github.com`

### Phase 4：远程仓库策略
1. 优先使用主名仓库；如果用户临时建了备选名，最终统一到主名仓库。
2. remote 统一为：
   - `git@github.com:<owner>/<repo>.git`
   - 若 DNS 不稳，可改成：
   - `ssh://git@ssh.github.com:443/<owner>/<repo>.git`
3. 首次推送：
   - `git push -u origin main`

### Phase 5：上线后补展示面
1. 继续优化 README 首屏
2. 补 `POST.md`
3. 必要时追加：
   - GitHub description
   - profile 引流文案
   - pinned repo 建议
4. 新增优化后再次 commit + push

## 这次已验证有效的关键经验
- 不要把 skill 发布目录直接挂在上层工作区 git 下；要初始化成独立 repo。
- 如果 `gh auth login` 因 `github.com` 超时失败，不要卡死在网页登录，直接切 SSH over 443。
- `ssh.github.com:443` 可作为 GitHub 受限网络下的稳定兜底链路。
- README 首屏和 `POST.md` 要一起做，不然仓库虽上线但展示弱。
- 用户只需要参与 GitHub 身份动作；其余整理、初始化、推送、优化默认由我自己完成。
