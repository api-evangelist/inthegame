---
name: Run a live trivia round
description: Admin creates trivia and injects it; viewers submit answers.
api: openapi/inthegame-openapi.yml
operations: [adminapiTriviaCreate, adminapiStreamerInjecttrivia, userapiTriviaAnswer]
---

# Run a live trivia round

## Auth
Admin steps use the `Authorization` header token; the viewer step uses `userToken` in the body.

## Steps
1. **Create trivia** — `adminapiTriviaCreate` (`POST /adminApi/trivia/create`) with the question set; capture the `triviaId`.
2. **Inject into the channel** — `adminapiStreamerInjecttrivia` (`POST /adminApi/streamer/injectTrivia/{channelId}`). Viewers get the `injectTrivia` Socket.IO event.
3. **Viewer answers** — `userapiTriviaAnswer` (`POST /userApi/trivia/answer/{questionId}`) with the selected option and `userToken`.

## Rules
- Plain HTTP status errors (404/500); no idempotency key. See `conventions/` and `errors/`.
