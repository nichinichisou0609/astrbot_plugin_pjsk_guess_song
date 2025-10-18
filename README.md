# PJSK猜歌插件使用说明

## 1. 插件功能简介

本插件是使用《プロジェクトセカイ カラフルステージ！ feat. 初音ミク》/《世界计划 多彩舞台》/《初音未来 缤纷舞台》a.k.a. PJSK经过少量修改的音频资源制作的听歌猜曲游戏。除了普通的猜歌模式，还加入了倒放、变速、纯伴奏、纯贝斯、纯鼓组、纯人声、钢琴重现等多种效果，并包含了实验性的“猜歌手”功能。

插件内置了积分排行榜、每日游戏次数限制、游戏冷却等功能，并提供一个**轻量模式**选项以优化在核心数较少服务器上的表现。此外，可通过API与远程服务器同步玩家分数和游戏数据。

**注意**：本插件所有必需的音频和数据资源均托管在我的服务器，并已经默认配置，不需要下载（日后可能会上传资源下载供本地使用），资源截止至日服v6.0.2
>猜歌群：883195991
## 2. 环境依赖

本插件的核心音频处理功能（如裁剪、变速、倒放等）依赖于`ffmpeg`。请确保您的系统环境已正确安装 `ffmpeg`。

- **对于 Docker 用户**：官方提供的 `AstrBot` Docker 镜像**不包含或无法使用** `ffmpeg`。您需要手动进入正在运行的容器并执行以下指令进行安装：
  ```bash
  # 进入容器
  docker exec -it <你的容器名或ID> /bin/bash

  # 在容器内安装 ffmpeg (以Debian/Ubuntu为例)
  apt-get update && apt-get install -y ffmpeg
  ```

- **对于其他环境**：请根据您的操作系统（如 CentOS, Windows 等）使用对应的包管理器或从官网下载安装 `ffmpeg`。

## 3. 指令列表

### 游戏指令
- `猜歌` / `gs`: 开始一轮普通模式的猜歌游戏 (1分)。
  - *注: `猜歌` 和 `gs` 后可直接跟数字选择模式，例如 `猜歌1` 和 `猜歌 1` 效果相同。*
- `猜歌 1`: **2倍速**模式 (1分) *(轻量模式下会自动切换为普通模式)*。
- `猜歌 2`: **倒放**模式 (3分) *(轻量模式下会自动切换为普通模式)*。
- `猜歌 3`: <strong>AI-Assisted Twin Piano ver.</strong>模式 (2分)。
- `猜歌 4`: **纯伴奏**模式 (1分)。
- `猜歌 5`: **纯贝斯**模式 (3分)。
- `猜歌 6`: **纯鼓组**模式 (4分)。
- `猜歌 7`: **纯人声**模式 (1分)。
- `随机猜歌` / `rgs`: **核心玩法**。随机组合多种效果，分数越高的组合出现概率越低。
- `猜歌手`: 竞猜歌曲的演唱者 (测试功能, 不计分)。

### 点歌与试听
- `听<模式> [歌曲名/ID]`: 随机或指定播放一首歌曲的特殊音轨。
  - **可用模式**: `钢琴`, `伴奏`, `人声`, `贝斯`, `鼓组`。
  - **注意**: 按歌曲名搜索时，需要提供完整的官方名称，暂不支持别名或模糊匹配。若查找困难，建议先使用其他查歌机器人获取准确的歌曲名或ID。
  
  - **示例**:
    - `听钢琴`: 随机播放一首钢琴曲。
    - `听贝斯 Tell Your World`: 播放指定歌曲的贝斯音轨。
    - `听鼓组 3`: 播放ID为3的歌曲的鼓组音轨。
>注：所有音轨均为使用Demucs分离
- `听anvo [歌名/ID] [角色名缩写]` : 播放指定或随机的Another Vocal。可指定角色后随机
  - **注意**: 按歌曲名搜索时，需要提供完整的官方名称，暂不支持别名或模糊匹配。若查找困难，建议先使用其他查歌机器人获取准确的歌曲名或ID。
  - **示例**:
    - `anvo` : 随机播放一首Another Vocal
    - `anvo 280 toya` : 播放指定Another Vocal
    - `anvo 280` : 查看可播放的Another Vocal版本
    - `anvo miku` : 随机播放一首指定角色的Another Vocal
>所有听歌指令共享每日次数限制。

### 数据与帮助
- `猜歌帮助`: 显示帮助信息。
- `群猜歌排行榜` / `gssrank`: 查看**本群**的猜歌排行榜。
- `猜歌排行榜` / `gslrank`: 查看连接的**服务器总排行榜** (需要配置API)。
- `本地猜歌排行榜` / `localrank`: 查看插件**本地数据库**中的总排行榜。
- `猜歌分数` / `gsscore`: 查看自己的积分、正确率和排名统计。
- `查看统计`: 查看所有游戏模式的正确率排名（优先显示服务器数据）。

### 管理员指令
- `测试猜歌 [模式,...] <歌曲名或ID>`: 生成一个测试游戏，立即公布答案，不计入统计。
    - 模式部分可选，支持数字或名称（如 `bass`, `reverse`），用逗号分隔。
    - **示例**: `测试猜歌 5,reverse Tell Your World`
- `重置猜歌次数 [用户ID]`: 重置指定用户的每日猜歌次数。
- `重置听歌次数 [用户ID]`: 重置指定用户的每日听歌次数。
- `重置题型统计`: 清空本地所有题型的统计数据。
- `同步分数`: 将本地未配置apikey前所有用户的总分强制同步至服务器。

## 4. 插件配置说明

插件的配置由机器人管理员通过 AstrBot 框架提供的 **WebUI 界面**进行修改。以下是可配置的选项说明：

```json
{
  "group_whitelist": [],
  "super_users": [],
  "answer_timeout": 30,
  "game_cooldown_seconds": 60,
  "daily_play_limit": 15,
  "daily_listen_limit": 5,
  "max_guess_attempts": 10,
  "clip_duration_seconds": 10,
  "bonus_time_after_first_answer": 5,
  "end_game_after_bonus_time": true,
  "debug_mode": false,
  "lightweight_mode": false,
  "use_local_resources": false,
  "remote_resource_url_base": "https://your.server.com/path/to/pjsk_resources",
  "stats_server_api_key": ""
}
```
- `group_whitelist` (列表): **群聊白名单**。只有在此列表中的群号才能使用本插件。若列表为空 `[]`，则对所有群聊生效。
- `super_users` (列表): **管理员QQ号列表**。
- `answer_timeout` (整数): 游戏回答的**超时时间**（秒）。
- `game_cooldown_seconds` (整数): 游戏结束后的**冷却时间**（秒）。
- `daily_play_limit` (整数): 每个用户每天可发起**猜歌游戏**的最大次数。
- `daily_listen_limit` (整数): 每日**听歌**次数限制，所有听歌指令共享此限制。
- `max_guess_attempts` (整数): 每轮游戏中，所有玩家总共可以**尝试回答**的次数。
- `clip_duration_seconds` (整数): 播放的音频**片段时长**（秒）。
- `bonus_time_after_first_answer` (整数): 首位答对者出现后，其他玩家可继续得分的**奖励时间**（秒）。
- `end_game_after_bonus_time` (布尔值): 是否在奖励时间结束后**立即结束游戏**。
- `debug_mode` (布尔值): **调试模式**。启用后，游戏会立即显示答案，不计入统计，适合测试。
- `lightweight_mode` (布尔值): **轻量模式**。启用后，会禁用“2倍速”、“倒放”等消耗CPU较高的效果，将其自动转为普通模式。
- `use_local_resources` (布尔值): **是否使用本地资源**。`false`为从远程URL加载，`true`为从插件`resources`目录加载。默认为`false`。
- `remote_resource_url_base` (字符串): **远程资源URL**。当`use_local_resources`为`false`时，从此URL获取音频等**媒体资源**。
- `stats_server_api_key` (字符串): **统计服务器API密钥**。用于连接后端服务器同步分数、排行榜和统计数据。留空则禁用所有在线功能。[点击前往领取密钥](http://47.110.56.9:5000/register)

## 4.1 群聊特定配置

除了通过WebUI进行全局配置外，本插件还支持为特定的群聊设置独立的配置，以满足不同群组的需求。这通过在插件根目录（`data/plugins/astrbot_plugin_pjsk_guess_song/`）下创建一个名为 `group_settings.json` 的文件来实现。

如果该文件不存在，插件会使用全局配置。如果文件存在，插件会加载它，并对文件中指定的群聊应用特定设置。

**文件格式示例:**

```json
{
    "123123": {
      "daily_play_limit": 50,
      "game_cooldown_seconds": 5,
      "daily_listen_limit": 50
    },
    "12312342": {
      "daily_play_limit": 50,
      "game_cooldown_seconds": 100
    }
}
```

**说明:**
- 文件的最外层是一个JSON对象。
- 对象的键（Key）是**群号**（必须是字符串格式）。
- 键对应的值（Value）是另一个JSON对象，包含了要为该群聊覆盖的配置项。
- 如果某个群聊在此文件中被配置，那么该群聊的相应设置将**优先于**WebUI中的全局设置。未在文件中指定的设置项仍会使用全局配置。

**可单独配置的选项:**
- `daily_play_limit`: 每日猜歌游戏次数。
- `daily_listen_limit`: 每日听歌次数。
- `game_cooldown_seconds`: 游戏冷却时间（秒）。
- `answer_timeout`: 游戏回答的超时时间（秒）。
- `max_guess_attempts`: 每轮游戏总计可尝试回答的次数。
- `bonus_time_after_first_answer`: 首位答对后，他人可继续得分的奖励时间（秒）。
- `end_game_after_bonus_time`: 是否在奖励时间结束后立即结束游戏。
- `independent_daily_limit` (布尔值): **独立每日次数限制**。
  - `true`: 本群的每日游戏次数将**独立计算**，不消耗用户的全局次数。
  - `false` (默认): 本群使用用户的全局游戏次数。
- `disable_guess_song_periods` (列表): **禁用猜歌时段**。
  - 在此列表中定义的时段内，所有“猜歌”类指令（`猜歌`、`随机猜歌`、`猜歌手`）将被禁用，但“听歌”类指令不受影响。
  - 列表中的每一项都是一个对象，包含 `start` 和 `end` 时间 (24小时制, `HH:MM`格式)，以及一个可选的自定义提示 `message`。
  - **注意**: 跨天的时段（如 23:00 - 07:00）需要拆分为两个时段配置。

**高级配置示例:**

```json
{
    "12345678": {
      "comment": "高强度游戏群，有独立的每日次数，且深夜禁用游戏",
      "daily_play_limit": 100,
      "independent_daily_limit": true,
      "disable_guess_song_periods": [
        { "start": "02:00", "end": "07:00", "message": "现在是深夜休息时间哦 (02:00-07:00)" }
      ]
    },
    "87654321": {
      "comment": "休闲群，使用全局次数，但有午休时段",
      "game_cooldown_seconds": 120,
      "disable_guess_song_periods": [
        { "start": "12:00", "end": "13:30" }
      ]
    }
}
```

## 5. 一些更新说明

在普通猜歌中问题似乎依然存在,锁没有释放，进行了可能的修改
- **Commit:** [`6c642db`](https://github.com/nichinichisou0609/astrbot_plugin_pjsk_guess_song/commit/6c642dbddd775e5817d595366362c5a7f2612913) - fix(session): Ensure game session lock is always released on error

建议拉取最新的修改或者至少进行以下修改，否则在一些有发言频率限制的群聊发送失败时，会造成锁无法释放，重启机器人才能继续进行游戏。
- **Commit:** [`94571a9`](https://github.com/nichinichisou0609/astrbot_plugin_pjsk_guess_song/commit/94571a9bc2213e7151ec5ce79c374764f6c77656) - fix(session): Ensure game session is properly cleaned up on send failure


