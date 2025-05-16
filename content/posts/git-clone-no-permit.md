---
tags:
  - git
summary: 通过 SSH Agent 为 git clone 命令提供认证，出现无权限问题
title: git clone no permit
share: "true"
date: 2025-05-17T00:10:42+08:00
---

在 windows 中，使用 ssh-agent 为 git clone 提供认证，出现以下问题

1. `ssh -T git@github.com` is ok
2. `git clone git@github.com:xxx/xx.git` 无效，提示无权限

这可能是 git 使用的 ssh 客户端有问题，更换为 Windows 内置的客户端试试

---

If you encounter the following error on Windows: `Permission denied (publickey). fatal: Could not read from remote repository.` But at the same time, the command `ssh -vT git@github.com` gives you this message: `Hi USER_NAME! You've successfully authenticated, but GitHub does not provide shell access.`

- Add to your environment variables: `GIT_SSH_COMMAND="C:/Windows/System32/OpenSSH/ssh.exe"`. Ensure there's no space after the equal sign. Also, verify that the path is correct for your OS.
- Alternatively, change the Git configuration by running: `git config --global core.sshCommand C:/Windows/System32/OpenSSH/ssh.exe`
- Don't forget to start the SSH agent service: `Get-Service -Name ssh-agent | Set-Service -StartupType Manual Start-Service ssh-agent` You can set the StartupType to Automatic if you want it to run when Windows starts.

A restart might be necessary, but in many cases, simply re-opening the terminal is sufficient.

_Note:_ Git for Windows includes an embedded SSH client, which it might use by default. Switching to the Windows System32 SSH client can resolve certain authentication issues.