## Configure Payload CMS Database Public TLS connection

- Coolify UI -> Projects -> cpm-cms production -> Databases: mongodb-payload
  - Stop 
  - Check: SSL Configuration - Enable SSL
    - SSL Mode: allow (insecure)
    - Start

- Coolify UI -> Projects -> cpm-cms production -> Databases: mongodb-payload
  - Mongo URL (public)
   - Reveal
   - Click inside textbox
   - Select all
   - Copy

- Coolify UI -> Projects -> cpm-cms production -> Applications: payload -> Environment Variables
  - DATABASE_URL
    - Reveal, and delete content
    - Paste Mongo URL (public) here
      - Check  Available at Buildtime
      - Check  Available at Runtime
      - Click Update
    - Reveal, Change IP address to the IP address of the Virtual Machine
      - Get IP of Virtual Machine from `multipass list` command    

- Coolify UI -> Projects -> cpm-cms production -> Applications: payload 
  - Redeploy


--- 

Error: 
Error: cannot connect to MongoDB. Details: ENOENT: no such file or directory, open '/etc/mongo/certs/ca.pem'

---

