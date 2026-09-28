---
title: "MCP server"
nav_order: 4
---

# The LUMI MCP server

An LLM% only knows what it learned during training, and that rarely includes up-to-date details about LUMI. Ask a coding agent% how to use PyTorch on LUMI and it may confidently suggest installing it with `pip install torch`, something that works on a laptop but gives you a PyTorch that cannot use LUMI's AMD GPUs% and bloats the system.

The LUMI AI Factory runs a public MCP% server that fills this gap. It lets your agent look things up in the LUMI documentation and check LUMI's current status, so it can answer questions about LUMI more accurately and write code suited to the system. It works with any harness% or app that supports MCP, such as OpenCode, Claude Code, Codex or VS Code, whichever LLM it uses. You do not need an account or an API token% to use it.

But what is this MCP? MCP, the Model Context Protocol, is a shared standard for adding tools to an agent. An MCP server describes the tools it offers, the harness passes those descriptions on to the LLM, and from then on the LLM can call them just like the harness's built-in tools (the [OpenCode page](/02_opencode#how-an-agent-works) explains how tool calling works). Every MCP-capable harness speaks the same protocol, so one server works with all of them.

## What it can do

The server gives your agent two tools:

| Tool | What it does | Helps with questions like |
|:-----|:-------------|:--------------------------|
| `retrieve_docs` | Searches a regularly updated knowledge base of the [LUMI documentation](https://docs.lumi-supercomputer.eu/) and the [LUMI AI Guide](https://github.com/Lumi-supercomputer/LUMI-AI-Guide), and returns the most relevant passages with links to their sources | "How do I run PyTorch on LUMI?" |
| `get_service_status` | Reports LUMI's current status, planned maintenance and ongoing incidents | "Why is my job not starting? Is something down?" |

![OpenCode, asked "how's LUMI doing?", calls lumi-aif_get_service_status and sums up the result: all compute partitions and login nodes are up, and only the LUMI-K web console is in maintenance](assets/opencode-query-MCP.png)

Your agent decides when to use them. If it answers a LUMI question without checking, ask it to, for example "search the LUMI documentation for how to set up a PyTorch environment".

## See what your agent receives

You can look at exactly what the tools hand back to the agent.

`get_service_status` passes on the LUMI status API unchanged, so you can open the same data in your browser:

- [Current status](https://status.lumi.csc.fi/api/status), including node availability and response times
- [Planned maintenance](https://status.lumi.csc.fi/api/maintenance)
- [Incidents](https://status.lumi.csc.fi/api/incidents)

The same information in a human-readable form is on the [LUMI status page](https://status.lumi.csc.fi).

`retrieve_docs` cannot be opened in a browser, but you can call it from your terminal. Change the text after `"query"` to your own search, and `"k"` to the number of passages you want back:

```bash
curl -s https://lumi-aif-agents.2.rahtiapp.fi/mcp \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"retrieve_docs","arguments":{"query":"agent infrastructure","k":1}}}'
```

Each passage comes back with a link to its source and a score showing how closely it matches your query.

## Add it to your harness

### OpenCode

On LUMI, the OpenCode container is already connected to the MCP server, so there is nothing to do. On your own machine, the `opencode.json` on the [OpenCode page](/02_opencode#opencode-on-your-own-machine) already includes it.

### Claude Code

Run this once to make the server available in all your projects:

```bash
claude mcp add --transport http --scope user lumi-aif https://lumi-aif-agents.2.rahtiapp.fi/mcp
```

See the [Claude Code MCP documentation](https://code.claude.com/docs/en/mcp) for other options.

### Codex

Add these lines to `~/.codex/config.toml`:

```toml title="~/.codex/config.toml"
[mcp_servers.lumi-aif]
url = "https://lumi-aif-agents.2.rahtiapp.fi/mcp"
```

See the [Codex MCP documentation](https://learn.chatgpt.com/docs/extend/mcp?surface=cli) for other options.

### Other harnesses

Most other harnesses and apps, such as VS Code, can connect to a remote MCP server too. Look in their documentation for how to add one, and give it the address `https://lumi-aif-agents.2.rahtiapp.fi/mcp`.

## Knowledge check

```quiz
title: Check your understanding

Q: What do you need to use the LUMI MCP server?
- [ ] An Aitta API token
- [ ] A LUMI account
- [x] Nothing, it is public
> The server is public and needs no account or API token. You only add its address to your harness.

---

Q: Your agent answers a LUMI question without checking the documentation. What can you do?
- [ ] Nothing, the agent decides on its own
- [x] Ask it to search the LUMI documentation
- [ ] Reconnect the MCP server
> The agent decides when to use its tools, but you can always ask it to use one.
```

<details>
<summary>Optional: how the MCP server finds its answers</summary>

**What it knows.** The server only knows what is in its knowledge base: the LUMI documentation and the LUMI AI Guide. It is only as current as the last time the knowledge base was updated, and short, specific queries work best. Queries are capped at 100 characters.

**How it searches.** `retrieve_docs` is a RAG% search. The documentation is split into small passages, and each passage is stored as an embedding%. Your agent's query is turned into an embedding too, so passages are matched by meaning rather than exact words. The closest passages come back with links to their sources: 4 by default, up to 10.

**Who writes the answer.** The server does not write the answer. Your agent's own LLM reads the passages and answers from them. That is why it can point you to the right page of the documentation, and also why a weak LLM can still misread good passages.

For the full picture of how RAG works, see the [RAG chapter of The Pragmatic Guide to LLMs](https://arbruiser.github.io/The-Pragmatic-Guide-to-LLMs/8-rag/).

</details>
