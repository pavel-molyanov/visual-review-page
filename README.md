**English** | [Русский](README.ru.md)

# Visual Review Page

**Telegram:** [@molyanov_blog](https://t.me/+JeJiLYwo9E81NWJi)

A skill for [Claude Code](https://claude.com/claude-code) and [Codex](https://github.com/openai/codex)
that turns a plan, decision, or proposal into a local web page you can look at and answer on,
instead of a long wall of text in chat.

The agent shows what is hard to agree on in words: options side by side on your own material,
zones drawn over a real screenshot, before and after, a single frame where something breaks, or
an audio player that switches between variants without losing the playback position. Every
decision is explained in plain words, what happens now and what will happen after, without code
or diffs. Under each decision there is a field for your answer.

## How it works

1. Ask the agent to show a plan visually, for example: "show me this plan as a page", "pack the
   options into a page", "present the solution visually". A plain request to discuss a plan stays
   in chat.
2. The agent builds the page in a folder inside your project, starts a small local server, and
   opens the page in its built-in browser. If your agent has no browser, open the printed
   `http://127.0.0.1:<port>/` link yourself.
3. Write your answers in the fields. They are saved as you type into `answers.json` in the page
   folder. An empty field means "not answered", never "agreed".
4. Tell the agent in chat that you have answered, in any words. It reads your answers, revises the
   page, and shows above each answer what it changed or why not. Agreed items collapse. Your
   answers from each round are kept as `answers-<round>.json`, so the fields start empty again
   for the next round.
5. Work starts only when you explicitly tell the agent to do it. Answers on the page agree on or
   change items; they never start the work.

## Install

The skill is one folder, `visual-review-page/`, used as is by both runtimes. The commands below
are for macOS and Linux; on Windows, run them in WSL.

```bash
git clone https://github.com/pavel-molyanov/visual-review-page.git
```

Claude Code:

```bash
mkdir -p ~/.claude/skills && cp -R visual-review-page/visual-review-page ~/.claude/skills/
```

Codex:

```bash
mkdir -p ~/.agents/skills && cp -R visual-review-page/visual-review-page ~/.agents/skills/
```

Downloaded a ZIP archive instead? Unpack it and copy the inner `visual-review-page/` folder, the
one that contains `SKILL.md`, the same way.

Codex also reads the older `~/.codex/skills/` folder. To use the skill in one project only, copy
the folder into that project's `.claude/skills/` or `.agents/skills/` instead. Restart the agent
session after installing.

To update, delete the installed folder and copy the new version again, so files removed in the
new version do not stay behind (for Codex, use `~/.agents/skills` in the second line):

```bash
git -C visual-review-page pull
rm -rf ~/.claude/skills/visual-review-page && cp -R visual-review-page/visual-review-page ~/.claude/skills/
```

## Requirements

- Claude Code and/or Codex
- Python 3.7 or newer (the server uses only the standard library)
- A browser; the built-in browser of the Claude or Codex desktop app works best, because the agent
  can check the page before showing it to you

## Privacy

The server listens only on `127.0.0.1` and answers only the page itself: requests with a foreign
`Host` header or from another site are rejected. Your answers are saved in `answers.json` in the
page folder; the agent reads them like a chat message, and nothing else is sent anywhere. The
agent does not commit page folders unless you ask; if you commit everything in your project, add
them to `.gitignore`.

## What is inside

| File | Purpose |
|---|---|
| `visual-review-page/SKILL.md` | Instructions for the agent: what to show, how to word decisions, page shape, answer rounds |
| `visual-review-page/assets/page.html` | Page template: one reading column, answer fields, autosave |
| `visual-review-page/scripts/serve.py` | Local server: serves the page and media (with seeking), stores answers |

## License

[MIT](LICENSE)

## Author

Pavel Molyanov

My website: [molyanov.ru](https://molyanov.ru)

I write a Telegram channel about AI, entrepreneurship, management, and
vibecoding: [@molyanov_blog](https://t.me/+JeJiLYwo9E81NWJi)
