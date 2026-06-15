# Publishing `n8n-nodes-nilai`

> **Gate:** publish only after the engineering review of the attestation verification has signed off
> (`../nilAI_attestation_review.md`).

The package name `n8n-nodes-nilai` is free on npm, and ownership stays personal (your personal email / npm account / GitHub) — not under a Nillion org.

## 1. One-time accounts (use your personal email)
- **npm account:** sign up at npmjs.com if you don't have one. Then create an **automation access token**: Account → *Access Tokens* → *Generate New Token* → *Automation*. Copy it.
- **GitHub repo:** create `https://github.com/iamrobertmoore/n8n-nodes-nilai` (public) and push this folder to it. The repo URL is already referenced in `package.json`.

## 2. Publish — no Node install needed (via Docker)
You already run Docker, so you can build + publish inside a Node container without installing Node on your Mac. From this folder:

```bash
cd ~/n8n/n8n-nodes-nilai
docker run --rm -v "$PWD":/app -w /app -e NPM_TOKEN=PASTE_YOUR_TOKEN node:20 sh -c '
  npm config set //registry.npmjs.org/:_authToken=$NPM_TOKEN &&
  npm install &&
  npm run build &&
  npm publish --access public
'
```

That installs deps, rebuilds `dist/` fresh, and publishes. (`prepublishOnly` also rebuilds as a safety net.)

### Alternative: if you do have Node locally
```bash
npm login
npm install && npm run build
npm publish --access public
```

## 3. Verify it worked
- `https://www.npmjs.com/package/n8n-nodes-nilai` should show v0.1.0.
- In any n8n: **Settings → Community Nodes → Install** → enter `n8n-nodes-nilai` → it should install and the **nilAI** node + **nilAI API** credential appear.

## 4. Releasing updates later
Bump the version and republish:
```bash
# edit "version" in package.json (or `npm version patch`), then re-run the publish command above
```

## 5. After publishing
- Submit for n8n **verified** status (separate review on n8n's side; flag as "in progress", it takes time).
- Publish the privacy **templates** (the summariser first).
- Track the review's one remaining open item: get Nillion to publish per-release measurements (the measurement-mismatch hard-fail, `verify_attestation_tcb` and TLS-session binding are all implemented and tested).

## Notes
- The published package contains only `dist/` + `index.js` (see `files` in `package.json`) — no source, no `node_modules`.
- The node has **no runtime dependencies** (Node built-in `crypto` only), which keeps installs clean and the verifier easy to audit.
