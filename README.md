# dsh-vibegap

Vocabulary flashcards for the gaps in vibe coding. A word card appears in the
dsh web UI after your agent has been running for ~18 seconds, and retreats when
the session finishes or needs your attention. Fast tasks never trigger it.

Part of [VibeGap](https://github.com/ktao732084-arch/vibegap). Works standalone —
the VibeGap desktop app is not required. If the local VibeGap daemon is running,
the card automatically shares the desktop wordbook and progress cursor, and falls
back to in-browser progress when the daemon is unreachable.

## Install

```bash
dsh plugin --profile web add "github:ktao732084-arch/dsh-vibegap#main"
dsh web
```

Uninstall:

```bash
dsh plugin --profile web remove dsh-vibegap
```

## How it works

- First launch: click the download button on the card to fetch the CET6
  wordbook (~0.5 MB, from [qwerty-learner](https://github.com/RealKai42/qwerty-learner),
  downloaded on demand — never bundled).
- Type the word from its meaning; correct letters light up, mistakes shake and
  reset. `Tab` toggles the answer (peeking marks the word as failed). `Esc`
  hides the card until the next batch of work starts.
- Wordbook, shuffle seed, cursor and pronunciation preference persist in the
  browser via dsh's official snapshot store. Private mode still works, just
  without persistence.
- With a local VibeGap daemon, words and progress come from
  `http://127.0.0.1:8765/panel/*` (CORS restricted to localhost origins).

## Development

The browser half (`lib/client.js`) is a hand-written ModuleLoader bundle — no
webpack, no tsc. Edit and refresh. The node half (`lib/index.js`) reports agent
lifecycle to a local VibeGap daemon when present and silently degrades otherwise.

Development happens in the [VibeGap monorepo](https://github.com/ktao732084-arch/vibegap)
(`vibegap/adapters/dsh/plugin/`); this repository is the release mirror that
`dsh plugin add` installs from. Issues and PRs are welcome in either place.

## License

MIT. The CET6 wordbook is downloaded at runtime from qwerty-learner (GPL-3.0
project); the data is never redistributed with this plugin — please respect the
upstream licenses.

---

## 中文说明

vibe coding 间隙背单词:agent 持续运行约 18 秒后,dsh web 右下角浮现拼写单词卡;
会话完成或等待确认时,拼完当前词自动收起。快任务全程不打扰。

安装:`dsh plugin --profile web add "github:ktao732084-arch/dsh-vibegap#main"`,
卸载 `... remove dsh-vibegap`。首次点击卡片上的"下载词库"获取 CET6(按需下载,
不随插件分发)。进度经 dsh 官方 snapshot store 存在浏览器里;本机运行
VibeGap 桌面端 daemon 时自动共享桌面词书与游标。

开发主场在 [VibeGap monorepo](https://github.com/ktao732084-arch/vibegap),
本仓库是安装用的发布镜像。
