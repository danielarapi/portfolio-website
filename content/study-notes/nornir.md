---
title: "Network Automation with Nornir"
date: 2021-08-24T03:18:45-04:00
draft: false
author: "Daniel Arapi"
language: "English"
tags: ["study-notes", "nornir", "automation", "nornir-scrapli", "nornir-napalm", "nornir-netmiko", "netbox"]
---


**Create a python 3 virtual environment with venv**  

```
cd  
yum update -y  
python3 -m venv arapi-venv  
source arapi-venv/bin/activate
disconnect
```

**Create a directory for Nornir**  

```
cd  
mkdir Nornir-Automation  
cd Nornir-Automation  
touch config.yaml, groups.yaml, hosts.yaml, defaults.yaml, runbook1.py  
```


**Install Nornir**  

- ***Note:*** Do this inside the virtual environment
```
pip install --upgrade pip  
python3 -m pip install nornir nornir-utils nornir_scrapli nornir_netmiko nornir_napalm python-gnupg scrapli[genie] ipdb
pip freeze  
```

 **Order of Operations**  
 
```
hosts.yaml --> groups.yaml --> defaults.yaml
```


---

## Nornir Plugins

- Note: Plugins can be found on the Nornir website [nornir.tech/nornir/plugins]](#https://nornir.tech/nornir/plugins/)

**Nornir only does two things:**

- Inventory Management
- Concurrent Exectuion

**Command execution is performed by any of the following plugins:**

- Scrapli
- Netmiko
- Napalm

**Plugin Installation**

```
python3 -m pip install nornir-utils
python3 -m pip install nornir_scrapli
python3 -m pip install nornir_netmiko
python3 -m pip install nornir_napalm
```

---

```
nr = InitNornir(config_file="config.yaml")
nr.inventory.groups
nr.inventory.hosts
```

---

# Encrypt password

```
python3 -m pip install python-gnupg
gpg --gen-key
gpg --symmetric -o file_name.gpg file_name.txt
```

---

# Sturcutred Data with Genie

```
python3 -m pip install scrapli[genie]
```

