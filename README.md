# FresherHub MCP server

Website: https://fresherhub.app

A free, public, read-only [MCP](https://modelcontextprotocol.io) server for **[FresherHub](https://fresherhub.app)**, the job-readiness check for students and freshers in tech.

Ask your AI assistant what a fresher needs for a tech role. It gets FresherHub's published role data: the skills each role needs, the proof that counts for each skill, and a step-by-step readiness checklist.

- **Endpoint:** `https://fresherhub.app/mcp`
- **Transport:** Streamable HTTP (stateless, JSON responses)
- **Auth:** none. The server reads public role data only. It stores and returns no user data.
- **Website:** [fresherhub.app](https://fresherhub.app) · [Role requirements](https://fresherhub.app/roles)

## Tools

| Tool | Input | Returns |
| --- | --- | --- |
| `list_roles` | none | The 11 entry-level tech roles, each with a link to its requirements page. |
| `get_role_skills` | `role` (slug or name) | Each required skill: area, must-have flag, share of the readiness score, and the evidence that proves it. |
| `get_readiness_checklist` | `role` (slug or name) | The readiness stages in order, the skills in each stage, and the actions that build proof. |

Roles: Backend Developer, Frontend Developer, Full-Stack Developer, Mobile App Developer, Data Analyst, Data Scientist, Data Engineer, Machine Learning Engineer, AI Engineer (GenAI), DevOps & Cloud Engineer, QA Automation Engineer (SDET).

Every result links back to the matching page on [fresherhub.app](https://fresherhub.app), where you can run the free readiness check against your resume and GitHub.

## Connect

### Claude Desktop, Cursor, Windsurf and other clients with remote MCP support

```json
{
  "mcpServers": {
    "fresherhub": {
      "type": "http",
      "url": "https://fresherhub.app/mcp"
    }
  }
}
```

The same file is in [`examples/mcp.json`](examples/mcp.json).

### Claude Code

```bash
claude mcp add --transport http fresherhub https://fresherhub.app/mcp
```

### Clients that support only stdio

```bash
npx -y mcp-remote https://fresherhub.app/mcp
```

### Try it with curl

```bash
curl -s https://fresherhub.app/mcp \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"get_role_skills","arguments":{"role":"data-analyst"}}}'
```

## Example prompts

- "What skills does a fresher Data Analyst need, and how do I prove each one?"
- "Give me a job-readiness checklist for a Backend Developer role."
- "Which tech roles does FresherHub cover?"

## Limits

- 60 requests per minute per client IP. Over the limit, the server returns HTTP 429 with `Retry-After: 60`.
- Request bodies are limited to 16 KB. Responses are small (about 2–6 KB).
- Role data is cached for up to one hour.

## About FresherHub

[FresherHub](https://fresherhub.app) scores how job-ready a student or fresher is for one target tech role. It checks your resume, projects and GitHub against the published role requirements. Then it shows your top blockers and the next best action. The check is free.

This repository holds the documentation and the [LobeHub](https://lobehub.com/mcp) manifest (`lhm.plugin.json`). FresherHub hosts and runs the server.

## License

[MIT](LICENSE)
