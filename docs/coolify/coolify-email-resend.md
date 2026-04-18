# Set e-mail with Resend

- Configure Resend to send emails
  - resend.com/emails
    - Domains  
      - Add domain
        - Name: my-domain.com
        - Region: Choose nearest to you
        - Add domain
          - Read Fill in your DNS Records section, and set your DNS records according to the document.
            - DKIM
          - Enable Sending (Green Enabled)
            - SPF
            - DMARC
          - If you enable receiving, also set the mx record for it.

- Copy API Key from Resend
  - Resend -> API Keys:
    - Delete if any unused exiting API keys
    - Create API Key
      - Name: resendsend
      - Permission: Sending access
      - Domain: Select your domain ( my-domain.com )
      - Add
        - View API Key
          - Click copy icon, and store the API key in a safe place
          - After you store the API key to a safe place Click Done

- Coolify Settings -> Transactional Email
  - From Name: John Doe
  - From Address: info@my-domain.com
  - Resend
    - API Key: Enter the API Key we created at the previous step
    - Check Enabled
    - Save
  - Save

- Click Send Test Email
  - Enter Recipient address and hit Send Email

- Coolify UI -> Notifications -> Email
  - Check: Use system wide (transactional) email settings
  - Notification Settings:
    - Check notifications you want. 
      - (Don't forget!; Resend has limited resources.)
    - Save

