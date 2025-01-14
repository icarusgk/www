---
layout: docs
title: "DOTENV_KEY - Security"
redirect_from:
  - /dotenv-key
---

##### Security

# DOTENV_KEY

The **DOTENV_KEY** unlocks your encrypted environment variables.

You can view it with **npx dotenv-vault keys**. Set it on your server's environment variables after running **npx dotenv-vault build**.

#### Example

Here's an example of a `DOTENV_KEY`.

```
dotenv://:key_1234@dotenv.org/vault/.env.vault?environment=production
```

#### Viewing

You can view a DOTENV_KEY by running the keys command.

```
$ npx dotenv-vault keys
remote:   Listing .env.vault decryption keys... done
 environment DOTENV_KEY
 ─────────── ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 development dotenv://:key_1234@dotenv.org/vault/.env.vault?environment=development
 ci          dotenv://:key_4567@dotenv.org/vault/.env.vault?environment=ci
 staging     dotenv://:key_7890@dotenv.org/vault/.env.vault?environment=staging
 production  dotenv://:key_9876@dotenv.org/vault/.env.vault?environment=production

Set DOTENV_KEY on your infrastructure
```

Specify an environment to view that key alone.

```
$ npx dotenv-vault keys production
remote:   Listing .env.vault decryption keys... done
dotenv://:key_9876@dotenv.org/vault/.env.vault?environment=production
```
