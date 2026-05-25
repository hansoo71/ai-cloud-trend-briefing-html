# Agent Editing Guide

- Keep facts grounded in source URLs and RSS/feed timestamps.
- Prefer vendor/original URLs over Google News wrappers.
- Update `src/`, `docs/index.html`, dated `docs/YYYYMMDD/`, and standalone output together.
- Do not commit secrets or tokenized GitHub remotes.
- If image generation creates recognizable company logos without permission, discard and regenerate or use deterministic fallback.
