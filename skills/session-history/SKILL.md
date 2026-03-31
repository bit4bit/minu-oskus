---
name: llm-vcs-session-saver
description: Save and format LLM conversation sessions as version-control-friendly flat text files, then commit them. Use this skill whenever the user wants to log, record, archive, or commit a conversation/chat session to any version control system (Git, SVN, Mercurial, Perforce, etc.), or asks for session persistence, conversation history files, or audit trails of LLM interactions. Also trigger automatically whenever the user asks to make ANY VCS commit or check-in — always save the current session before or alongside the commit. Trigger even if the user just says "save this session", "log this chat", "commit my changes", "git commit", "svn commit", "hg commit", or "check in".
---

# LLM VCS Session Saver

Saves an LLM conversation session as a flat text file optimized for version control: one line per message, unique timestamped filename, no identifying information. Works with any VCS — Git, SVN, Mercurial, Perforce, and others.

---

## Output File Path

Save the session file to:

```
llm-sessions/YYYY-MM-DD-HHMMSS-XXXX-session.txt
```

- `YYYY-MM-DD-HHMMSS` — timestamp of when the session was saved (UTC)
- `XXXX` — 4-character random alphanumeric string to prevent collisions
- Do **not** include usernames, email addresses, or any identifying info in the filename or content

Example: `llm-sessions/2025-04-01-143022-k7qr-session.txt`

---

## Content Format

Each message must be written as a **single line** in this exact format:

```
[YYYY-MM-DD HH:MM:SS] User: <message text>
[YYYY-MM-DD HH:MM:SS] LLM: <message text>
```

### Rules

- **Chronological order** — messages appear in the order they occurred
- **Timestamps** — use `YYYY-MM-DD HH:MM:SS` format (UTC preferred)
- **Speaker labels** — only `User:` and `LLM:` (no other labels or roles)
- **One line per message** — newlines within a message must be replaced with a space or `\n` literal so each message stays on one line
- **No summarizing** — reproduce messages fully and verbatim
- **No extra formatting** — no markdown, no section headers, no blank lines between messages
- **No metadata block** — do not prepend titles, model names, or session IDs

### Example Output

```
[2025-04-01 14:30:01] User: What is the capital of France?
[2025-04-01 14:30:03] LLM: The capital of France is Paris.
[2025-04-01 14:30:10] User: And what's the population?
[2025-04-01 14:30:12] LLM: Paris has a population of approximately 2.1 million in the city proper.
```

---

## Git Readiness

- The file can be written directly and committed with no post-processing
- One-line-per-message ensures Git diffs are clean and human-readable — each changed message shows as exactly one modified line
- The `llm-sessions/` directory should be committed as part of the repo (not gitignored)

---

## Commit Workflow

Every time the user asks to make a Git commit — for any reason, even if unrelated to sessions — run this skill first, then include the session file in the commit.

Steps to follow on every commit:

1. **Generate the session file** using the path format and content rules above
2. **Stage the session file** along with the user's other changes:
   ```bash
   git add llm-sessions/YYYY-MM-DD-HHMMSS-XXXX-session.txt
   git add <any other files the user wants to commit>
   ```
3. **Commit everything together**:
   ```bash
   git commit -m "<user's commit message>"
   ```

The session file should reflect the full conversation up to and including the commit request itself.

---

## Multi-Session Safety

- Filenames are unique by design (timestamp + random ID), so concurrent sessions will never overwrite each other
- No locking or coordination needed between sessions
- If two sessions start in the same second, the random suffix (`XXXX`) prevents collision