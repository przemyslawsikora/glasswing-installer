# GlassWing installer

1. Prepare server with Ubuntu 18 (tested with Ubuntu 18.04)
2. Connect via SSH to your server as root user
3. Install Ansible
   ```bash
   apt-get update
   apt-get install software-properties-common
   apt-add-repository --yes --update ppa:ansible/ansible
   apt-get install ansible
   ```
4. Pull the GIT repository (master branch)
5. Create private and public key for user admin (use PuttyGen or similar software)
6. Copy the public key into the <code>installer/roles/users/files</code> as <code>admin_key.pub</code>  
   Ensure the content is similar to
   ```
   ssh-rsa AAAAB3...38== admin
   ```
7. Open <code>installer/vars/all.yml</code> file and change <code>ip_address</code> to the IP address of your server
8. Enter directory <code>installer</code>
9. Install Ansible roles
   ```bash
   ansible-galaxy install -r requirements.yml
   ```
10. Run installer with admin password
    ```bash
    ansible-playbook site.yml -i production --extra-vars "adminpassword=<ADMIN_PASSWORD>"
    ```

After installation, you can send messages to the system via:
- kafka protocol on port 9092
- HTTP protocol on port 8082  
  you can send message 'Hello World!' on kafka topic 'messages':
  ```
  curl -X POST -H "Content-Type: application/vnd.kafka.json.v2+json" \
    -H "Accept: application/vnd.kafka.v2+json" \
    --data '{"records":[{"value":"Hello World!"}]}' \
    "http://<ip_address>:8082/topics/messages"
  ```

Kafka Manager is available on http://<ip_address>:9000
