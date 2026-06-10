# Jobs & polling — `check_generation`

Every creative tool returns a **job**, not a finished asset. You poll `check_generation` until the job reaches a terminal status, then deliver the result. **Do this for the user** — never ask them to check manually.

## Call

```json
{ "job_id": "<id from the creative tool's result>" }
```

## Cadence (don't spam)

Generations usually take **1–3+ minutes** (video longer).

- **First check** ~10–15s after you get the job (give the worker time to start).
- Then ~**30s** between checks while status is `queued` / `processing` / `pending`.
- Stretch to **45–60s** for clearly long video jobs.
- Tell the user **once** that you're waiting — don't post "checking again…" every few seconds.

## Terminal states

Stop polling when status is:

- `completed` → deliver the result URL (or, for Virality Analysis, the summary).
- `failed` → report the failure reason plainly; offer to retry or adjust.
- `cancelled` → acknowledge and ask whether to restart.

## Delivering

- Generated media → the URL + a one-line summary (e.g. "3 lifestyle shots" / "10s product video, 9:16"). Embed images/video if the client supports it.
- Virality Analysis → the score summary in plain language. Never fabricate numbers; use the completed job's result only.
