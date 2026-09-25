---
title: "Glossary"
nav_order: 99
---

# Glossary

A quick reference for all the terms used in this template, grouped by the page where they first appear. Every term in a **Term** column can be referenced from any page by putting a single **percent sign** directly after the word (e.g. `Markdown%`). The reader sees a dashed underline and gets the definition in a small pop-up on hover.

> [!tip] How to add or reference a term
> Add a new row to any table below and keep the two columns **Term** and **Definition**. To link a term in your text, type it and add a percent sign right after it, with no space: `Front Matter%`. Matching is **case-insensitive** and plural forms work too (`Front Matters%`).

| Term | Definition |
|:-----|:-----------|
| **Markdown** | A lightweight plain-text formatting syntax used to write all the content in this template. |
| **Callout** | A coloured box used to highlight a note, warning, info side-note, tip, or copyable command for your students. |
| **Front Matter** | The block of metadata at the very top of a page, between the `---` lines, that sets the page title and sidebar order. |

## Home

| Term | Definition |
|:-----|:-----------|
| **Open-weight LLM** | A Large Language Model whose trained weights are published, so anyone can download it and run it on their own hardware. |
| **Inference platform** | A service that runs LLMs on its own hardware and lets you use them over the internet, through a web chat or an API. |
| **LLM** | Large Language Model. An AI model trained on huge amounts of text to understand and write language, like the models behind chat assistants. |
| **API** | Application Programming Interface. A fixed way for programs to talk to each other, for example for an agent to send a prompt to an LLM and get the answer back. |
| **Coding agent** | An AI assistant that works on your code with you: it reads files, writes and edits code and can run commands, using an LLM to decide each step. |
| **Open source** | Software whose source code is public, so anyone can read, change and share it. That is why we like it for agents: anyone can check exactly what the agent does with your files, and it does not tie you to one company's products. |
| **MCP** | Model Context Protocol. An open standard that lets AI agents connect to outside tools and data in the same way, whichever agent or tool is involved. |

## Aitta

| Term | Definition |
|:-----|:-----------|
| **LUMI-G** | The GPU partition of LUMI: the nodes fitted with AMD MI250X GPUs, where AI models are trained and run. |
| **Slurm** | The job scheduler on LUMI. It decides whose jobs run on which compute nodes, and when. |
| **API token** | A secret key that tells Aitta's API who you are and which LUMI project you are working in. Treat it like a password. |
| **Tool calling** | How an LLM asks an agent to use a tool, such as reading a file or running a command: it replies with a structured request that the agent carries out. Any LLM can be prompted to try, but only LLMs trained for tool calling do it reliably, which is what coding agents need. |
| **Embedding** | A list of numbers that captures the meaning of a piece of text. Texts that mean similar things get similar numbers, so programs can search and compare text by meaning. |

## OpenCode

| Term | Definition |
|:-----|:-----------|
| **Container** | A packaged software environment that bundles a program with everything it needs to run. On LUMI it also limits which of your files the program inside can see. |
| **Login node** | The shared machine you land on when you connect to LUMI. It is meant for light work such as editing files and submitting jobs, not heavy computation. |
| **Compute node** | One of the many LUMI machines where the heavy work runs. You get to use them by submitting a job through Slurm. |
| **Working directory** | The folder your terminal is currently in. A program you start there looks for files in it by default. |

## MCP server

| Term | Definition |
|:-----|:-----------|
| **RAG** | Retrieval-augmented generation. Instead of answering only from what it learned in training, the LLM is first given relevant passages found in a set of documents, and answers from those. |


