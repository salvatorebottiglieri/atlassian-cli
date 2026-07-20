# jira-api

Minimal Jira REST API v3 CLI. No dependencies — pure Python standard library.

## Credentials

File: `~/.config/jira/credentials.json`

```json
{
  "base_url": "https://your-org.atlassian.net",
  "email": "you@example.com",
  "token": "<Atlassian API token>"
}
```

Generate your token at https://id.atlassian.com/manage-profile/security/api-tokens

## Commands

### `get` — Fetch an issue

```bash
jira-api get PROJ-123
```

Returns the full issue payload as JSON.

---

### `create` — Create an issue

```bash
jira-api create \
  --project PROJ \
  --summary "Fix the login bug" \
  [--type Story] \
  [--description "Plain text description"] \
  [--labels bug,needs-triage] \
  [--parent PROJ-10] \
  [--fix-version 1.0] \
  [--assignee me] \
  [--story-points 3] \
  [--sp-without-ai 5] \
  [--actual-sp 2] \
  [--team MyTeam]
```

- `--assignee` accepts an email, an accountId, or `me` (token owner)
- `--story-points` sets the estimation field (planning); `--actual-sp` is for post-completion
- `--sp-without-ai` is required by some workflow gates before starting development
- `--team` accepts a team name (mapped to UUID internally) or a raw UUID

---

### `edit` — Update an existing issue

```bash
jira-api edit PROJ-123 \
  [--summary "New summary"] \
  [--labels ready-for-dev] \
  [--assignee me] \
  [--story-points 5] \
  [--sp-without-ai 8] \
  [--actual-sp 3] \
  [--team MyTeam] \
  [--parent PROJ-10] \
  [--description "Updated description"]
```

---

### `labels` — List an issue's labels

```bash
jira-api labels PROJ-123
jira-api labels PROJ-123 --json
```

---

### `transition` — Move an issue through its workflow

List available transitions from the current status:
```bash
jira-api transition PROJ-123
```

Apply a transition by name or destination status:
```bash
jira-api transition PROJ-123 --to "In Progress"
jira-api transition PROJ-123 --to Done --sp-without-ai 3
```

Apply by transition id:
```bash
jira-api transition PROJ-123 --id 31
```

`--sp-without-ai` is set via a separate edit call before the transition fires, since some workflow validators require the field to be populated first.

---

### `search` — Search by JQL

```bash
jira-api search --jql "project = PROJ AND status != Done AND assignee = currentUser()"
jira-api search --jql "project = PROJ AND labels = needs-triage" --max 100
```

---

### `plan` — Read an Advanced Roadmaps plan

```bash
jira-api plan --id 124
jira-api plan --id 124 --assignee me
jira-api plan --id 124 --include-done --backlog
jira-api plan --id 124 --json
```

Uses the internal `jpo` API endpoint (the public Plans API requires admin permissions).

---

### `link` — Create an issue link

```bash
jira-api link --type Blocks --outward PROJ-10 --inward PROJ-20
jira-api link --type "Relates" --outward PROJ-10 --inward PROJ-20
```

---

### `log-work` — Log time on an issue (native Jira worklog)

```bash
jira-api log-work PROJ-123 --time "2h 30m"
jira-api log-work PROJ-123 --time "1h" --comment "fixed the regression"
```

> For logging time to **Tempo Timesheets**, use [`tempo-api log`](tempo-api.md) instead.

---

### `comment` — Add a comment

```bash
jira-api comment PROJ-123 --body "Investigated the issue — root cause is in the auth middleware."
```
