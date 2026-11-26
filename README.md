# Irreplaceable Handoff

A compact, security-hardened, universal skill (`SKILL.md`) for producing high-signal end-of-session handoffs across coding, DevOps, research, writing, and planning workflows. Designed so an incoming agent instance can resume work seamlessly after `/clear`, a context reset, or switching tools.

Built on the [Agent Skills Open Standard](https://agentskills.io/) with capability-based adapters, ISO 8601 dated filenames, and an explicit security boundary so it works reliably across current and future runtimes.

## Repository layout

Keep `README.md` at the repository root—**not** inside the `irreplaceable-handoff/` skill folder—so agent skill loaders only ingest `SKILL.md`:

```text
irreplaceable-handoff/
├── README.md
├── LICENSE
├── .gitignore
├── examples/
│   ├── 2026-11-25-handoff-docker-reverse-proxy.md
│   └── 2026-11-25-handoff-newsletter-editorial-plan.md
└── skills/
    └── irreplaceable-handoff/
        └── SKILL.md
```

## Features

- **Future-proof capability adapters:** Groups runtimes by capability archetype (Local CLI, IDE, Always-on Cloud Agents, Stateless Web Chats) rather than hardcoding brittle tool internals.
- **Universal across domains:** Covers software engineering, sysadmin/ops, research, content creation, and planning—with an optional **quick mode** for shorter sessions.
- **Verified vs. unconfirmed state:** Explicitly tags completed work and key facts as `[verified]` or `[unconfirmed]` and surfaces conflicting facts instead of guessing.
- **Dead-end tracking:** Records failed approaches and discovered constraints so the incoming agent does not repeat them.
- **Ordered next actions + instant resume line:** Pairs a single `Start here` step with an optional short ordered queue (`Then`), and prints a copy-ready one-line resume prompt with the exact saved path at the end of the chat reply.
- **Dated, PKM- and dreaming-friendly filenames:** Prints the handoff in chat and saves it as `YYYY-MM-DD-handoff-<short-slug>.md` (e.g., `2026-11-25-handoff-auth-refactor.md`) when filesystem access is available—ideal for Obsidian vaults, chronological archives, and downstream memory consolidation workflows.
- **Security-hardened by default:** Pure Markdown with a dedicated `Security boundary` section (zero scripts, zero network fetches, read-only checks, secret/PII redaction, workspace-scoped writes, and indirect prompt-injection defense).
- **Multilingual output:** Automatically matches the primary language of the conversation while preserving paths, commands, URLs, and identifiers verbatim.

## Security & trust

- **Zero executable code or external fetches:** This skill consists solely of `SKILL.md` (105 lines) with no bundled scripts and no runtime URL fetches—you can audit the entire file in under two minutes and it passes automated skill scanners cleanly.
- **Explicit `Security boundary`:** All security constraints live in a dedicated section inside `SKILL.md` covering read-only inspection, workspace-scoped sanitized `[a-z0-9-]` writes, secret/PII redaction, and indirect prompt-injection defense.
- **Read-only inspection:** Any state verification during handoff generation is strictly read-only; the skill never stops processes, modifies Git history, or runs destructive commands.
- **Review shared handoffs:** Always review generated handoffs before sharing or committing them, and inspect any handoff created by someone else before passing it to an agent.

## Examples

See sample outputs generated with this skill:
- **Technical (DevOps / Docker):** [examples/2026-11-25-handoff-docker-reverse-proxy.md](examples/2026-11-25-handoff-docker-reverse-proxy.md)
- **Non-technical (Editorial / Planning):** [examples/2026-11-25-handoff-newsletter-editorial-plan.md](examples/2026-11-25-handoff-newsletter-editorial-plan.md)

## How triggers work

Modern agent runtimes rely on **semantic routing** against the YAML frontmatter `description` in `SKILL.md`. Any natural phrasing expressing the intent to wrap up or hand off works in any language. You can also combine triggers with modifiers such as **quick/light** (compact one-line bullets), **chat only**, or a **custom file path**.

### Recommended trigger phrases

| Language | Base triggers | Variants with modifiers |
| :--- | :--- | :--- |
| English | `session handoff`, `wrap up session`, `hand off`, `summarize before I clear` | `quick handoff`, `session handoff, chat only`, `wrap up and save to docs/handoff.md` |
| Italian | `fai handoff`, `chiudiamo la sessione`, `riassunto di fine sessione`, `prima di fare clear` | `fai quick handoff`, `fai handoff solo in chat`, `fai handoff e salva in docs/handoff.md` |
| French | `fais un handoff`, `clôturons la session`, `résumé de fin de session`, `résume avant clear` | `handoff rapide`, `fais un handoff uniquement dans le chat`, `clôturons et sauvegarde dans docs/handoff.md` |
| German | `Session-Handoff`, `Session abschließen`, `Übergabe erstellen`, `Zusammenfassung vor Clear` | `kurzes Handoff`, `Session-Handoff nur im Chat`, `Übergabe in docs/handoff.md speichern` |
| Spanish | `haz un handoff`, `cerremos la sesión`, `resumen de fin de sesión`, `resume antes de clear` | `handoff rápido`, `haz un handoff solo en el chat`, `cerremos y guarda en docs/handoff.md` |
| Portuguese | `faça um handoff`, `encerrar a sessão`, `resumo de fim de sessão`, `resuma antes do clear` | `handoff rápido`, `faça um handoff só no chat`, `encerrar e salvar em docs/handoff.md` |
| Chinese (Simplified) | `生成交接总结`, `结束当前会话`, `会话交接`, `清空上下文前总结` | `快速交接总结`, `仅在聊天中生成交接总结`, `生成交接并保存到 docs/handoff.md` |
| Japanese | `セッションを引き継ぐ`, `ハンドオフを作成`, `セッション終了まとめ`, `クリア前に要約` | `クイックハンドオフ`, `チャットのみでハンドオフ作成`, `ハンドオフを docs/handoff.md に保存` |
| Korean | `세션 핸드오프`, `세션 마무리 요약`, `컨텍스트 초기화 전 정리`, `인수인계 작성` | `간단 핸드오프`, `채팅으로만 핸드오프 작성`, `핸드오프를 docs/handoff.md에 저장` |
| Hindi | `सेशन हैंडऑफ़ बनाओ`, `सेशन का सारांश दो`, `क्लियर करने से पहले समरी दो`, `हैंडऑफ़ तैयार करो` | `क्विक हैंडऑफ़ बनाओ`, `केवल चैट में हैंडऑफ़ दो`, `हैंडऑफ़ को docs/handoff.md में सेव करो` |

## Installation

### Quick install (Agent Skills CLI)
If your environment supports the `skills` CLI (Claude Code, Cursor, Codex, Gemini CLI, etc.):
```bash
npx skills add Undec0de/irreplaceable-handoff
```

### Local CLI agents

- **Claude Code:** Copy `skills/irreplaceable-handoff/SKILL.md` to `~/.claude/skills/irreplaceable-handoff/SKILL.md` (global) or `.claude/skills/irreplaceable-handoff/SKILL.md` (project-scoped).
- **Hermes Agent:** Copy `skills/irreplaceable-handoff/SKILL.md` to `~/.hermes/skills/irreplaceable-handoff/SKILL.md` (verify with `hermes skills list`), or install directly from GitHub:
  ```bash
  hermes skills install https://raw.githubusercontent.com/Undec0de/irreplaceable-handoff/main/skills/irreplaceable-handoff/SKILL.md
  ```
- **Codex CLI / OpenClaw:** Copy `skills/irreplaceable-handoff/SKILL.md` to `~/.agents/skills/irreplaceable-handoff/SKILL.md` (or `~/.codex/skills/irreplaceable-handoff/SKILL.md`).
- **Gemini CLI:** Copy `skills/irreplaceable-handoff/SKILL.md` to `~/.gemini/skills/irreplaceable-handoff/SKILL.md` (or `.gemini/skills/irreplaceable-handoff/SKILL.md`).

### IDE agents (Cursor, Windsurf, VS Code / Copilot)
- Place `skills/irreplaceable-handoff/SKILL.md` in `.agents/skills/irreplaceable-handoff/SKILL.md` (or `.github/skills/irreplaceable-handoff/SKILL.md` for GitHub Copilot), or import it as a rule file (e.g., `.cursor/rules/irreplaceable-handoff.mdc`).

### Always-on / proactive cloud agents
- **Perplexity Computer:** Open **Plugins** → **Add custom** → **Upload plugin or Skill** and upload `SKILL.md` once so it stays available globally across all sessions and Projects.
- **Claude Duet:** Add `SKILL.md` to your **Claude.ai** account skills (Duet reads skills and connectors from Claude.ai, not from local Claude Code directories).
- **OpenAI Dots:** Attach `SKILL.md` to your Dots agent instructions/knowledge or invoke it as a reusable routine.
- **Grok Bot:** Save the workflow in your Grok Bot instructions or Chief of Staff routine so specialist bots hand off state consistently.
- **Meta Muse:** Paste or attach the skill instructions in your agent preferences/routines.

### Stateless web chats (Claude.ai, ChatGPT, Perplexity, Gemini)
- Attach `SKILL.md` to a **Project** (Claude, Perplexity) or **Custom GPT / Gem**. Without local disk access, the skill outputs a clean, copy-ready Markdown block.

## Optional automation (platform recipes)

`SKILL.md` intentionally ships with **zero hooks or executable scripts** so it stays portable and passes strict security scans. If you want automatic reminders on your favorite platform, you can opt in using the lightweight recipes below:

### Claude Code (`~/.claude/settings.json`)
Add a zero-script `PreCompact` hook (whose stdout guides compaction to preserve handoff fields) and a `SessionStart` reminder after compaction:
```json
{
  "hooks": {
    "PreCompact": [
      {
        "matcher": "manual|auto",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Preserve all [verified]/[unconfirmed] decisions, dead ends, absolute paths, active processes, and Next actions—or advise the user to run irreplaceable-handoff.'"
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "matcher": "compact",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'If a recent *-handoff-*.md file exists in this workspace, check its Next actions section.'"
          }
        ]
      }
    ]
  }
}
```

### Cursor / Windsurf / `AGENTS.md`
Add two lines to your global or workspace instructions (`AGENTS.md` or `.cursor/rules/`):
```text
- Before switching topics or ending a long multi-step chat, proactively offer to run the `irreplaceable-handoff` skill.
- When starting a fresh chat in an existing workspace, check for the latest `*-handoff-*.md` file if the user asks to resume.
```

### Obsidian & PKM vaults
Save handoffs directly into a dedicated vault folder by passing a custom path override (e.g., `wrap up and save to ~/Vault/Handoffs/`). The `YYYY-MM-DD-handoff-<short-slug>.md` naming keeps every session chronologically sorted alongside Daily Notes.

### Always-on cloud agents (Perplexity Computer, Claude Duet, Grok Bot, OpenAI Dots)
Create a lightweight end-of-day or end-of-task routine that invokes `irreplaceable-handoff` and routes the output to your shared Notion database, Google Drive folder, or workspace hub.

## Managing handoff files

- **Default filename format:** `YYYY-MM-DD-handoff-<short-slug>.md` (lowercase ASCII kebab-case, e.g., `2026-11-25-handoff-auth-refactor.md`). If a file with the same name already exists, the agent appends `-2`, `-3`, etc. instead of overwriting your previous handoff.
- **Avoid accidental Git commits:** Add these patterns to your project `.gitignore` or global ignore file (`git config --global core.excludesfile ~/.gitignore_global`):
  ```gitignore
  # Session handoff files
  *-handoff-*.md
  HANDOFF.md
  !examples/*-handoff-*.md
  ```

## Resuming in a fresh session

At the end of every handoff, the agent prints a copy-ready one-line resume prompt in chat with the exact file path. You can also use any of the prompts below (matching your conversation language):

| Language | Resume from latest file (CLI / IDE) | Resume from pasted handoff (Web chat / Cloud agent) |
| :--- | :--- | :--- |
| English | `Read the latest *-handoff-*.md file and resume from "Next actions".` | `Here is the handoff from our previous session. Resume from "Next actions".` |
| Italian | `Leggi il file *-handoff-*.md più recente e riparti da "Next actions".` | `Ecco l'handoff della sessione precedente. Riparti da "Next actions".` |
| French | `Lis le fichier *-handoff-*.md le plus récent et reprends à partir de "Next actions".` | `Voici le handoff de la session précédente. Reprends à partir de "Next actions".` |
| German | `Lies die neueste *-handoff-*.md-Datei und mache bei "Next actions" weiter.` | `Hier ist das Handoff aus der vorherigen Session. Mache bei "Next actions" weiter.` |
| Spanish | `Lee el archivo *-handoff-*.md más reciente y retoma desde "Next actions".` | `Aquí tienes el handoff de la sesión anterior. Retoma desde "Next actions".` |
| Portuguese | `Leia o arquivo *-handoff-*.md mais recente e retome a partir de "Next actions".` | `Aqui está o handoff da sessão anterior. Retome a partir de "Next actions".` |
| Chinese (Simplified) | `读取最新的 *-handoff-*.md 文件，并从 "Next actions" 继续。` | `这是上次会话的交接总结，请从 "Next actions" 继续。` |
| Japanese | `最新の *-handoff-*.md を読み込み、"Next actions" から再開してください。` | `前回のセッションのハンドオフです。"Next actions" から再開してください。` |
| Korean | `가장 최근 *-handoff-*.md 파일을 읽고 "Next actions"부터 이어서 진행해 주세요.` | `이전 세션의 핸드오프입니다. "Next actions"부터 이어서 진행해 주세요.` |
| Hindi | `सबसे नई *-handoff-*.md फ़ाइल पढ़ो और "Next actions" से आगे बढ़ाओ।` | `यह पिछली सेशन का हैंडऑफ़ है। "Next actions" से आगे बढ़ाओ।` |

## Maintainer

Created and maintained by [Undec0de](https://github.com/Undec0de) (Andrea M. Amedeo).

## License

MIT — see [LICENSE](LICENSE) for details.
