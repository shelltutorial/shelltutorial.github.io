---
title: Set Up Your Workstation With Ansible
description: Learn How To Set Up Your Own Workstation With Ansible Automation Tool
header: Set Up Your Workstation With Ansible
---

Ansible is a tool for automation that belongs to Red Hat, Inc. It can manage all infrastructure state with no need of agent or client for the nodes. Ansible connects to its nodes and sends little pieces of code named modules.

A very long list of tasks can be done with Ansible. Here there is a playbook we can use to set up our own machine after the end of operating system instalation. Playbooks are written in a yaml file. They tell Ansible the state the system has to achieve and what changes we want to be applied.

Our playbook will install the following software:

- vim (apt repository)
- htop (apt repository)
- python3-pip (apt repository)
- make (apt repository)
- flameshot (apt repository)
- mlocate (apt repository)
- terminator (apt repository)
- xpad (apt repository)
- retext (apt repository)
- Zoom (third part software with third party repository)
- VirtualBox (third part software with third party repository)
- Vagrant (third part software with third party repository)
- Microsoft Teams (third part software with third party repository)
- Google Chrome Browser (third part software with third party repository)
- Brave Browser (third part software with third party repository)

~~~ yaml
---
  name: Preparing Workstation  
  hosts: localhost  
  connection: local
  tasks:

      name: Installing Linux Apps
      become: true
      apt:
        name: {{ item }}
        install_recommends: yes
        state: present
      loop:
          - vim
          - htop
          - python3-pip
          - make
          - flameshot
          - mlocate
          - terminator
          - xpad
          - retext

      name: Installing Zoom
      become: true
      apt:
        deb: "https://zoom.us/client/latest/zoom_amd64.deb"
        install_recommends: yes
        state: present

      block:
        name: Install Virtualbox Oracle Key
        become: true
        apt_key:
          url: 'https://www.virtualbox.org/download/oracle_vbox_2016.asc'
          state: present
        name: Add Virtualbox Oracle repository
        become: true
        apt_repository:
          repo: 'deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian {{ ansible_distribution_release }} contrib'
          state: present
          filename: virtualbox
        name: Install Oracle Virtualbox
        become: true
        apt:
          name: virtualbox-6.1

      block: 
        name: Install Vagrant Key
        become: true
        apt_key:
          url: 'https://apt.releases.hashicorp.com/gpg'
          state: present
        name: Install Vagrant Repository
        become: true
        apt_repository:
          repo: 'deb [arch=amd64] https://apt.releases.hashicorp.com {{ ansible_distribution_release }} main'
          state: present
          filename: vagrant
        name: Install Vagrant
        become: true
        apt:
          name: vagrant

          
      block:
        name: Install Microsoft Key
        become: true
        apt_key:
          url: 'https://packages.microsoft.com/keys/microsoft.asc'
          state: present
        name: Add Microsoft Teams Repository
        become: true
        apt_repository:
          repo: 'deb [arch=amd64] https://packages.microsoft.com/repos/ms-teams stable main'
          state: present
          filename: teams
        name: Install Microsoft Teams
        become: true
        apt:
          name: teams
        
      block: 
        name: Install Google Chrome Key
        shell: wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
        name: Install Google Chrome Repository
        become: true
        apt_repository:
          repo: 'deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main'
          state: present
          filename: brave
        name: Install Google Chrome
        become: true
        apt:
          name: google-chrome-stable

      block: 
        name: Install Brave Key
        become: true
        apt_key:
          url: 'https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg'
          state: present
        name: Install Brave Repository
        become: true
        apt_repository:
          repo: 'deb [arch=amd64] https://brave-browser-apt-release.s3.brave.com/ stable main'
          state: present
          filename: brave
        name: Install Brave Browser
        become: true
        apt:
          name: brave-browser  
---
~~~

It was tested for Ubuntu 18.04 and 20.04.  
 
| ![fail](/assets/images/fail.gif) |  |  **Do not copy and paste the yaml content from above. Go to git repository. See the git link there below.** |

In order to make the playbook work correctly first you must testify if Ansible it is installed. If it is not installed just follow the steps below:

```console
$ sudo apt update && sudo apt install ansible unzip git
```

For the execution of the playbook:

```console
$ ansible-playbook workstation.yml -K
```

Where "-K" option means "ask pass" to allow permission.

[https://github.com/shelltutorial/workstation](https://github.com/shelltutorial/workstation)
