# n8n-nodes-nilai

Use [Nillion **nilAI**](https://docs.nillion.com/build/private-llms/overview) — private LLM inference that runs inside a **Trusted Execution Environment (TEE)** — directly in your n8n workflows, with **built-in cryptographic verification** of every response.

This is an [n8n community node](https://docs.n8n.io/integrations/community-nodes/). It lets you add private, verifiable AI to any workflow with nothing more than an API key.

## Why nilAI?

Ordinary AI nodes send your data to a provider you have to trust. nilAI is different:

- **Private by construction.** Models run inside a TEE (a hardware-isolated, encrypted enclave). The operator — including Nillion — cannot see your prompts or the model's output.
- **Verifiable.** Every response is cryptographically signed *inside* the enclave. This node verifies that signature against the enclave's public key, so you can *prove* the answer came from a genuine Nillion TEE and was not altered in transit. The result is exposed as a `tee_verified` field.
- **Drop-in.** nilAI is OpenAI-compatible, so anything you do with a normal LLM — summarisation, classification, triage, extraction, Q&A — works here, privately.

### What is a TEE, briefly?

A Trusted Execution Environment is a secure region of a CPU/GPU that runs code in an encrypted, hardware-isolated enclave. Data inside is invisible to the host operating system, the cloud provider and the operator. The hardware can produce a signed *attestation* proving which code is running and that it is genuinely inside a real enclave. nilAI runs LLMs inside TEEs and signs each response, turning "trust us" into "verify it yourself."

## Installation

In n8n: **Settings → Community Nodes → Install**, then enter `n8n-nodes-nilai`.

(For local development, build the package and load it via `N8N_CUSTOM_EXTENSIONS`.)

## Credentials

Create a **nilAI API** credential and paste your API key. Get one for free at the [Nillion developer portal](https://developer.nillion.com/nilai). The base URL defaults to Nillion mainnet.

## Usage

Add the **nilAI** node, pick a model (loaded live from your endpoint), and enter a prompt. Output fields:

| Field | Meaning |
|---|---|
| `text` | The model's response |
| `tee_verified` | `true` if the response signature verified against the enclave's public key |
| `signature` | The raw TEE signature (secp256k1 / ECDSA) |
| `usage` | Token usage |

Turn off **Simplify Output** to receive the full raw nilAI response.

## How verification works

nilAI signs each response inside the enclave (secp256k1 ECDSA over SHA-256). This node fetches the enclave's public key from `/v1/public_key`, reconstructs the exact signed bytes from the raw response, and verifies the signature — all locally, with no external dependencies.

## Resources

- [nilAI / Private LLMs docs](https://docs.nillion.com/build/private-llms/overview)
- [Nillion developer portal](https://developer.nillion.com/nilai)
- [n8n community nodes](https://docs.n8n.io/integrations/community-nodes/)

## License

[MIT](LICENSE)
