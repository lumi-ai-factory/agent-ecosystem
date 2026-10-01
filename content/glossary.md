---
title: "Glossary"
nav_order: 99
---

# Glossary

## Introduction

| Term | Definition |
|:-----|:-----------|
| **Open-weight LLM** | A Large Language Model whose trained weights are published, so anyone can download it and run it on their own hardware. |
| **GPU** | Graphics Processing Unit. A processor first built for graphics that can do a huge number of calculations at once, which is exactly what training and running LLMs needs. LUMI's GPUs are made by AMD. |
| **Node** | An individual computer within the LUMI supercomputer. |
| **Inference platform** | A service that runs LLMs on its own hardware and lets you use them over the internet, through a web chat or an API. |
| **LLM** | Large Language Model. An AI model that reads and writes text, such as OpenAI's GPT models, Anthropic's Claude or Google's Gemini. Open-weight LLMs such as Llama, Qwen and Gemma are the kind you can use on Aitta. |
| **API** | Application Programming Interface. A fixed way for programs to talk to each other, for example for a harness to send a prompt to an LLM and get the answer back. |
| **Coding agent** | An LLM paired with a harness, so that it can work on your code with you: it reads files, writes and edits code and can run commands. The LLM decides each step and the harness carries it out. |
| **Harness** | The program side of an agent, such as OpenCode or Claude Code. It sends your requests to the LLM, recognises when the LLM asks for a tool, runs it and passes the result back. The LLM decides, the harness acts. |
| **Open source** | Software whose source code is public, so anyone can read, change and share it. That is why we like it for harnesses: anyone can check exactly what the harness does with your files and data. |
| **MCP** | Model Context Protocol. An open standard that lets AI agents connect to outside tools and data in the same way, whichever harness or tool is involved. |

## Aitta

| Term | Definition |
|:-----|:-----------|
| **Slurm** | The job scheduler on LUMI. It decides whose jobs run on which compute nodes, and when. |
| **API token** | A secret key that tells a service's API who you are, so it knows what you are allowed to use. Aitta's API token also says which LUMI project you are working in. Treat it like a password. |
| **Tool calling** | How an LLM asks the harness to use a tool, such as reading a file or running a command: it replies with a structured request that the harness carries out. Any LLM can be prompted to try, but only LLMs trained for tool calling do it reliably, which is what coding agents need. |
| **Hugging Face** | The main website for sharing AI models. Most open-weight LLMs are published there, each with a model card describing what it can do, such as whether it supports tool calling. |
| **Embedding** | A list of numbers that captures the meaning of a piece of text. Texts that mean similar things get similar numbers, so programs can search and compare text by meaning. |

## OpenCode

| Term | Definition |
|:-----|:-----------|
| **Container** | A packaged software environment that bundles a program with everything it needs to run. On LUMI it also limits which of your files the program inside can see. |
| **Login node** | A LUMI machine you land on when you connect, where you edit files and submit jobs. |
| **Compute node** | A LUMI machine where your submitted Slurm jobs do the heavy computation. |
| **Working directory** | The folder your terminal is currently in. |

## MCP server

| Term | Definition |
|:-----|:-----------|
| **RAG** | Retrieval-augmented generation. Instead of answering only from what it learned in training, the LLM is first given relevant passages found in a set of documents, and answers from those. |


