---
title: "Learn Hugo 101"
date: 2021-08-21T03:18:45-04:00
draft: false
author: "Daniel Arapi"
language: "English"
tags: ["hugo", "web", "study-notes"]
categories: ["web-development"]
---

## Setting up Tools

### Install Chocolatey

- This is used for the next step of installing Hugo via CLI
- Go to https://chocolatey.org/install
- Copy/paste the command into PowerShell (run it as Administrator)

Verify successful install of Chocolatey:

```.sh
PS C:\Users\da202> choco
Chocolatey v0.10.15
Please run 'choco -?' or 'choco <command> -?' for help menu.
PS C:\Users\da202>
```

---

### Install Hugo

Use Chocolatey (in PowerShell) to install Hugo:

```.sh
PS C:\Users\da202> choco install hugo-extended
PS C:\Users\da202> choco upgrade hugo-extended

PS C:\Users\da202> hugo version
hugo v0.87.0-B0C541E4+extended windows/amd64 BuildDate=2021-08-03T10:57:28Z VendorInfo=gohugoio
```

---

### Add VS Code Extensions

- Code Spell Checker
- Makrdownlint
- EditorConfig for VS Code
- Better TOML
- GitLens — Git supercharged

---

### Install Git

```.sh
PS C:\Users\da202> choco install git
PS C:\Users\da202> git version
git version 2.33.0.windows.1
```

 **Command**                        | **Function**
------------------------------------|-----------------------------------------------
 git status                         | View git status
 git add \.                         | Add untracked file to git
 git commit \-m "this is a comment" | Commit changes to local repo with  commentary
 git clone                          | Clone a remote repo to local computer
 git push                           | Push changes from local repo to remote repo
 git pull                           | Pull changes from remote repo to local repo

- Learning Resources: [www.try.github.io](#www.try.github.io)

---

### Install Posh-Git

Posh-Git provides visuals in the CLI to make using git easier

```
PS C:\Users\da202> choco install poshgit
```

---

### Instal NodeJS

```.sh
C:\Users\da202> choco install nodejs
C:\Users\da202> choco upgrade nodejs

C:\Users\da202> npm --version
7.20.3
C:\Users\da202> node --version
v16.7.0
C:\Users\da202>
```

---

### Install 7Zip

```.sh
C:\Users\da202> choco install 7zip
C:\Users\da202> choco upgrade 7zip
```

---

## Setup Git and Repository

### Workflow Overview

- All website changes will be made on local computer
- These changes will be pushed to GitHub repo
- Netlify website will pull the changes from the GitHub repo

---

### Set up SSH Keys for Git

- Open Git Bash

```.sh
(IN GITBASH)
C:\Users\da202>ssh-keygen -t rsa -C "daniel@arapiconsulting.com" -b 4096
C:\Users\da202>cat ~/.ssh/id_rsa.pub | clip
```

- Copy/Paste the key to:
`Go to GitHub --> Settings --> SSH and GPG keys --> New SSH key`

---

### Create New Repo and Clone Locally

- https://github.com/danielarapi/PORTFOLIO-WEBSITE

```.sh
mkdir C:\myWebsite

cd C:\Hugo\Sites
hugo new site portfolio-website
cd portfolio-website\themes\
git clone git@github.com:danielarapi/portfolio-website.git
```

---

## Build New Hugo Site

### Create New Site

```.sh
hugo new site danielarapi
```

Create .gitignore and create .editorconfig

---

## Hugo Theme

- Download: https://themes.gohugo.io/
- My top two favorites:
  - Academic: https://academic-demo.netlify.app/
  - Coder: https://hugo-coder.netlify.app/
  - Hell-Friend-NG: https://themes.gohugo.io/themes/hugo-theme-hello-friend-ng/

---

## Hugo Commands

 **Function**      | **Command**
-------------------|------------------------------------
 New Site          | hugo new site \[website\_name\]
 Activate Site     | hugo server
 New File          | hugo new \[file\_name\]
 YouTube Shortcode | \{ \{< youtube website\_id >\}\}\`

---

## Taxonomies

Add taxonomy to single page, e.g.:

```.sh
tags: ["hugo", "web", "study_notes"]
categories: ["web-development"]
moods: ["happy", "upbeat"]
```

Define new taxonomy in config.toml file.
In this example, you can go to [website.com/moods/]

```.sh
[taxonomies]
  tag = "tags"
  category = "categories"
  mood = "moods"
```

---

## Templates

 **Page Type** | **Default Template Path**                      | **Custom Template Path**
---------------|------------------------------------------------|-----------------------------------------
 List Page     | /website/themes/layouts/\_default/list\.html   | /website/layouts/\_default/list\.html
 Single Page    | /website/themes/layouts/\_default/single\.html | /website/layouts/\_default/single\.html
 Home Page     | /website/themes/layouts/\_default/list\.html   | /website/layouts/\_default/index\.html
 Base          | n/a                                            | /website/layouts/\_default/baseof\.html

