# TalkMate — English Friend

A Telegram bot that acts as your English conversation partner. Talk to it by voice
or text: it keeps the conversation going naturally, and separately flags and explains
the mistakes you make — so practice feels like chatting, not like a grammar test.

**▶ Try it in Telegram: [@talkmate_practice_bot](https://t.me/talkmate_practice_bot)**

> The source code is private — TalkMate is in active development toward a public
> launch. This repository is a showcase of the product and its architecture.
> Happy to walk through the code in an interview.

<!-- Screenshots: add images to screenshots/ and uncomment this block.
<p align="center">
  <img src="screenshots/conversation.png" alt="Conversation with corrections" width="260" />
  <img src="screenshots/missions.png" alt="Missions" width="260" />
  <img src="screenshots/stats.png" alt="Progress stats" width="260" />
</p>
-->

## Features

**Conversation**
- Voice in, voice out: speech-to-text with OpenAI Whisper, natural replies voiced by Cartesia TTS
- Replies adapt to your level (A1 / A2-B1 / B2 / C1+) — vocabulary and sentence length are tuned per level
- 6 personas to talk to: friend, barista, job interviewer, psychologist, travel guide, celebrity
- Male or female voice, beginner or native speaking pace

**Feedback on your English**
- Mistakes are detected and categorized (tense, articles, prepositions, word choice, word order, vocabulary gaps)
- Relaxed or strict correction mode
- Tap a correction for a deeper explanation plus 2–3 ways a native speaker might say it (casual / neutral / formal style)
- Occasional idiom tips and specific praise when you phrase something well

**Practice & progress**
- Missions: real-life scenarios with a goal to achieve (negotiate a coffee discount, handle a tough interview question, ...) — the character pushes back, so you have to actually persuade them
- Stats: messages, corrections, streak, weak areas and most frequent mistakes, plus a one-tap micro-drill generated from one of your real past mistakes
- AI estimate of your English level based on your messages, with the option to apply it
- Personal vocabulary with spaced-repetition review
- Transcript of voice replies with one-tap translation and "add to vocabulary"
- Daily streak reminders (opt-in)

**Interface** in English, Ukrainian, Russian and Spanish — menus and correction explanations follow the chosen language.

## Tech stack

- **TypeScript** on **Node.js 20**
- **[grammY](https://grammy.dev/)** — Telegram Bot API
- **OpenAI** — `gpt-4o-mini` with JSON Schema structured outputs, `whisper-1` for speech-to-text
- **[Cartesia](https://cartesia.ai/)** — text-to-speech, separate voices per persona
- **SQLite** via `better-sqlite3`
- **Vitest** — 250+ unit tests
- **Docker** — runs 24/7 on a VPS, with automated DB backups

## How it works

```
 voice / text message
          |
          v
 +------------------+     +---------------------------+
 |  grammY handler  | --> | Whisper STT (voice only)  |
 +------------------+     +---------------------------+
          |
          v
 +---------------------------------------------------+
 | gpt-4o-mini, structured output (JSON Schema):     |
 |   reply + corrections + tips in one call          |
 +---------------------------------------------------+
          |                          |
          v                          v
 +------------------+     +---------------------------+
 | Cartesia TTS     |     | SQLite: history, mistakes,|
 | -> voice reply   |     | streak, vocabulary        |
 +------------------+     +---------------------------+
```

User settings (level, strictness, persona, style, interface language) shape the
system prompt, so a single model call returns both the conversational reply and
structured correction data.

## AI-assisted development

Built with an AI-first workflow using Claude Code: each larger feature starts as a
written design spec and a step-by-step implementation plan, then is implemented
test-first on a feature branch and merged through a pull request.

## Author

Daniel Dzhyzhula — [GitHub](https://github.com/DanielDzh) · [LinkedIn](https://www.linkedin.com/in/daniel-dzhyzhula-b26817210/)
