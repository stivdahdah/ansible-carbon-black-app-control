# Ansible Playbook for VMware Carbon Black App Control

No Galaxy modules are required.

This playbook will help you to install Carbon Black Application Control on RHEL/CentOS.

This playbook has 3 tasks: <br>
Task-01: Create a directory called CarbonBlackApp in /opt <br>
Task-02: Download and Extract VMware Carbon Black App Control from Carbon Black App Control Server to the folder that was created in Task-01 <br>
Task-03: Install VMware Carbon Black App Control from the directory /opt/CarbonBlackApp/

Requirements:
- The network connection between your agent and Carbon Black App Control server should be Allowed since this playbook will fetch the installation from the CB server directly.

To get the Policy Installation Package link: <br>
1- Login to the App Control console. <br>
2- Navigate to https://ServerName/hostpkg/ and copy the package link. <br>

![Alt text](https://github.com/stivdahdah/ansible-carbon-black-app-control/blob/main/carbon_black_package_example.png?raw=true "CB Package")


**Example Playbook**


```
---


- name: Playbook to Download and Install VMware Carbon Black App Control
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
      src: "https://192.168.1.9/hostpkg/pkg.php?pkg=App-Control-redhat.tgz"
      validate_certs: no
      dest: "/opt/CarbonBlackApp/"
      mode: 0755
      remote_src: yes

  - name: Install VMware Carbon Black App Control
    become: yes
    shell:
      "./b9install.sh -n"
    args:
        chdir: "/opt/CarbonBlackApp/App-Control-redhat/" 
    register: output
```
