# GlassWing installer

1. Prepare server with installed Ubuntu 18 (tested with Ubuntu 18.04)
2. Connect via SSH to your server via root user
3. Install Ansible
   ```bash
   sudo apt-get update
   sudo apt-get install software-properties-common
   sudo apt-add-repository --yes --update ppa:ansible/ansible
   sudo apt-get install ansible
   ```
4. Pull the GIT repository (master branch)
5. Create your personal private and public key using PuttyGen or similar software
6. Copy the public key into the <code>roles/users/files</code> as <code>admin_key.pub</code>  
   Ensure the content is similar to
   ```
   ssh-rsa AAAAB3...38== admin
   ```
7. Enter directory <code>installer</code>
8. Install Ansible roles
   ```bash
   sudo ansible-galaxy install -r requirements.yml
   ```
9. Run installer with admin password
   ```bash
   sudo ansible-playbook site.yml -i production --extra-vars "adminpassword=<ADMIN_PASSWORD>"
   ```
