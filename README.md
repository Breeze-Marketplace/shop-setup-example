# Shop setup example

Example project for a **self-hosted shop** used with [Breeze Marketplace](https://foz55-laaaa-aaaan-q6ckq-cai.icp0.io/). You can deploy with **[icp-cli](https://cli.internetcomputer.org/)** or **[dfx](https://internetcomputer.org/docs/current/developer-docs/setup/install/)**; both work with this layout, but **icp-cli is preferred**. Shop **constructor args** and **WASM** are set in **`icp.yaml`** (icp-cli) and mirrored in **`dfx.json`** (dfx) so you can use either tool.

This repo defines two canisters (see `icp.yaml` and `dfx.json`):

- **shop** — product catalog, orders, and related logic (WASM from [Breeze-Marketplace/Shop](https://github.com/Breeze-Marketplace/Shop) releases).
- **media** — stores static assets (images, videos) for your shop, backed by the Internet Computer asset storage canister.

## Configure

Edit **`icp.yaml`** if you deploy with **icp-cli** (recommended), and **`dfx.json`** if you deploy with **dfx**. If both files live in your clone, keep **shop constructor args** and **WASM/candid URLs** aligned when you change them so either tool stays correct.

### Shop constructor (`init_args` / `init_arg`)

The **shop** canister takes Candid constructor arguments. Set them to whatever you want (for example `name` and `description`).

**icp-cli** — in `icp.yaml`, field **`init_args`** (single-quoted Candid):

```yaml
init_args: '(record { name = "My Awesome Shop"; description = ""; })'
```

**dfx** — in `dfx.json`, under the `shop` canister, field **`init_arg`** (JSON string; escape inner double quotes):

```json
"init_arg": "(record { name = \"My Awesome Shop\"; description = \"\"; })"
```

Match the record shape to the Shop WASM you use; if you change the release, check that project’s `.did` or release notes for new or renamed fields.

### Shop WASM `url` and `sha256`

The shop build uses a **pre-built** `shop.wasm.gz` from GitHub. To pin a **newer (or specific) version**:

1. Open [Breeze-Marketplace/Shop — Releases](https://github.com/Breeze-Marketplace/Shop/releases) and pick the tag you want (for example `v0.2.2`).
2. Under **Assets**, confirm **`shop.wasm.gz`** exists for that tag. Set `build.steps[0].url` to:

   `https://github.com/Breeze-Marketplace/Shop/releases/download/<TAG>/shop.wasm.gz`

3. Copy the published **SHA-256** for `shop.wasm.gz` from that release’s page (usually in the release notes or next to the asset) and set `build.steps[0].sha256` to that value (lowercase hex, no spaces). It must match the file for the tag you chose.

**dfx** — in `dfx.json`, set the shop canister’s **`wasm`** and **`candid`** URLs to the same release tag (`dfx.json` does not use a separate `sha256` for those URLs). Keep **`init_arg`** in sync with `icp.yaml`’s `init_args` if you maintain both.

## Deploy

1. Install tooling: **[icp-cli](https://cli.internetcomputer.org/)** (preferred) and/or **[dfx](https://internetcomputer.org/docs/current/developer-docs/setup/install/)**, depending on which you use to deploy.
2. Follow the official guide for your tool — for example [deploying to IC mainnet with icp-cli](https://cli.internetcomputer.org/0.2/guides/deploying-to-mainnet/) or the current dfx docs for mainnet. Identity, cycles, and network flags live there and change over time, so this README does not duplicate them.
3. From this directory, deploy both canisters with either **`icp deploy --environment ic`** (preferred when using icp-cli) or **`dfx deploy --network ic`**. After a successful deploy you should have **shop** and **media** canister IDs in your tooling output or generated env files.

## Register on Breeze

1. Open [Create shop — Breeze Marketplace](https://foz55-laaaa-aaaan-q6ckq-cai.icp0.io/shop/new).
2. Choose **Self-hosted** and enter your **shop** canister ID (not the media canister).

After registration, the marketplace admin experience can upload files to your **media** canister.
