# General

This plugin automates the process of completing a ``dns-01`` challenge by creating, and subsequently removing, TXT records using the (XML-RPC-based) namecheap.com API.
updated to work in python3.x

------------------

## Presequence

### Getting API access

Namecheap has certain requirements for activation to prevent system abuse. In order to have API enabled for your account, you should meet one of the following requirements:

- have at least 20 domains under your account;
- have at least $50 on your account balance;
- have at least $50 spent within the last 2 years.

## Credentials

Use of this plugin requires a configuration file containing Namecheap API credentials, obtained from your Namecheap account's [API Managenment page](https://ap.www.namecheap.com/settings/tools/apiaccess/).

```ini
# Namecheap API credentials used by Certbot
dns_namecheap_username=my-username
dns_namecheap_api_key=my-api-key
```

The path to this file can be provided by using the `--dns-namecheap-credentials` command-line argument.

### Usage

- Make sure that you have python3-full installed.

```sh
python --version
```

- clone the repo anyware you want to store it

```sh
git clone  https://github.com/abdorag/certbot-dns-namecheap.git
```

- go the repo dir

```sh
cd repo-dir-certbot-dns-namecheap
```

- create the ini file

```sh
nano namecheap.ini
```

## Important
- **Not recommended usage** If you know what you're doing install the plugin into the same python environment like `certbot`.
- **Recommended usage** work in isolated enviroment use this routine by creating speacial command as I will show in steps:

- install virtual modules

```sh
sudo apt install python3-venv python3-pip
```

- start virtual env and install the package

```sh
python3 -m venv ~/certbot-venv
source ~/certbot-venv/bin/activate
pip install .
```

- Check that `certbot` discovers the plugin:

```sh
certbot plugins
```
you must see the package name certbot-dns-namecheap in the list

- Now run the command:

```sh
certbot certonly \
  -a dns-namecheap \
  --dns-namecheap-credentials=/namecheap.ini \
  --agree-tos \
  -email "your@mail.com" \
  -d example.com \
  --test-cert
```
if it works that mean you had success installing and use

- Deactivate the virtual environment

```sh
deactivate
```

- creat auto use for the virtual enviroment

```sh
nano /usr/local/bin/certbot-namecheap
```

```bash
#bash
#!/bin/bash
# Activate the virtual environment
source /path/to/certbot-venv/bin/activate

# Run Certbot with all arguments passed to the script
certbot "$@"

# Deactivate the virtual environment
deactivate
```

- give the script excut ablity

```sh
chmod +x /usr/local/bin/certbot-namecheap
```

#command for tested example 
certbot-namecheap [certbot commands]
```sh
certbot-namecheap certonly \
  -a dns-namecheap \
  --dns-namecheap-credentials=/dir/to/namecheap.ini \
  --agree-tos \
  --email email@any.com \
  -d '*.domain.com'
```

