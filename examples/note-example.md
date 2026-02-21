---
task_id: a1b2c3d4-e5f6-7890-abcd-ef1234567890
---

Discussed deployment strategy with the team.
Decided to go with a blue-green deployment approach.

## Key decisions

- Use rolling updates with health checks
- Keep the previous version running for 30 minutes as fallback
- Monitor error rates before cutting over fully
