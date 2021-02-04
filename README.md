# ansible-proxmox-ssl
This is an Ansible playbook for creating SSL certificates using Let's Encrypt. This playbook uses DNS-based authentication so you don't need to do any port forwarding or run any kind of web server.

This playbook assumes you already own a domain name that's set up with Cloudflare and you have a linux machine in some form with Ansible already installed.

If you're not familiar with Ansible I recommend LearnLinuxTV's [Ansible playlist](https://www.youtube.com/playlist?list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70) on youtube.

### Ansible setup

The account you're using to run the ansible playbook should have its own SSL keypair for SSH to use. The public key should be installed on every machine that this playbook will be interacting with. Ansible will assume that the user you're currently using exists on every proxmox machine, has sudo rights and has the same password.

### Cloudflare setup

Before Let's Encrypt can do anything you need to add a TXT record. On the Account Home page of Cloudflare click the domain name you have registered with them. Click the DNS tab at the top. Click *+ Add record* and create a new TXT record with *name* set to your domain name, *content* can be left blank and a *TTL* of Auto is fine. Click Save.

Next we need the Global API key. Click *Overview* and scroll down and click *Get your API token* on the right side then click *API Tokens*. Where it says *Global API Key* click View, enter your password and complete the Captcha. The code you're given replaces GLOBAL_API_KEY_HERE in inventory.yml.

### inventory.yml setup

In inventory.yml you'll want to add the FQDN for each proxmox machine to the `hosts` list (in the manner of proxmox.your-domain.site) and in the `crt_subject_alt_name` list. Both lists should match. Set `crt_common_name` and `cf_zone` to the domain name you've set up with Cloudflare and set `cf_account_email` to whatever email address you use to log into Cloudflare.

### Running the playbook

Now that you've set things up all you need to do is run `ansible-playbook --ask-become-pass -i inventory.yml deploy.yml`. Enter your remote user's password when prompted and press Enter. Hopefully it'll work first try.

### Sources

This playbook was bodged together primarily from two sources:

* https://graspingtech.com/ansible-lets-encrypt-esxi/
* https://gist.github.com/ahnooie/f674ac98ea8adb3909bae65f5ecf37ed

I'm not an expert with Ansible so if something goes wrong I'dd do what I can to help but it probably won't be much.
