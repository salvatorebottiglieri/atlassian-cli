# tempo-api

Minimal Tempo Timesheets v4 CLI. No dependencies — pure Python standard library.

## Credentials

File: `~/.config/tempo/credentials.json`

```json
{
  "jira_base_url": "https://your-org.atlassian.net",
  "email": "you@example.com",
  "jira_token": "<Atlassian API token>",
  "tempo_token": "<Tempo API token>"
}
```

**Both tokens are required:**

- **Jira token**: generate at https://id.atlassian.com/manage-profile/security/api-tokens  
  Used to resolve issue keys (e.g. `PROJ-123`) to their numeric ids, since Tempo v4 only accepts numeric issue ids.
- **Tempo token**: generate inside Jira → Tempo sidebar → Settings → API Integration → New Token  
  Used to authenticate all calls to `api.tempo.io`.

> Note: this tool points to a **separate Jira instance** from `jira-api`. Each tool has its own credentials file.

## Commands

### `log` — Log time on an issue

```bash
tempo-api log PROJ-123 --time "2h 30m"
tempo-api log PROJ-123 --time "1h" --date 2026-07-18 --comment "code review session"
```

- `--time` accepts `Xh`, `Xm`, or `Xh Ym` (e.g. `2h`, `45m`, `1h 30m`)
- `--date` defaults to today if omitted (ISO 8601, e.g. `2026-07-20`)
- `--comment` sets the worklog description in Tempo

Each `log` command makes two API calls: one to Jira to resolve the issue key to its numeric id, then one to Tempo to post the worklog.

---

### `worklogs` — List worklogs for an issue

```bash
tempo-api worklogs PROJ-123
tempo-api worklogs PROJ-123 --from 2026-07-01 --to 2026-07-31
tempo-api worklogs PROJ-123 --json
```

Displays date, time spent, issue key, and description. Prints a total at the end.

---

### `my-worklogs` — List your own worklogs

```bash
tempo-api my-worklogs
tempo-api my-worklogs --from 2026-07-01 --to 2026-07-31
tempo-api my-worklogs --json
```

Defaults to the last 7 days if no date range is provided.

---

### `delete` — Delete a worklog

```bash
tempo-api delete 98765
```

The worklog id is printed by `log` on creation and visible in `--json` output as `tempoWorklogId`.

## API notes

- Base URL: `https://api.tempo.io/4`
- Rate limit: 5 requests/second
- Tempo v4 requires numeric `issueId` — issue key resolution is handled automatically
