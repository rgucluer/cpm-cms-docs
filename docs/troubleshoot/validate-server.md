Servers -> Configuration -> General:

Sometime we get "You can't use this server until it is validated." message.

Click Validate Server button.

Errors:
```bash
Error: OpenSSL version mismatch. Built against 3050003f, you have 30500020
```

There is an issue about this
https://github.com/coollabsio/coolify/issues/6692


andrasbacsai offers:

This is a very strange bug. I suggest to downgrade until I figure out the issue:
```bash
curl -fsSL https://cdn.coollabs.io/coolify/install.sh | bash -s 4.0.0-beta.429
```

Docs: https://coolify.io/docs/get-started/downgrade


ssh to server and run the command above

Do not update to new version until this issue is resolved.
