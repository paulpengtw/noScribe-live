# noScribe Live: notes for agents

The live/ web viewer never starts transcriptions: during demos the owner runs the noScribe CLI with their usual flags, and the viewer only watches that run (via an opt-in feed from the CLI) and streams its segments to the browser at localhost. Keep the CLI's normal behaviour unchanged when the feed is off, and don't copy AICSS Pro components; build the aicss.dev look from free components or original code.

The 「緋紅」/"Crimson" theme uses a client's corporate colours (red Pantone 7622 C ≈ #93272C, navy 2955 C ≈ #003865, cool grays, cream, sparing gold). Never name that client anywhere (UI, docs, commits, PR text), and never use their logo, mascot or brick auxiliary pattern: colours only.

UI component licences: the owner approved animate-ui (MIT + Commons Clause) for this project, because it is consulting work and the product is not sold (2026-10-22). The free tiers of aicss.dev, affectui.com, obsidianui.dev and uiarc.dev are MIT and may be used. Record each component's licence in live/README.md's licensing section before copying it.

## AI correction (live/web llm/)

The code is in `live/web/src/lib/llm/`. The owner runs AI correction with their own Anthropic API key through Anthropic's OpenAI-compatible endpoint (base URL `https://api.anthropic.com/v1`, `Authorization: Bearer <key>`, model `claude-opus-5-5`), called directly from the browser.

- That endpoint allows browser calls only when the request carries the header `anthropic-dangerous-direct-browser-access: true`. A preflight without it returns 400; with it, the reply has `access-control-allow-origin: *`.
- On `claude-opus-5-5` the endpoint rejects a temperature below 1 and any `top_p`, so leave both out.
- It ignores `response_format`, so parse the JSON out of fenced or prose-wrapped replies, and keep the one retry on a bad reply.
- The mock (`live/stress/mock_llm.py --anthropic`) should enforce the same rules, so the Anthropic path is tested for free.
