---
layout: docs
title: "Integrations - Tutorial"
parent: Docs
redirect_from:
  - /docs/tutorials/deploy
---

##### Tutorial

# Integrate Everywhere™

Integrate everywhere you deploy your code, with Dotenv Vault.

#### Run dotenv-vault build

Open terminal, enter your project's root directory (where your .env.vault file is), and run dotenv-vault build.

```
$ npx dotenv-vault build
```

{% include helpers/screenshot.html url="https://res.cloudinary.com/dotenv-org/image/upload/v1665603984/npx-build_qqcbcb.gif" %}

Your `.env.vault` file should now look something like this.

```
DOTENV_VAULT=vlt_509ada...
DOTENV_VAULT_DEVELOPMENT="Z53YGRqDcfkQZ..."
DOTENV_VAULT_CI="Ib3n/NKbTVO..."
DOTENV_VAULT_STAGING="up1/d3F13uCKrbsU..."
DOTENV_VAULT_PRODUCTION="oJ6qWV1erwlU1TP4..."
```

As you can see, your environment variables are encrypted per environment.

#### Set DOTENV_KEY

The `DOTENV_KEY` decrypts the encrypted values from `DOTENV_VAULT_PRODUCTION`. Run dotenv-vault keys production.

```
$ npx dotenv-vault keys production
```

That will output your DOTENV_KEY.

```
dotenv://:key_1234@dotenv.org/vault/.env.vault?environment=production
```

Set `DOTENV_KEY` on your infrastructure. For example, on Heroku:

```
$ heroku config:set DOTENV_KEY="dotenv://:key_1234@dotenv.org/vault/.env.vault?environment=production"
```

#### Require dotenv-vault-core

The last step is to add the [dotenv-vault-core library](https://github.com/dotenv-org/dotenv-vault-core) into your code.

As early as possible in your application, import and configure dotenv-vault-core:

```
require('dotenv-vault-core').config()
console.log(process.env) // remove this after you've confirmed it working
```
[Node](https://github.com/dotenv-org/dotenv-vault-core), [Ruby](https://github.com/dotenv-org/dotenv-vault-ruby), [Python](https://github.com/dotenv-org/python-dotenv-vault) – [Request language](https://github.com/dotenv-org/dotenv-vault/discussions/95)

That's it!

When your app boots, it will recognize the `DOTENV_KEY`, decrypt the `.env.vault` file, and load the variables to `ENV`. If a `DOTENV_KEY` is not set (like during development on your local machine) it will fall back to regular dotenv.

---

#### Bonus: Platform Integration Tutorials

Sometimes it is nice to have a platform or framework specific tutorial. Here are some below.

{% include helpers/integrations_grid.html %}
