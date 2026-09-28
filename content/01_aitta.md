---
title: "Aitta"
nav_order: 2
---

# Aitta

Aitta is an inference platform: a service that runs LLMs% on its own GPUs and lets you use them over the internet, so you don't have to book GPUs and serve an LLM yourself with tools such as vLLM. It is developed by the LUMI AI Factory at CSC, which also hosts LUMI, and runs open-weight LLMs% directly on LUMI-G% nodes. These LLMs serve as the "brain" for your coding agent%.

You can use Aitta in two ways:

- **In your browser**: The [Aitta web frontend](https://aitta.csc.fi) offers a straightforward chat interface where you can log in, pick an LLM, and start prompting immediately without any setup.
- **Through its API%**: This is how harnesses% and your Python scripts connect to it. The API is OpenAI-compatible, meaning it accepts the same requests as OpenAI's own API. Most tools built for OpenAI will work with Aitta once you point them to Aitta's address and provide your API token%.

## Which LLMs to choose

Aitta provides a variety of generative LLMs that can read and write text, with some also supporting image inputs. For coding agents, you need an LLM that was trained for tool calling% (the [OpenCode page](/02_opencode#how-an-agent-works) explains what that means).

To check if a specific LLM is suited for this, you can look it up on [Hugging Face](https://huggingface.co), where Aitta's LLMs always go by the same names, and see whether its model card mentions tool calling capabilities. The [OpenCode page](/02_opencode) also lists the LLMs on Aitta that support tool calling.

## Where your data goes

When a coding agent does its job, it reads your local files, terminal outputs, and system errors, sending all of this context to the LLM.

On personal plans, companies like OpenAI and Anthropic may keep your conversations to train future LLMs, which can involve human reviewers reading them. That includes your sessions in harnesses such as Claude Code or Codex on those plans, along with every file the agent reads.

When your agent uses Aitta, your prompts and files are processed on LUMI's own hardware rather than being sent to a commercial company.

However, this does not mean it is fine to send absolutely any data: Aitta does not allow sensitive data. Before you point an agent at a project, read the [Aitta terms of use](https://aitta.csc.fi/terms-of-use).

## Resources and limitations

Aitta has two dedicated LUMI-G nodes (reserved specifically for Aitta, so it doesn't queue for them), shared by everyone who uses it. However, when demand is high and the dedicated nodes fill up, Aitta will automatically book additional GPU nodes from the rest of the LUMI cluster through Slurm%, like any other job.

- **Starting an LLM takes time.** If you request an LLM that is not currently running, it takes a few minutes for the LLM weights to be loaded into VRAM (GPU memory). During this time, your agent may seem stuck or time out.
- **Queueing for extra nodes.** Booking additional GPU nodes from the rest of the LUMI cluster may involve some additional queueing for resources.
- **No guarantees.** Aitta cannot guarantee that a given LLM is available at a given time, so it suits research and development, not a service that other people depend on.

You can check which LLMs are currently running by looking at the [Aitta web frontend](https://aitta.csc.fi) or by running this command in your terminal (replace `<YOUR_TOKEN>` with your actual API token). LLMs marked `"status": "running"` are ready to answer straight away, while `"requested"` or `"starting"` means the LLM is still waiting for GPUs or loading:

```bash
curl -H "Authorization: Bearer <YOUR_TOKEN>" https://aitta-api.csc.fi/worker
```

## Getting started

### Prerequisites

- You need an active LUMI project.
- You need a place to run your harness, such as your own laptop or a LUMI login node. (The [OpenCode page](/02_opencode) explains how to set this up).

### Three steps to connect

1. **Log in** to the [Aitta web frontend](https://aitta.csc.fi).
2. **Generate an API token** using the "Generate token" link in the interface, or go straight to [aitta-auth.csc.fi/myToken](https://aitta-auth.csc.fi/myToken). The API token stays valid for 90 days, or until your LUMI project ends if that comes first, so you only need to give it to your harness again when it expires.
3. **Connect your harness** by giving it Aitta's base address, `https://aitta-api.csc.fi/openai/v1`, and your API token.

For a complete walkthrough of the interface and detailed instructions, refer to the official [Aitta step-by-step guide](https://docs.lumi-supercomputer.eu/laif/inference/aitta/).

<details>
<summary>Optional: using embedding models</summary>

While coding agents rely on generative LLMs, Aitta also hosts embedding% models for tasks like semantic search and document comparison.

To see which embedding models are available, list all the models with your API token (replace `<YOUR_TOKEN>`) and look for them by name, such as `intfloat/multilingual-e5-large`:

```bash
curl -H "Authorization: Bearer <YOUR_TOKEN>" https://aitta-api.csc.fi/openai/v1/models
```

Send your texts to the `/embeddings` endpoint, which works like OpenAI's embeddings API:

```bash
curl https://aitta-api.csc.fi/openai/v1/embeddings \
  -H "Authorization: Bearer <YOUR_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{"model": "intfloat/multilingual-e5-large", "input": ["query: How do I run PyTorch on LUMI?"]}'
```

Like LLMs, an embedding model that is not running needs a few minutes to load before it answers (see [Resources and limitations](#resources-and-limitations)).

What comes back is the embedding itself: a long list of numbers for each text.

</details>

## Knowledge check

```quiz
title: Check your understanding

Q: Your agent has been waiting several minutes for its first answer. What is the most likely reason?
- [ ] Aitta has lost your request
- [x] The LLM was not running, so Aitta is loading its weights into VRAM
- [ ] Your API token has expired
> An LLM that is not running has to be loaded into GPU memory first, which takes minutes. An expired API token results in an immediate error, not a long wait.

---

Q: Your prompts are processed on LUMI when you use Aitta. Does that mean you can send it any data?
- [ ] Yes, nothing leaves LUMI, so anything goes
- [x] No, the Aitta terms of use dictate what data is allowed
> Your prompts stay on LUMI, but Aitta does not allow sensitive data. The terms of use set out what you may send.
```
