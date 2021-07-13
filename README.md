# Configuration of Target Node by using Variable file having same name as the OS in Ansible

![ansible_redhat_ubuntu](https://miro.medium.com/max/1282/1*haiFwB--epsq52ZoQ-k7GA.jpeg)

## Objective
Configure Target Node by using the variable file of the same name as that of the Target Node’s OS name.
<br><br>

## Content
- **Project Understanding**
- **Output**
<br><br>

## Project Understanding
Let’s understand the implementation part by part

### Part 1 : Ansible Playbook (main.yml)

```yaml
- hosts: target_os
  vars_files:
    - "{{ ansible_facts['distribution'] }}.yml"
    
  tasks:
    - name: Webserver Installation
      package:
        name: "{{ webserver_package_name }}"
        state: present
        
    - name: Placing Web Page in Document Root
      template:
        dest: "/var/www/html/index.html"
        src: "/t14_3/web_page/index.html"
        
    - name: Enabling Webserver service
      service:
        name: "{{ service }}"
        state: started
        enabled: yes
```

#### Important Points

- _The name of **variable file** is updated dynamically as per the **target node’s OS** and which makes it easier to configure webserver as per OS. It internally uses the **facts** associated with a particular target node._
- _Two OS are considered over here, one is **RedHat 8** and the other one is **Ubuntu 20.04**. The software for setting up webserver has different name in both OS i.e., **httpd** in RedHat 8 and **apache2** in Ubuntu._

<br>

### Part 2 : Variable Files

#### RedHat.yml

```yaml
webserver_package_name: httpd
service: httpd
```

#### Ubuntu.yml

```yaml
webserver_package_name: apache2
service: apache2
```

<br>

### Part 3 : Web Page

#### index.html

```html
Hello, this is {{ ansible_facts['distribution'] }}
```

<br>

### Note:

- _Unlike RedHat 8, root account can’t be logged into separately in case of Ubuntu OS. In order to use root power in Ubuntu OS, sudo password needs to be provided._
- _Also, in the inventory file, general user’s credential needs to be specified for Ansible to access the OS via SSH in case of Ubuntu._
- _Software can’t be installed in Ubuntu OS without root power. In order to solve this issue an option could be provided with the ansible-playbook command as mentioned below:_

```shell
ansible-playbook main.yml --ask-become-pass
```

- _The above command works if the sudo password for both the OS is same._

<br>
<p align="center"><b>. . .</b></p><br>

## Output

![redhat_output](https://miro.medium.com/max/1400/1*UiMchHplF6DREXiwY5WGHw.png)

<p align="center"><b>Web Page Output in RedHat 8</b></p><br><br>

![ubuntu_output](https://miro.medium.com/max/1400/1*f9mqhqkNvBIW6KD6DFHjrA.png)

<p align="center"><b>Web Page Output in Ubuntu 20.04</b></p><br><br>

<br>
<p align="center"><b>. . .</b></p><br>

<h2>Thank You :smiley:<h2>

[![Linkedin](https://i.stack.imgur.com/gVE0j.png) LinkedIn](https://www.linkedin.com/in/satyam-singh-95a266182)


