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

## Editing the site

All content lives in the `content/` folder as Markdown. Commit a change to `main` and GitHub Actions rebuilds and redeploys the site within a minute or two. There is nothing to build locally.

A few things to know before you edit:

- **Page order.** Every page starts with front matter that sets its title and position in the sidebar:

  ```markdown
  ---
  title: "Page Title"
  nav_order: 2
  ---
  ```

- **Site title.** The top `#` heading of `content/index.md` becomes the site title.
- **Glossary terms.** Writing `LLM%` shows the glossary definition when a reader hovers over the term. Mark only the first appearance of a term on each page, never in the sentence that defines it, and add any new term to the table in `content/glossary.md`, otherwise the marker does nothing.
- **Callouts, quizzes, collapsible sections and Mermaid diagrams** are all available. Examples of each are in the [course template](https://github.com/lumi-ai-factory/course-template).
- **Style.** Use British spelling and plain language. Link to existing LUMI documentation instead of copying it.
- **Images and downloads** go in `public/assets/`.

The full writing guide for this repository, including what each page must and must not say, is in [CLAUDE.md](CLAUDE.md).

## Getting template updates

The site is built on the LUMI AI Factory [course template](https://github.com/lumi-ai-factory/course-template). To pull in its latest styling fixes and features without touching our content, register the template once:

```bash
git remote add template https://github.com/lumi-ai-factory/course-template.git
```

Then, whenever you want the latest version (commit and push your own work first):

```bash
git fetch template
git checkout template/main -- . ":(exclude)content" ":(exclude)public" ":(exclude)README.md"
git commit -m "Pull in template updates"
git push
```

Check `git status` before committing. Any local edits to the template's internals (`src/`, the build config, the deploy workflow) are overwritten and need to be re-applied.

## Licence

The content and documentation are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), and the code under the MIT License. See [LICENSE](LICENSE) for details.
