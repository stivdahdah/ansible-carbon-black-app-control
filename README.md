# ansible-carbon-black-app-control
Ansible Playbook To Install VMware Carbon Black App Control on Linux

No Galaxy modules are required.

This playbook will help you to install SolarWinds Agent on RHEL/CentOS and Debian/Ubuntu.

First, you should generate the agent installation command from SolarWinds. This command should be placed in the YAML file: 
Please follow the below steps (You need to copy the command from Step-8):
https://documentation.solarwinds.com/en/success_center/orionplatform/content/core-deploy-a-linux-agent-manually.htm

Requirements:
- The network connection between your agent and SolarWinds should be Allowed since this playbook will fetch the installation from the SolarWinds server directly.

Note: RHEL/CentOS generated command is different from Debian/Ubuntu. I would suggest creating a playbook for each distro. 

Once the command line is generated copy the command line and place it in Lice-7 after "shell: "

**Example Playbook**
Let's say that we generated the below command from SolarWinds.
We will have to copy the command and place it in Licne-7 as a value for "shell: " in solarwinds_agent_Installation.yml. 

```
---

- name: Playbook to Download and Install VMware Carbon Black App Control in Visibility Mode
  hosts: all
  
  tasks:
  - name: Create a Directory called CarbonBlackApp in opt
    become: yes
    file:
      path: "/opt/CarbonBlackApp"
      state: directory
      mode: 0755

  - name : Download and Extract VMware Carbon Black App Control from Carbon Black App Control Server
    become: yes
    unarchive:
      src: "https://192.168.1.9/hostpkg/pkg.php?pkg=App-Control-Visibility-redhat.tgz"
      validate_certs: no
      dest: "/opt/CarbonBlackApp/"
      mode: 0755
      remote_src: yes

  - name: Install VMware Carbon Black App Control
    become: yes
    shell:
      "./b9install.sh -n"
    args:
        chdir: "/opt/CarbonBlackApp/App-Control-Visibility-redhat/" 
    register: output
```
