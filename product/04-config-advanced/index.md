﻿[title]: # (Configure DSV: Advanced)
[tags]: # (,)
[priority]: # (4000)

# Configure DSV: Advanced

The `thy init` command without flags produces a default DSV config. A custom config offers more control, allowing you to:

* choose how DSV stores credentials
* determine the authentication type
* select the cache strategy

To configure DSV with custom settings, use the `thy init --advanced` command.

```bash
thy init --advanced
Please enter tenant name: example
Please enter domain (default:secretsvaultcloud.thycotic.com):
```

DSV will prompt you about **credential storage**.

```bash
Please enter store type:
        (1) File store (default)
        (2) None (no caching)
        (3) Pass (linux only)
        (4) Windows Credential Manager (windows only)
```

Select `(1) File store (default)` to keep the credentials in a configuration file. DSV will prompt for the storage location.

```bash
Please enter directory for file store (default:/home/username/.thy):
```

Select `(2) None (no caching)` to avoid storing the credentials. With this option active, DSV requires periodic re-authentication.

Select `(3) Pass (linux only)` to use [Linux pass](https://www.passwordstore.org/) for encrypted storage.

Select `(4) Windows Credential Manager (windows only)` to use [Windows Credential Manager](https://support.microsoft.com/en-us/help/4026814/windows-accessing-credential-manager) to store credentials.

Next, the initialization process will prompt about the **type of authentication**.

```bash
Please enter auth type:
        (1) Password (default)
        (2) Client Credential
        (3) AWS IAM (federated)
        (4) Azure (federated)
```

Select `(1) Password (default)` to authenticate by username and password.

Select `(2) Client Credential` to authenticate by Client ID and Client Secret authentication; this supports use of DSV commands by applications.

Select `(3) AWS IAM (federated)` to authenticate as a trusted IAM Role or User.

Select `(4) Azure (federated)` to authenticate as a trusted Azure MSI.

DSV pertains to secrets and their access and use, so authentication naturally figures prominently in DSV setup considerations. The [Authentication • General](..\authentic-gen\index.htm) and [Authentication • Azure and AWS](..\authentic-other\index.htm) sections provide more details on how to set up DSV to work with various authentication methods.

Finally, the initialization process will prompt about the **cache strategy for secrets**. The choice here depends on your specific set of concerns around security, network connectivity, performance, and systems availability.

```bash
Please enter cache strategy for secrets:
        (1) Never (default)
        (2) Server then cache
        (3) Cache then server
        (4) Cache then server, but allow expired cache if server unreachable
```

* DSV defaults to **never** caching secrets because that is the most secure choice.

  However, not all environments require such airtight security—and this choice requires more network traffic, since anytime DSV needs a secret it must obtain it directly from the vault.
  
  It also means that problems with the network or server that prevent secrets access will be felt immediately as process interruptions.

* The second choice, **server then cache**, allows DSV to use a cached copy of the secret in lieu of obtaining it directly from the vault. This can help blunt the impacts of problems affecting the server or connectivity to the server.

* **Cache then server** can substantially reduce secrets-related network traffic while also reducing the load on the server, because once DSV accesses a secret, it need not pull that secret from the server again until the cached copy expires.

* The last choice allows use of cached secrets even when they have expired, provided the server cannot be reached. This offers the most resilience against problems with the server or network, but may not comport with some organization’s security policies.

Selecting the cache strategy completes the configuration. You can begin using the DSV CLI to access secrets in your organization’s Secret Server vault, and to perform operations on those secrets.

The [Commands Overview](..\05-cli-overview\index.htm), [Commands Examples](..\06-cli-examples\index.htm), and [CLI Reference](..\07-cli-ref\index.htm) will get you started.


