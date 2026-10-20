---
title: "Glossary"
nav_order: 99
---

# Glossary

## Introduction

| Term | Definition |
|:-----|:-----------|
| **Open-weight LLM** | An LLM whose trained weights are public. Anyone can download and run it locally. |
| **GPU** | Graphics Processing Unit. A processor that performs many calculations at once, essential for running LLMs. LUMI uses AMD GPUs. |
| **Node** | A single computer within the LUMI supercomputer. |
| **Inference platform** | A service that hosts LLMs and lets you interact with them online via chat or API. |
| **LLM** | Large Language Model. An AI model that reads and writes text (e.g., GPT, Claude, Llama, Qwen). |
| **API** | Application Programming Interface. A standard way for programs to communicate, such as a harness sending a prompt to an LLM. |
| **Coding agent** | An LLM and a harness working together. The LLM plans the steps, and the harness executes them (editing code, running commands). |
| **Harness** | The software side of an agent (like OpenCode). It sends prompts, runs tools, and reports back to the LLM. |
| **Open source** | Software with public code that anyone can inspect, modify, and share. |
| **MCP** | Model Context Protocol. A standard for securely connecting agents to external tools and data sources. |

## Aitta

| Term | Definition |
|:-----|:-----------|
| **Slurm** | LUMI's job scheduler. It manages who runs tasks on compute nodes, and when. |
| **API token** | A secret key identifying you to a service. Aitta's token also links to your LUMI project. Protect it like a password. |
| **Tool calling** | How an LLM asks the harness to use a tool, responding with a formatted request. Coding agents rely on LLMs trained to do this well. |
| **Hugging Face** | A hub for sharing AI models and viewing their capabilities (model cards). |
| **Embedding** | A numerical representation of a text's meaning, used for semantic search. |

## OpenCode

| Term | Definition |
|:-----|:-----------|
| **Container** | A software package containing everything a program needs to run. On LUMI, it also limits what files the program can access. |
| **Login node** | The machine you connect to on LUMI for editing files and submitting jobs. |
| **Compute node** | A LUMI machine dedicated to running heavy Slurm jobs. |
| **Working directory** | The folder your terminal is currently operating in. |

## MCP server

| Term | Definition |
|:-----|:-----------|
| **RAG** | Retrieval-Augmented Generation. Supplying relevant documents to an LLM so it answers based on current facts rather than just training data. |
