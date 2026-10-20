---
title: "MCP server"
nav_order: 4
---

# The LUMI MCP server

An LLM% only knows its training data, which rarely covers current LUMI details. If you ask a coding agent% to install PyTorch on LUMI, it might suggest `pip install torch`. That works on a laptop but installs a version that cannot use LUMI's AMD GPUs% and bloats your environment.

The LUMI AI Factory runs a public MCP% server to solve this. It gives your agent tools to search the LUMI documentation and check system status. The agent can then answer LUMI questions accurately and write better code. It works with any MCP-compatible harness%, like OpenCode or Claude Code. You do not need an account or an API token%.

What is MCP? The Model Context Protocol is a standard way to add tools to an agent. The server describes its tools to the harness, which passes them to the LLM. The LLM can then use them like built-in tools.

## What it can do

The LUMI MCP server provides two tools:

| Tool | What it does | Helps with questions like |
|:-----|:-------------|:--------------------------|
| `retrieve_docs` | Searches the [LUMI documentation](https://docs.lumi-supercomputer.eu/) and [LUMI AI Guide](https://github.com/Lumi-supercomputer/LUMI-AI-Guide), returning relevant passages and source links. | "How do I run PyTorch on LUMI?" |
| `get_service_status` | Reports LUMI's status, planned maintenance, and active incidents. | "Why is my job not starting?" |

![OpenCode, asked "how's LUMI doing?", calls lumi-aif_get_service_status and sums up the result: all compute partitions and login nodes are up, and only the LUMI-K web console is in maintenance](assets/opencode-query-MCP.png)

Your agent decides when to use these tools and how to present the information.

## See what your agent receives

You can inspect the exact data the server sends back.

The `get_service_status` tool passes data straight from the LUMI status API, so you can view it in your browser:

- [Current status](https://status.lumi.csc.fi/api/status), including node% availability
- [Planned maintenance](https://status.lumi.csc.fi/api/maintenance)
- [Incidents](https://status.lumi.csc.fi/api/incidents)

A human-readable version is on the [LUMI status page](https://status.lumi.csc.fi).

You cannot open `retrieve_docs` in a browser, but you can call it from your terminal. Change `"query"` to your search and `"k"` to the number of results:

```bash
curl -s https://lumi-aif-agents.2.rahtiapp.fi/mcp \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"retrieve_docs","arguments":{"query":"agent infrastructure","k":1}}}'
```

Each result includes a link to the source and a score indicating how well it matched.

## Add it to your harness

### OpenCode

On LUMI, the OpenCode container connects to the server automatically. On your machine, our `opencode.json` from the [previous chapter](/02_opencode#opencode-on-your-own-machine) already includes it.

### Claude Code

Run this command once to enable the server for all projects:

```bash
claude mcp add --transport http --scope user lumi-aif https://lumi-aif-agents.2.rahtiapp.fi/mcp
```

Check the [Claude Code MCP docs](https://code.claude.com/docs/en/mcp) for more options.

### Codex

Add this to `~/.codex/config.toml`:

```toml title="~/.codex/config.toml"
[mcp_servers.lumi-aif]
url = "https://lumi-aif-agents.2.rahtiapp.fi/mcp"
```

Check the [Codex MCP docs](https://learn.chatgpt.com/docs/extend/mcp?surface=cli) for more details.

### Other harnesses

Most harnesses, including VS Code, support remote MCP servers. Check their documentation and use the address `https://lumi-aif-agents.2.rahtiapp.fi/mcp`.

## Knowledge check

```quiz
title: Check your understanding

Q: What do you need to use the LUMI MCP server?
- [ ] An Aitta API token
- [ ] A LUMI account
- [x] Nothing, it is public
> The server is public and needs no account or API token. You only add its address to your harness.

---

Q: Who decides when the LUMI MCP server's tools are used?
- [ ] You, by turning them on for each question
- [ ] The MCP server
- [x] Your agent
> Your agent decides when to use the tools and how to incorporate their output into its response to you.
```

<details>
<summary>Optional: how the MCP server finds its answers</summary>

**What it knows.** The server searches its knowledge base: the LUMI documentation and AI Guide. It is only as current as its last update. Short, specific queries work best, up to 100 characters.

**How it searches.** `retrieve_docs` uses RAG% search. The text is stored as an embedding%. The agent's query becomes an embedding, matching passages by meaning rather than exact words. It returns the top 4 matches by default.

**Who writes the answer.** The server does not write answers. The LLM reads the passages and forms a response. This allows it to link to documentation, but a weak LLM can still misinterpret correct passages.

Read the [Pragmatic Guide to LLMs](https://arbruiser.github.io/The-Pragmatic-Guide-to-LLMs/8-rag/) for more on RAG.

</details>
