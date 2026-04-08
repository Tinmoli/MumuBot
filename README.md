# MumuBot

基于 OneBot 11 的 QQ 机器人框架，支持插件化扩展。

当前版本：`2.0.1`

## 环境要求

- Python 3.10+（建议）
- 已运行并可连接的 OneBot 11 实现（WebSocket）

## 快速启动

### Windows

双击 `start.bat`

### Linux

```bash
chmod +x start.sh
./start.sh
```

## 主配置（`config.toml`）

核心配置在根目录 `config.toml` 的 `[OneBot11]` 段：

```toml
[OneBot11]
WebSocketUrl = "ws://127.0.0.1:3001/"
AccessToken = ""
CommandPrefix = "-"

# 群白名单：不为空时，仅处理这些群的消息/通知
AllowedGroups = []

# 私聊管理员：只允许这些 QQ 私聊机器人
AdminUsers = []
```

说明：

- 群聊：正常处理（可选白名单）。
- 私聊：**仅 `AdminUsers` 中的用户**会被处理，其余私聊静默忽略。

## 指令与帮助

发送 `-help` 查看指令菜单（前缀以 `CommandPrefix` 为准）。

管理员指令菜单会自动聚合：

- 内置管理员指令
- 插件声明的管理员命令（插件通过 `get_admin_commands()` 声明）

## 插件系统

插件目录为：`plugin/`。

每个插件都是一个目录，至少包含：

- `config.toml`（启用开关/插件配置）
- `__init__.py`（导出插件类）
- `<plugin_name>_plugin.py`（插件实现）

插件启用/禁用：编辑插件目录下的 `config.toml`：

```toml
[plugin]
enabled = true
```

插件依赖：若插件目录包含 `requirements.txt`，启动时会自动安装。

插件可用能力：

- 命令处理：`handle_command(...)`
- 消息处理：`on_message(ctx)`
- 通知处理：`handle_notice(...)`
- OneBot 11 API：优先通过 `self.context.onebot11_service` 调用

## 项目结构

```text
MumuBot/
├── config.toml
├── requirements.txt
├── plugin/
│   ├── <plugin_name>/
│   │   ├── config.toml
│   │   ├── __init__.py
│   │   └── <plugin_name>_plugin.py
└── src/
    ├── core/
    ├── models/
    ├── services/
    └── main.py
```

## 免责声明

本项目仅供学习交流使用，请勿用于违法用途；使用本项目产生的一切后果由使用者自行承担。
