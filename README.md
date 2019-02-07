# AWX with SSL :sunglasses:

This repo is a fork of [AWX](https://github.com/ansible/awx) with a just one
change.

It generates the SSL certificates, uses them to serve AWX on HTTPS and
also renews the certificates. :tada: :confetti_ball:

## Steps to setup AWX

**Note:** This is not an official install guide for AWX.
You should absolutely read [official install guide](
https://github.com/ansible/awx/blob/devel/INSTALL.md) if you have not setup AWX
before. Mentioned below are steps to deploy AWX using Docker-Compose. Altough
the steps have only been tested on Ubuntu 18.04, but should work on Ubuntu
14.04 and 16.04 also. Still, YMMV.

1. Setup Ansible :a:
```bash
sudo apt-get update && \
  sudo apt-get install software-properties-common -y && \
  sudo apt-add-repository --yes --update ppa:ansible/ansible -y && \
  sudo apt-get install ansible -y
```

2. Setup Docker and Docker-Compose :whale:
```bash
curl -fsSL https://get.docker.com | sudo -E bash -

sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

3. Setup Node.js
```bash
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
```

4. Setup other dependencies
```bash
sudo apt-get install python-pip -y
pip install docker
pip install docker-compose
```

5. Clone the project
```
git clone https://github.com/mbtamuli/awx.git --depth=1
git clone https://github.com/ansible/awx-logos.git
```

6. Configure and run the installer. :rocket:

**Note:** _Replace the values for `awx_hostname` and `le_email`_
```
cd awx/installer
sed -i \
  -e 's/^.*use_docker_compose.*/use_docker_compose=true/' \
  -e 's/.*awx_official.*/awx_official=true/' \
  -e 's/.*awx_hostname.*/awx_hostname=awx.example.com/' \
  -e 's/.*le_email.*/le_email=admin@example.com/' \
  inventory
ansible-playbook -i inventory install.yml
```
