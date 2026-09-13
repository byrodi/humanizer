# humanizer

A Claude Code plugin with one skill that edits Russian and English prose. It removes signs of AI generation, bureaucratic phrasing and filler, keeps every fact, number and link of the original, and checks the result with a deterministic linter. The skill also runs as the final pass over a draft the agent wrote for another person, and it can audit a text for AI patterns without changing it.

## Features

- Pattern catalogues for both languages, 38 Russian patterns and 35 English ones (E1-E35), each with markers and before and after examples.
- Hard bans that always fail a draft, such as `not just X but Y` constructions, em and en dashes, math signs in prose, rhetorical questions and decorative emoji.
- A lint script (`scripts/lint.py`, Python 3 standard library only). It reports hard bans as errors with exit code 1 and pattern markers as warnings, and it catches chatbot copy-paste artifacts such as `oaicite` footnotes, `turn0search` markers and `utm_source=chatgpt.com`.
- Detect mode, which returns a qualitative verdict with the list of patterns found and quotes. Detection is heuristic, so the skill gives no percentages.
- Draft-for-others mode. When the agent writes a message to a colleague, a ticket comment, a PR description or a document, the skill runs all three phases over that draft and returns plain text without code fences, block quotes or backticks, ready to paste.
- A messaging mode with registers for personal chat, work chat and formal letters.

## Usage

Call the skill directly with `/humanizer:humanizer`, or let the agent pick it up from a request like these:

- "убери воду из этого текста"
- "humanize this paragraph"
- "проверь на AI, это писала нейросеть?"
- "write a reply to the QA team about the fix and run it through humanizer"

The skill does not generate content from nothing, translate, fix spelling, or edit code, configs, legal and academic texts.

Run the linter on its own:

```sh
python3 skills/humanizer/scripts/lint.py draft.md
python3 skills/humanizer/scripts/lint.py < draft.md
python3 skills/humanizer/scripts/lint.py --self-test
```

## Install for Claude Code

```sh
git clone https://github.com/stependiant/humanizer ~/IdeaProjects/skills/humanizer
ln -s ~/IdeaProjects/skills/humanizer ~/.claude/skills/humanizer
```

Remove or rename an existing `~/.claude/skills/humanizer` first. A new session loads the plugin as `humanizer@skills-dir`, and `git pull` in the clone updates it.

## Install for Codex

```sh
ln -s ~/IdeaProjects/skills/humanizer/skills/humanizer ~/.agents/skills/humanizer
```

Codex does not expand `${CLAUDE_SKILL_DIR}` in SKILL.md. The skill explains that the variable means its own folder, so paths still resolve.

## Repository layout

```
humanizer/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── humanizer/
│       ├── SKILL.md
│       ├── references/
│       │   ├── corrections.md    owner feedback, read in the audit phase
│       │   ├── patterns-en.md    English patterns E1-E35
│       │   └── patterns-ru.md    Russian patterns 1-38
│       └── scripts/
│           └── lint.py
├── LICENSE
├── README.md
└── THIRD_PARTY.md
```

## Credits and license

The skill combines and adapts [smixs/humanizer-ru](https://github.com/smixs/humanizer-ru) and [blader/humanizer](https://github.com/blader/humanizer), with the copy-paste artifact registry from [Vladimir-Human/humanizer-ru](https://github.com/Vladimir-Human/humanizer-ru) and [petergyang/no-ai-slop](https://github.com/petergyang/no-ai-slop). The patterns build on [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing). Upstream license texts are in [THIRD_PARTY.md](THIRD_PARTY.md).

Released under the MIT License, see [LICENSE](LICENSE).
