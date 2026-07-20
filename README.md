# atlassian-cli

Minimal, zero-dependency Python CLIs for Jira and Tempo Timesheets.

| Tool | API | Docs |
|---|---|---|
| `jira-api` | Jira REST v3 | [docs/jira-api.md](docs/jira-api.md) |
| `tempo-api` | Tempo Timesheets v4 | [docs/tempo-api.md](docs/tempo-api.md) |

Both tools are single-file Python 3.10+ scripts with no external dependencies — only the standard library.

## Installation

```bash
cp bin/jira-api bin/tempo-api ~/.local/bin/
chmod +x ~/.local/bin/jira-api ~/.local/bin/tempo-api
```

Make sure `~/.local/bin` is in your `PATH`.

## Credentials

Each tool reads from its own JSON file. Copy the examples and fill in your tokens:

```bash
# jira-api
mkdir -p ~/.config/jira
cp credentials/jira.example.json ~/.config/jira/credentials.json

# tempo-api
mkdir -p ~/.config/tempo
cp credentials/tempo.example.json ~/.config/tempo/credentials.json
```

See [credentials/jira.example.json](credentials/jira.example.json) and [credentials/tempo.example.json](credentials/tempo.example.json) for the expected format.

- **Jira token**: generate at https://id.atlassian.com/manage-profile/security/api-tokens
- **Tempo token**: generate inside Jira → Tempo sidebar → Settings → API Integration → New Token
