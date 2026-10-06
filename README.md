# 音叙 SoundTale

**把音乐，留在本地。**

一个只读你指定文件夹的本地音乐播放器；同时它把自己封装成一个 **MCP 服务器**，让支持 MCP 的 AI 客户端能够读到你的曲库，也能替你控制播放。

> 这个仓库只放 **安装包与使用说明**，不放源码。

---

## 下载

到 [Releases](../../releases/latest) 下载最新的 `soundtale-<版本>.apk`，直接安装。

- 系统要求：Android 7.0（API 24）及以上
- 覆盖安装：applicationId 相同、签名一致、versionCode 递增即可直接覆盖，不用卸载
- 官方安装包只从本仓库的 Releases 发布，签名指纹见文末「官方构建」

## 它是什么

- **只读你授权的文件夹**：通过系统文件选择器（SAF）授权，不申请整盘存储权限，也不扫描你没让扫的目录
- 支持 **MP3 / FLAC / M4A / AAC / OGG / OPUS / WAV**
- **歌词只在本地找**：读歌曲同目录的 `.lrc` 文件，不联网匹配，也不会随机绑一首歌词给你
- **播放页**：封面模糊成背景 + 黑胶唱片；点一下唱片，唱片区域就变成歌词，在同一个页面切换
- **播放队列**：单独一张卡片，长按歌曲可以拖动调整顺序
- **封面**：歌单和文件夹的封面会自动取目录里的图片，不满意也能自己传一张
- **主题**：六套主色（森绿 / 云红 / 靛蓝 / 暖橙 / 紫罗兰 / 青碧），浅色深色各一套
- **没有广告，不需要登录，不联网**（除了你自己手动打开的 MCP 服务）

## MCP：把曲库和播放习惯，变成工具

服务**默认关闭**。打开后只监听 `127.0.0.1`，请求必须带 Bearer 令牌。

| | |
|---|---|
| 端点 | `http://127.0.0.1:8765/mcp` |
| 认证 | 请求头 `Authorization: Bearer <令牌>`，令牌在 App 的「MCP 服务」页里复制 |
| 传输 | Streamable HTTP，只用 POST |
| 权限 | 播放控制 / 读取音乐库 / 读取播放历史 / 修改歌单，四类可以分别关掉 |

**35 个工具，六类：**

- 曲库查询 6 · `search_music` `get_music_library` `get_artists` `get_albums` `get_playlists` `get_playlist_tracks`
- 播放控制 12 · `play_music` `pause_music` `resume_music` `next_track` `previous_track` `seek` `set_repeat_mode` `set_shuffle` `set_playback_speed` `play_artist` `play_album` `play_playlist`
- 播放状态 2 · `get_now_playing` `get_playback_queue`
- 歌词 2 · `get_lyrics` `get_current_lyrics`
- 播放历史与统计 10 · `get_playback_history` `get_recently_played` `get_most_played` `get_listening_statistics` `get_top_tracks` `get_top_artists` `get_top_albums` `get_listening_by_time` `get_listening_history` `get_track_statistics`
- 歌单编辑 3 · `create_playlist` `add_to_playlist` `remove_from_playlist`

**在客户端里怎么填**（示例，字段名以你所用的客户端文档为准）：

```json
{
  "mcpServers": {
    "soundtale": {
      "type": "streamable-http",
      "url": "http://127.0.0.1:8765/mcp",
      "headers": {
        "Authorization": "Bearer <在 App 里复制的令牌>"
      }
    }
  }
}
```

只想给同网段的其他设备用，再打开 App 里的「允许局域网访问」，并把地址里的 `127.0.0.1` 换成手机的内网 IP；确有必要时再开。

## 权限与隐私

- 曲库索引、歌单、播放统计、设置**全部存在本机**
- MCP 服务不联网、不上传任何内容；只有你手动开启时才在本机监听端口
- 卸载 App 会一并清掉本机数据；音乐文件本身不会被移动、复制或修改

## 官方构建

官方安装包只从本仓库的 [Releases](../../releases) 发布。

```
签名证书 SHA-256:
90:E0:CE:43:FB:1F:ED:C2:5A:E1:58:1A:24:64:89:2A:
66:9D:C3:35:94:18:9B:40:23:E4:E1:9D:9C:EE:E3:6F
```

校验方式（需要 Android SDK 的 apksigner）：

```bash
apksigner verify --print-certs soundtale.apk
```

指纹不一致的安装包**不是**官方版本，请不要安装。

## 许可

个人使用免费，详见 [EULA.md](EULA.md)：可以自由安装与分享，**不允许**二次打包、改名分发或收费转售。商业授权请在 [Issues](../../issues) 里联系作者。

## 反馈

安装、覆盖升级、MCP 接入的问题都可以开 [Issue](../../issues)；反馈时请附上 Android 版本、手机型号和 App 版本号。
