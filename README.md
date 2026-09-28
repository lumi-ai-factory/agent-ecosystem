# LUMI Agent Ecosystem

![LUMI AI Factory logo](public/assets/LAIF_logo_dark_white_background.jpg)

This repository holds the source of the **LUMI Agent Ecosystem** site, a short guide by the LUMI AI Factory to using AI coding agents on LUMI.

**Read the site:** https://lumi-ai-factory.github.io/agent-ecosystem/

## What the site covers

The LUMI AI Factory offers three pieces that work together, and each one also works on its own:

- **Aitta**, an inference platform that serves open-weight LLMs on LUMI's GPU nodes through a web chat and an OpenAI-compatible API, so your prompts are processed on LUMI's own hardware.
- **OpenCode**, an open-source harness (the program half of a coding agent) that comes ready to use on LUMI in a container and can also be installed on your own machine.
- **The LUMI MCP server**, a public server that lets any MCP-capable harness search the LUMI documentation and check LUMI's service status.

The site explains why each piece is useful, gives the configuration you need to get going, and links to the official [LUMI documentation](https://docs.lumi-supercomputer.eu/) for the step-by-step instructions rather than repeating them. It is written for newcomers to AI agents as well as for experienced users who only want the config files and commands.

| Page | Source file | What it is about |
|:-----|:------------|:-----------------|
| Introduction | `content/index.md` | Overview of the three pieces and where to start |
| Aitta | `content/01_aitta.md` | What an inference platform is, the models, data confidentiality, fair use and getting an API token |
| OpenCode | `content/02_opencode.md` | How an agent and tool calling work, what not to do with one, OpenCode on LUMI and on your own machine, a ready-made `opencode.json` |
| MCP server | `content/03_mcp_server.md` | What MCP is, the two tools, how to see their raw output and how to add it to other harnesses |
| Glossary | `content/glossary.md` | Short definitions of the technical terms used across the site |


## Licence

The content and documentation are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), and the code under the MIT License. See [LICENSE](LICENSE) for details.
