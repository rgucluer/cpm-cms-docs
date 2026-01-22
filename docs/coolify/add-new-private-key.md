- Servers -> <servername> -> Configuration -> Private Key
  - You should not use passphrase protected keys.
  - Add
    - Click `Generate new ED25519 SSH Key` button
    - Name: root
    - Copy all content of Public key
    - On Virtual machine as root user
      ```bash
      nano /root/.ssh/authorized_keys
      ```
      - Paste private key at the end of the file. Save & exit
    - Back to Coolify New Private Key Form
      - Click Continue
      - Click Save

- Coolify Web User Interface:
  - Servers -> devserver1 -> Configuration -> Private Key
    - Click `Use this key` under devserver1
