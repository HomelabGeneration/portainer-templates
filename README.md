# homeserver stack

Repository to show how you can deploy a whole homeserver with [Ansible](https://www.ansible.com). [Cloudflare Access](https://www.cloudflare.com/de-de/products/zero-trust/access/) is used to show it to the world with authentication. When complete, your Git repository will control the state of your homeserver.

## Overview

- [Introduction](https://github.com/wlwwt/home-stacks#-introduction)
- [Prerequisites](https://github.com/wlwwt/home-stacks#-prerequisites)
- [Repository structure](https://github.com/wlwwt/home-stacks#-repository-structure)
- [Lets go!](https://github.com/wlwwt/home-stacks#-lets-go)
- [Post installation](https://github.com/wlwwt/home-stacks#-post-installation)
- [Troubleshooting](https://github.com/wlwwt/home-stacks#-troubleshooting)
- [What's next](https://github.com/wlwwt/home-stacks#-whats-next)

## 👋 Introduction

The following components will be installed in your homeserver by default. Most are only included to get a minimum viable system up and running.

- [Docker](https://www.docker.com/) - Gui for docker containers
- [Portainer](https://www.portainer.io/) - Docker engine for running containers

For provisioning the following tools will be used:

- [Ansible](https://www.ansible.com) - Sets up the operating system and installs the required software

## 📝 Prerequisites

### 💻 Systems

- One node with a fresh install of [Ubuntu 22.04 Server](https://ubuntu.com/download/server).
- A [Cloudflare](https://www.cloudflare.com/) account with a domain, this will be managed by Terraform ([To Do](https://developers.cloudflare.com/cloudflare-one/api-terraform/access-with-terraform/)).

## 📂 Repository structure

The Git repository contains the following directories under `stacks` for usage with [Portainer](https://www.portainer.io/).

```sh
📁 portainer              # Portainer files
├─📁 applications         # Holding all the third party docker applications as docker compose based version.
└─📁 customizations       # Holding portainer customizations like a custom list of application templates.
```

## 🚀 Lets go

📍 **All of the below commands** are run on your **local** workstation, **not** on any of your remote nodes.

### 🔧 Workstation Tools

📍 Install the **most recent version** of the CLI tools below. If you are **having trouble with future steps**, it is very likely you don't have the most recent version of these CLI tools.

1. Install the following CLI tools on your workstation, if you are **NOT** using [Homebrew](https://brew.sh/) on MacOS or Linux **ignore** steps 4 and 5.

    * Required: [ansible](https://www.ansible.com), [go-task](https://github.com/go-task/task), [direnv](https://github.com/direnv/direnv), [pre-commit](https://github.com/pre-commit/pre-commit)

    * Recommended: [yamllint](https://github.com/adrienverge/yamllint)

2. This guide heavily relies on [go-task](https://github.com/go-task/task) as a framework for setting things up. It is advised to learn and understand the commands it is running under the hood.

3. [Homebrew] Install [go-task](https://github.com/go-task/task)

    ```sh
    brew install go-task/tap/go-task
    ```

4. [go-task] Install workstation dependencies

    ```sh
    task init
    ```

### ⚠️ pre-commit

It is advisable to install [pre-commit](https://pre-commit.com/) and the pre-commit hooks that come with this repository.

1. Enable Pre-Commit

    ```sh
    task precommit:init
    ```

2. Update Pre-Commit, though it will occasionally make mistakes, so verify its results.

    ```sh
    task precommit:update
    ```

### ⚡ Preparing Ubuntu Server with Ansible

📍 Here we will be running a Ansible Playbook to prepare Ubuntu Server for running docker.

1. Ensure you are able to SSH into your nodes from your workstation using a private SSH key **without a passphrase**. This is how Ansible is able to connect to your remote nodes.

   [How to configure SSH key-based authentication](https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server)

2. Install the Ansible deps

    ```sh
    task ansible:init
    ```

3. Verify Ansible can view your config

    ```sh
    task ansible:list
    ```

4. Verify Ansible can ping your nodes

    ```sh
    task ansible:ping
    ```

5. Run the Ubuntu Server Ansible prepare playbook

    ```sh
    task ansible:prepare
    ```

6. Run the Ubuntu Server Ansible update playbook

    ```sh
    task ansible:update
    ```

7. Reboot the nodes (if not done in step 5)

    ```sh
    task ansible:force-reboot
    ```

8. Install docker with Ansible

    ```sh
    task ansible:install
    ```

## 📣 Post installation

### 🌱 Environment

[direnv](https://direnv.net/) will make it so anytime you `cd` to your repo's directory it export the required environment variables (e.g. `ANSIBLE_CONFIG`). To set this up make sure you [hook it into your shell](https://direnv.net/docs/hook.html) and after that is done, run `direnv allow` while in your repos directory.

### 🤖 Renovatebot

[Renovatebot](https://www.mend.io/free-developer-tools/renovate/) will scan your repository and offer PRs when it finds dependencies out of date. Common dependencies it will discover and update are Ansible Galaxy Roles, Docker Images, Pre-commit hooks updates, and more!

The base Renovate configuration provided in your repository can be view at [.github/renovate.json5](https://github.com/HomelabGeneration/home-stacks/blob/main/.github/renovate.json5). If you notice this only runs on weekends and you can [change the schedule to anything you want](https://docs.renovatebot.com/presets-schedule/) or simply remove it.

To enable Renovate on your repository, click the 'Configure' button over at their [Github app page](https://github.com/apps/renovate) and choose your repository. Over time Renovate will create PRs for out-of-date dependencies it finds. Any merged PRs that are in the kubernetes directory Flux will deploy.s
