# FLUX.2 Flex API (`flux-2-flex`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.04 per image** on the APIMart route — one OpenAI-compatible endpoint at `https://api.apimart.ai/v1`, no monthly plan required. *(observed 2026-10-12)*

**[Get an API key](https://go.apimart.ai/k-30c69c)** · **[Live pricing](https://go.apimart.ai/k-810391)** · **[Model page](https://go.apimart.ai/k-27e19a)** · [⚡ 60-second quickstart](#quickstart)

**Why teams call FLUX.2 Flex (`flux-2-flex`) through APIMart**

- **One key, entire catalog.** The same `https://api.apimart.ai/v1` base URL and `Authorization` header reach FLUX.2 Flex (`flux-2-flex`) and 300+ other image, video and language models — switch the `model` field, not your client.
- **$1 minimum, pay as you go.** No subscription and no prepaid plan to size up front: top up from $1 and spend it on calls. There is no free quota to burn through first, so the price in this table is the price you pay.
- **The charge comes back in the response.** Every call reports the amount billed (`cost` / `credits_cost`), so a spend number is read per call instead of guessed at month end.
- **Async by design.** Submit, take the `task_id`, poll `GET /v1/tasks/{id}` — batching and retries are ordinary queue work, not a bespoke integration.

<!-- /conv-kit:v1 -->



FLUX.2 Flex generates and edits images with adjustable steps and guidance for fine control and typography.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `flux-2-flex` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-27e19a) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| 1MP | 0.05 | 0.04 |
| 2MP | 0.1 | 0.08 |
| 3MP | 0.15 | 0.12 |
| 4MP | 0.2 | 0.16 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-810391) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-27e19a) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"flux-2-flex","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "flux-2-flex", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**What is the FLUX.2 Flex model id?**

`flux-2-flex`. See the route table above.

**How is it billed?**

**from $0.04 per image** on the APIMart route. The completed task reports the exact amount in its `cost` field; a failed task is refunded.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-30c69c) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, other languages** — 9 more language builds: 中文 `flux-2-flex-api-cn` · Deutsch [flux-2-flex-api-de](https://github.com/tryjmmj1-at/flux-2-flex-api-de) · Español [flux-2-flex-api-es](https://github.com/x2dl2g/flux-2-flex-api-es) · Français [flux-2-flex-api-fr](https://github.com/c3cnu77tfa3g/flux-2-flex-api-fr) · Bahasa [flux-2-flex-api-id](https://github.com/tvy8j7dlbtq-v/flux-2-flex-api-id) · 日本語 `flux-2-flex-api-ja` · 한국어 [flux-2-flex-api-ko](https://github.com/bsl2e86y9/flux-2-flex-api-ko) · Português `flux-2-flex-api-pt` · Русский [flux-2-flex-api-ru](https://github.com/v501hfo6zxyf4/flux-2-flex-api-ru)
- **More models in this language** — [midjourney-api-gateway](https://github.com/apimartxc3iskw/midjourney-api-gateway) · [gemini-2-5-flash-image-preview-api-gateway](https://github.com/mcqdlebg/gemini-2-5-flash-image-preview-api-gateway) · [nano-banana-api-gateway](https://github.com/gix2kust/nano-banana-api-gateway) · [gemini-3-1-flash-image-preview-api-gateway](https://github.com/nn6jtthao8/gemini-3-1-flash-image-preview-api-gateway) · [nano-banana-2-api-gateway](https://github.com/u9mdw3pt594q2r/nano-banana-2-api-gateway) · [gemini-3-pro-image-preview-api-gateway](https://github.com/uqu9q/gemini-3-pro-image-preview-api-gateway)
- **Topic hubs** — every build of this model, all languages: [flux-2-flex](https://github.com/topics/flux-2-flex) · [flux-2-flex-api](https://github.com/topics/flux-2-flex-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-27e19a> | `apimart.ai/model/flux-2-flex` |
| Live pricing | <https://go.apimart.ai/k-810391> | `apimart.ai/pricing` |
| Get an API key | <https://go.apimart.ai/k-30c69c> | `apimart.ai/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

FLUX.2 Flex is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
