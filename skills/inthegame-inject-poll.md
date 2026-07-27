---
name: Create and inject a live poll
description: Admin flow to authenticate, create a poll, inject it into a live streamer channel, and publish the winning result.
api: openapi/inthegame-openapi.yml
operations: [adminapiUserLogin, adminapiPollCreate, adminapiStreamerInjectpoll, adminapiPollSendwinresult]
---

# Create and inject a live poll

Use this to run an interactive poll on a live Inthegame streamer channel.

## Auth
Admin API calls send the admin token in the `Authorization` header (issued after login to the admin panel `https://admintest.inthegame.io/`). See `authentication/inthegame-authentication.yml`.

## Steps
1. **Log in** — `adminapiUserLogin` (`GET /adminApi/user/loginNew`) with username/password to obtain the admin token; set it as the `Authorization` header on subsequent calls.
2. **Create the poll** — `adminapiPollCreate` (`POST /adminApi/poll/create`) with the poll question and options (form-encoded body). Capture the returned `pollId`.
3. **Inject into the channel** — `adminapiStreamerInjectpoll` (`POST /adminApi/streamer/injectPoll/{channelId}`) to push the poll onto the live overlay. Viewers receive the `injectPoll` Socket.IO event (see `asyncapi/inthegame-socket-asyncapi.yml`).
4. **Publish the result** — `adminapiPollSendwinresult` (`POST /adminApi/poll/result/{pollId}`) to close the poll and broadcast the winning option.

## Rules
- No idempotency key is supported (`conventions/inthegame-conventions.yml`); avoid duplicate injects.
- Errors are plain HTTP status codes: 404 (bad channelId/pollId) or 500 — see `errors/inthegame-problem-types.yml`.
