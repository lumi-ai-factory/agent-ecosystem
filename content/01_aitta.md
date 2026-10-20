---
title: "Aitta"
nav_order: 2
---

# Aitta

Aitta is an inference platform: it runs LLMs% on its own GPUs% so you do not have to serve them yourself. The LUMI AI Factory runs it directly on LUMI GPU nodes%. These LLMs act as the brain for your coding agent%.

Any LUMI project member can use Aitta in two ways:

- **In a browser**: Log in to the [Aitta web frontend](https://aitta.csc.fi) to chat with an LLM immediately.
- **Via the API%**: This connects a harness% or Python script to Aitta. The API is OpenAI-compatible. Most OpenAI-ready tools work with Aitta once you provide its address and your API token%.

<figure>

![Aitta web chat: asked "what is aitta?", Poro 2 70B describes a traditional Finnish storage building, a word in Hindi and Marathi and a possible place name, but not the service it is running on](assets/poro-2-what-is-aitta.png)

<figcaption>The web interface of Aitta</figcaption>
</figure>

## Which LLMs to choose

Aitta hosts several generative LLMs. A coding agent requires an LLM trained for tool calling% (explained on the next page).

To see if an LLM supports this, check its model card on [Hugging Face](https://huggingface.co). Aitta uses standard Hugging Face names. The next page also lists Aitta LLMs that currently support tool calling.

## Where your data goes

A coding agent sends your files, terminal outputs, and system errors to the LLM.

Commercial providers like OpenAI or Anthropic might keep this data to train future models. When using Aitta, your prompts and files stay on LUMI hardware. 

However, Aitta forbids sensitive data. Always read the [Aitta terms of use](https://aitta.csc.fi/terms-of-use) before pointing an agent at a project.

## Resources and limitations

Aitta shares two dedicated GPU nodes among all users. Response times depend on current demand:

- **Running LLMs** answer immediately, though heavy traffic causes delays.
- **Starting an LLM** takes time. If a requested LLM is not running, Aitta must load it into VRAM. This takes minutes, and your agent might seem stuck.
- **When nodes are full**, Aitta queues a Slurm% job for more GPUs. Your request waits for available hardware before loading begins.
- **No guarantees.** Aitta is for research and development. It cannot guarantee an LLM is available at any specific time.

Check running LLMs via the [Aitta web frontend](https://aitta.csc.fi) or by running this command (replace `<YOUR_TOKEN>` with your API token):

```bash
curl -H "Authorization: Bearer <YOUR_TOKEN>" https://aitta-api.csc.fi/worker
```

LLMs marked `"status": "running"` are ready. `"requested"` or `"starting"` means they are queuing or loading.

## Getting started

1. **Log in** to the [Aitta web frontend](https://aitta.csc.fi).
2. **Generate an API token** from the interface, or visit [aitta-auth.csc.fi/myToken](https://aitta-auth.csc.fi/myToken). The token lasts 90 days (or until your LUMI project ends), after which you must give your harness a new one.
3. **Connect your harness** by pointing it to `https://aitta-api.csc.fi/openai/v1` and providing the API token.

For more details, see the [Aitta documentation](https://docs.lumi-supercomputer.eu/laif/inference/aitta/).

<details>
<summary>Optional: using embedding models</summary>

Aitta also hosts embedding% models for semantic search.

To see available embedding models, list all models with your API token and look for them by name (e.g., `intfloat/multilingual-e5-large`):

```bash
curl -H "Authorization: Bearer <YOUR_TOKEN>" https://aitta-api.csc.fi/openai/v1/models
```

Send text to the `/embeddings` endpoint, which matches the OpenAI format:

```bash
curl https://aitta-api.csc.fi/openai/v1/embeddings \
  -H "Authorization: Bearer <YOUR_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{"model": "intfloat/multilingual-e5-large", "input": ["query: How do I run PyTorch on LUMI?"]}'
```

Like an LLM, an inactive embedding model takes minutes to load. It returns the text's embedding as a list of numbers.

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
