---
name: Viewer answers a poll and checks the leaderboard
description: End-viewer flow to authenticate, submit a poll answer, and read the channel leaderboard.
api: openapi/inthegame-openapi.yml
operations: [userapiUserLoginbycredentials, userapiPollAnswer, userapiLeaderboardGet]
---

# Viewer answers a poll and checks the leaderboard

Use this for the end-viewer play loop on an Inthegame channel.

## Auth
User API calls identify the viewer with a `userToken` in the request body (obtained at login). See `conventions/inthegame-conventions.yml`.

## Steps
1. **Log in** — `userapiUserLoginbycredentials` (`POST /userApi/user/login_by_credentials`) to obtain the `userToken` (or `userapiUserLoginbytoken` / `POST /userApi/user/login_by_token` for token re-auth).
2. **Answer the poll** — `userapiPollAnswer` (`POST /userApi/poll/answer_new/{questionId}`) with the chosen option and `userToken`.
3. **Read the leaderboard** — `userapiLeaderboardGet` (`GET /userApi/leaderboard/get/{broadcaster}`) to show the viewer's rank and points. Live updates also arrive via the `leaderboard` Socket.IO event.

## Rules
- One answer per question per viewer; resubmission is not idempotent.
- 404 means a bad questionId/broadcaster — see `errors/inthegame-problem-types.yml`.
