# humanizer

[![CI](https://github.com/byrodi/humanizer/actions/workflows/ci.yml/badge.svg)](https://github.com/byrodi/humanizer/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/byrodi/humanizer)](LICENSE)

Agent skill that removes signs of AI writing, bureaucratic phrasing and filler from Russian and English text.

The agent picks it up when you ask to edit or check a text, and as a final pass over drafts it writes for other people. Facts, numbers and links stay. Code, configs, translation, spelling, legal and academic texts are out of scope.

## How it works

1. Audit. Lists every pattern found in the text, with a quote, before editing.
2. Edit. Fixes only those findings, cutting filler before rewriting anything.
3. Check. Runs the linter until no errors remain, then rereads against a checklist.

## What you get

- Pattern catalogues for Russian (38) and English (35), with before and after examples.
- A linter that fails dashes, negative parallelisms and chatbot copy-paste traces.
- Detect mode with a verdict in words and quoted evidence, no percentages.
- Drafts for colleagues, tickets and PRs returned as plain text, ready to paste.
- Registers for personal chat, work chat and formal letters.

## Usage

Call `/humanizer:humanizer`, or ask in your own words:

- "убери воду из этого текста"
- "humanize this paragraph"
- "проверь на AI, это писала нейросеть?"

The linter also runs on its own. It needs only Python 3, prints its report in Russian and exits with 1 when a hard ban is found.

```sh
python3 skills/humanizer/scripts/lint.py draft.md
python3 skills/humanizer/scripts/lint.py < draft.md
python3 skills/humanizer/scripts/lint.py --self-test
```

## Install

Claude Code (remove or rename an existing `~/.claude/skills/humanizer` first):

```sh
git clone https://github.com/byrodi/humanizer ~/IdeaProjects/skills/humanizer
ln -s ~/IdeaProjects/skills/humanizer ~/.claude/skills/humanizer
```

A new session loads the plugin as `humanizer@skills-dir`. Run `git pull` in the clone to update.

Codex:

```sh
ln -s ~/IdeaProjects/skills/humanizer/skills/humanizer ~/.agents/skills/humanizer
```

Codex does not expand `${CLAUDE_SKILL_DIR}`, and SKILL.md tells the agent to read it as the skill's own folder.

## Credits and license

The skill combines and adapts [smixs/humanizer-ru](https://github.com/smixs/humanizer-ru) and [blader/humanizer](https://github.com/blader/humanizer), with the copy-paste artifact registry from [Vladimir-Human/humanizer-ru](https://github.com/Vladimir-Human/humanizer-ru) and [petergyang/no-ai-slop](https://github.com/petergyang/no-ai-slop). The patterns build on [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing). Upstream license texts are in [THIRD_PARTY.md](THIRD_PARTY.md).

Released under the [MIT License](LICENSE).
