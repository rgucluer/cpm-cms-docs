## Coolify - Create Private Key

- Coolify Web User Interface:
  - Keys & Tokens
    - Private Keys
    - Add
        - Name: < coolify-private-ssh-key-name >
          - Change name as you want    
      - We have two options:
        - Add an existing key
          - Paste key content into Private Key input
          - Continue
          - Save
        - Generate a new key
          - Click Generate new RSA SSH Key
          - Continue
- Coolify Web User Interface:
  - Keys & Tokens
  - Refresh page
---

## Add related public key to VM/VPS's authorized_keys file

ssh to VM
```bash
ssh root@< virtual-m-ip >
```

```bash
nano ~/.ssh/authorized_keys 
```

Copy public key content from Coolify, and paste as a new line in VM text editor. Save & exit .


### Continue with [Set e-mail with Resend](coolify-email-resend.md)

## References

