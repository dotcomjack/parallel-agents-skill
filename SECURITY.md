<!-- █ dcj · dotcomjack.com · MIT -->
# Security

This repository contains no executable code. It is a Claude Code skill: about
190 lines of Markdown that an AI coding agent reads and follows.

That does not make it inert. **Instructions an autonomous agent obeys are a real
attack surface**, and in some ways a worse one than a script, because the agent
has your credentials, your shell, and your repository, and it will act on
plausible sounding text without the pause a human would take. Please read this
repository the way you would read a script you were about to `sudo`.

## Reporting a vulnerability

**Do not open a public issue for a security problem.** Two private channels:

1. [Open a private advisory](https://github.com/dotcomjack/parallel-agents-skill/security/advisories/new)
   on this repository. Preferred.
2. Email **jack@dotcomjack.com** with `parallel-agents-skill security` in the
   subject.

**Response:** maintained by one person. Acknowledgement within 3 business days,
first assessment within 7. Valid reports are credited in the advisory unless you
ask otherwise.

## What counts as a vulnerability here

This is the part that differs from a normal repository. All of these are real
and in scope:

* **Injected instructions.** Any text in `SKILL.md` or `README.md` that would
  steer an agent toward exfiltrating data, contacting a network endpoint,
  reading credentials, or running a destructive command. Including text that
  only does so in combination with a plausible user request.
* **Hidden or obfuscated content.** Zero width characters, HTML comments,
  base64, homoglyphs, or anything else that reads differently to an agent than
  it does to a human reviewing the diff.
* **Overbroad delegation.** Guidance that causes the coordinating agent to hand
  a subagent more scope, more tools, or more trust than the task needs.
* **Unsafe concurrency guidance.** Advice that would have parallel agents write
  the same files or share state in a way that silently corrupts a working tree.

## Out of scope

* Claude Code itself, the Agent tool, and any model behaviour. Report those to
  [anthropics/claude-code](https://github.com/anthropics/claude-code/issues).
* The upstream [obra/superpowers](https://github.com/obra/superpowers) skill
  this one rewrites. Report there.
* Disagreement about whether the dispatch advice is good advice. That is a
  regular issue or pull request, and it is welcome as one.

## Verifying this yourself

The entire repository is two Markdown files and a license. Read them:

```sh
git clone https://github.com/dotcomjack/parallel-agents-skill
cd parallel-agents-skill
wc -l SKILL.md README.md          # about 190 lines total
cat SKILL.md                       # the whole thing, in one sitting

# Check for hidden characters that render invisibly
LC_ALL=C grep -nP '[^\x00-\x7F]' SKILL.md README.md || echo "ASCII only, nothing hidden"
```

There is nothing to build, nothing to install, and no network access anywhere in
it. If a copy of this skill tells you to run something, it did not come from
here.

## Supported versions

Only the current `main` is supported. Skills are not versioned. Pull before
reporting.
