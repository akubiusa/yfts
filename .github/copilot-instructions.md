# Copilot Code Review Instructions

Read `AGENTS.md` first; it holds the project overview, conventions and safety rules. Review against it.

## Language

Deliverables (PR text, review comments, code comments, docs) are in English. Localized UI strings such as `message_ja` and the Japanese text in `index.html` are intentionally kept. Runtime messages are English.

## Review priorities

- Untrusted input: flag user-supplied URLs reaching shell commands, file paths, `eval` or string-built paths without validation.
- WebSocket protocol: every step sends `status`, `process_code`, `message` and `message_ja`, and `process_code` stays contiguous with the progress bar `max` in `index.html`.
- Error handling: helpers return `False` or the caught `Exception` instead of raising; exceptions must not escape the handler and kill the WebSocket loop.
- Client cleanup: every early exit deletes its `clients[key]` entry.

## Known non-issues (do not flag)

- Synchronous `requests`/`youtube_dl`/`ffmpeg` calls inside the async handler, the hardcoded `dirpath = "/tmp/yfts"`, and the bare `except:` in the handler.
- Inline CSS/JS in `index.html` with no build step.
- No automated tests or CI.
