ANSIBLE:
It's a configuration management tool.

configuration: hardware and software properties 
Management: update, deleting, installing & maintaining ...

Ansible is an end-to-end automated tool.

servers -- > configure (jenkins, tomcat & pks) -- > deployed

Manual: server & configuration
automated: Deployment


ansible is an open-source engine used to automate the process.
its platform independent.
ansible doesn't req agents.
dependency: python
here we use playbooks to automate the work.
to work with Ansible we use YAML syntax.

HISTORY:
2012 -- > maichel dehaan -- > ansible -- > owned by rehat -- > 
we have 2 versions 
1. free 
2. paid


ARCHITECTURE:
ANSIBLE SERVER: it will execute playbooks on worker nodes.
PLAYBOOK: its a file which have code. (to install pkg, deployment) 
INVENTORY FILE: the file which is having worker node info.
CONNECTION: ssh


SETUP:
CREATE 5 SERVERS

ALL NODES:
sudo -i
hostnamectl set-hostname ansible/dev-1/dev-2/test-1/test-1
sudo -i

passwd root 
vim /etc/ssh/sshd_config (38: uncomment, 63: no=yes)
systemctl restart sshd
systemctl status sshd
hostname -i

ANSIBLE:
amazon-linux-extras install ansible2 -y
yum install python python-pip python-level -y
vim /etc/ansible/hosts 
Note: user your ips (ADD FROM LINE 12)
[dev]
172.31.1.64
172.31.2.138
[test]
172.31.14.186
172.31.1.69


CONNECTION:
ssh-keygen -- > enter -- > enter -- > enter -- > enter 

ssh-copy-id root@private-ip of dev-1  -- > yes -- > password -- > ssh private-ip of dev-1 -- > ctrl d

ssh-copy-id root@private-ip of dev-2  -- > yes -- > password -- > ssh private-ip of dev-2 -- > ctrl d

ssh-copy-id root@private-ip of test-1  -- > yes -- > password -- > ssh private-ip of test-1 -- > ctrl d

ssh-copy-id root@private-ip of test-2  -- > yes -- > password -- > ssh private-ip of test-2 -- > ctrl d

HISTORY:
 1  passwd root
    2  vim /etc/ssh/sshd_config
    3  systemctl restart sshd
    4  systemctl status sshd
    5  hostname -i
    6  amazon-linux-extras install ansible2 -y
    7  yum install python python-pip python-dlevel -y
    8  vim /etc/ansible/hosts
    9  ssh-keygen
   10  ll -al
   11  ll .ssh
   12  ssh-copy-id root@172.31.1.64
   13  ssh 172.31.1.64
   14  ssh-copy-id root@172.31.2.138
   15  ssh 172.31.2.138
   16  ssh-copy-id root@172.31.14.186
   17  ssh 172.31.14.186
   18  ssh-copy-id root@172.31.14.186
   19  ssh-copy-id root@172.31.1.69
   20  ssh 172.31.1.69
   21  ansible all -m ping
   22  history


========================================================================================

1. ADHOC COMMANDS:
these are simple Linux commands. 
these are used for temp works.
these commands will be over ridden.

ansible all -a "yum install git -y"
ansible all -a "yum install maven -y"
ansible all -a "mvn --version"
ansible all -a "touch file1"
ansible all -a "touch raham.txt"
ansible all -a "ls"
ansible all -a "yum install httpd -y"
ansible all -a "systemctl status httpd"
ansible all -a "systemctl start httpd"
ansible all -a "user add raham"
ansible all -a "cat /etc/passwd"
ansible all -a "yum remove git* maven* httpd* -y"


===================================================================

MODULES:
modules are key-value pairs.
key-value pair is also called a dictionary.

name: raham
course: devops
fee: 8k
batch: 4:30 pm

modules are reusable.
based on the work the module is going to change.

ansible all -a "yum install git -y"
ansible all -m yum -a "name=git state=present" (present=install) 
ansible all -m yum -a "name=maven state=present" (present=install) 
ansible all -m yum -a "name=maven state=latest" (latest=update)
ansible all -m yum -a "name=maven state=absent" (absent=uninstall)
ansible all -m yum -a "name=httpd state=present"
ansible all -m service -a "name=httpd state=started" (started=start)
ansible all -m service -a "name=httpd state=stopped" (stopped=stop)
ansible all -m user -a "name=ashu state=present state=present"
ansible all -m user -a "name=ashu state=present"
ansible all -m copy -a "src=jenkins.sh dest=/root" (note: create file before copy)


Note: we can't use multiple modules same time in the above method
so we use playbooks now.


PLAYBOOKS:
playbooks used to execute multiple modules.
we can reuse the playbook multiple times.
in real time we use a playbook to automate our work.
for deployment, pkg installation ----
here we use key-value pairs.
ansible-playbook will be written on YAML syntax.
YAML = YET ANOTHER MARKUP LANGUAGE
extension for playbook is .yml or .yaml
playbook start with --- and end with ... (opt)


ex:1 

- hosts: all
  tasks:
    - name: install git
      yum: name=git state=present

    - name: install httpd
      yum: name=httpd state=present

    - name: starting httpd
      service: name=htttpd state=started

    - name: create user
      user: name=ananth state=present

TO execute: ansible-playbook raham.yml
sed -i 's/present/absent/g' raham.yml
ansible-playbook raham.yml


25  ansible all -a "yum install git -y"
   26  ansible all -a "yum install maven -y"
   27  ansible all -a "mvn --version"
   28  ansible all -a "ls"
   29  ansible all -a "touch file1"
   30  ansible all -a "ls"
   31  ansible all -a "touch raham.txt"
   32  ansible all -a "ls"
   33  ansible all -a "yum install httpd -y"
   34  ansible all -a "systemctl status httpd"
   35  ansible all -a "systemctl start httpd"
   36  ansible all -a "systemctl status httpd"
   37  ansible all -a "useradd raham"
   38  ansible all -a "cat /etc/passwd"
   39  ansible all -a "yum remove git maven apache2 -y"
   40  ansible all -a "yum remove git maven httpd -y"
   41  ansible all -a "mvn --version"
   42  ansible all -a "httpd --version"
   43  ansible all -a "git --version"
   44  ansible all -a "yum remove git* -y"
   45  ansible all -a "git --version"
   46  ansible all -m yum -a "name=git state=present"
   47  ansible all -a "git --version"
   48  ansible all -m yum -a "name=maven state=present"
   49  ansible all -m yum -a "name=maven state=latest"
   50  ansible all -m yum -a "name=maven state=absent"
   51  ansible all -a "mvn --version"
   52  ansible all -m yum -a "name=httpd state=present"
   53  ansible all -m yum -a "name=httpd state=start"
   54  ansible all -m yum -a "name=httpd state=started"
   55  ansible all -m service -a "name=httpd state=started"
   56  ansible all -m service -a "name=httpd state=stopped"
   57  ansible all -a "systemctl status httpd"
   58  ansible all -m service -a "name=httpd state=started"
   59  ansible all -a "systemctl status httpd"
   60  ansible all -m user -a "name=ashu state=present"
   61  ansible all -a "cat /etc/passwd"
   62  ansible all -m user -a "name=prachi"
   63  ansible all -a "cat /etc/passwd"
   64  ll
   65  vim jenkins.sh
   66  ll
   67  ansible all -m copy -a "src=jenkins.sh dest=/root"
   68  ansible all -a "ls"
   69  ansible all -a "yum remove git* maven* httpd* -y"
   70  vim raham.yml
   71  ansible-playbook raham.yml
   72  vim raham.yml
   73  ansible all -a "yum remove git* maven* httpd* -y"
   74  ansible-playbook raham.yml
   75  cat raham.yml
   76  sed -i 's/present/absent/g' raham.yml
   77  cat raham.yml
   78  ansible-playbook raham.yml
   79  history
====================================================================

Tags: To skip or execute a specific task/tasks in a playbook

- hosts: all
  tasks:
    - name: install git
      yum: name=git state=absent
      tags: a
    - name: install httpd
      yum: name=httpd state=absent
      tags: b
    - name: starting httpd
      service: name=httpd state=started
      tags: c
    - name: create user
      user: name=ananth state=absent
      tags: d

SINGLE TAG EXECUTION: ansible-playbook raham.yml --tags a
MULTI TAG EXECUTION: ansible-playbook raham.yml --tags b,c
SINGLE TAG SKIPPING: ansible-playbook raham.yml --skip-tags "a"
MULTI TAG SKIPPING: ansible-playbook raham.yml --skip-tags "b,c"

NOTE: unistall the package when installation is done
sed -i 's/present/absent/g' raham.yml
ansible-playbook raham.yml

LOOPS:

- hosts: all
  tasks:
    - name: install PKGS
      yum: name={{item}} state=present
      with_items:
        - git
        - maven
        - docker
        - tree
        - java-1.8.0-openjdk
        - httpd
ansible-playbook raham.yml
sed -i 's/present/absent/g' raham.yml
ansible-playbook raham.yml

- hosts: all
  tasks:
    - name: install PKGS
      user: name={{item}} state=absent
      with_items:
        - sravanthi
        - kunal
        - nayana
        - prateeksha
        - vijay
        - revi

ansible-playbook raham.yml
sed -i 's/present/absent/g' raham.yml
ansible-playbook raham.yml


SHELL VS COMMAND VS RAW:

- hosts: all
  tasks:
    - name: install git
      shell: yum install git -y

    - name: install java-1.8.0
      command: yum install java-1.8.0-openjdk -y

    - name: install apache
      raw: yum install httpd -y

raw >> command >> shell
ansible-playbook raham.yml
sed -i 's/install/remove/g' raham.yml
ansible-playbook raham.yml

CONDITIONS:
CLUSTER: Group of servers
HOMOGENIUS: all servers have having same OS and flavour.
HETROGENIUS: all servers have different OS and flavour.

used to execute this module when we have different Clusters.

RedHat=yum
Ubuntu=apt

- hosts: all
  tasks:
    - name: installing git on RedHat
      yum: name=git state=present
      when: ansible_os_family == "RedHat"
    - name: installing git on Debian
      apt: name=git state=present
      when: ansible_os_family == "Debian"

SETUP MODULE: to show complete info of worker nodes.
ansible all -m setup 
ansible all -m setup | grep -i family
ansible all -m setup | grep -i mem
ansible all -m setup | grep -i cpus

HISTORY:
 80  vim raham.yml
   81  ansible-playbook raham.yml --tags d
   82  cat raham.yml
   83  ansible-playbook raham.yml --tags a,b
   84  cat raham.yml
   85  sed -i 's/absent/present/g' raham.yml
   86  cat raham.yml
   87* ansible-playbook raham.yml --skip-tags "c,d"
   88  sed -i 's/present/absent/g' raham.yml
   89  ansible-playbook raham.yml
   90  vim raham.yml
   91  ansible-playbook raham.yml
   92  ansible all -a "git --version"
   93  ansible all -a "mvn --version"
   94  ansible all -a "docker --version"
   95  ansible all -a "tree --version"
   96  ansible all -a "java -version"
   97  ansible all -a "update-alternatives --config java"
   98  ansible all -a "httpd -v"
   99  sed -i 's/present/absent/g' raham.yml
  100  ansible-playbook raham.yml
  101  ansible all -a "httpd -v"
  102  ansible all -a "java -version"
  103  ansible all -a "tree --version"
  104  ansible all -a "docker --version"
  105  ansible all -a "yum remove git* java* -y"
  106  ansible all -a "java -version"
  107  ansible all -a "git --version"
  108  vim raham.yml
  109  ansible-playbook raham.yml
  110  ansible-playbook "cat /etc/passwd"
  111  ansible all -a "cat /etc/passwd"
  112  cat raham.yml
  113  vim raham.yml
  114  ansible-playbook raham.yml
  115  ansible all -a "cat /etc/passwd"
  116  sed -i 's/present/absent/g' raham.yml
  117  ansible-playbook raham.yml
  118  vim raham.yml
  119  ansible-playbook raham.yml
  120  ansible all -a "java -version"
  121  ansible all -a "git --version"
  122  ansible all -a "httpd --version"
  123  sed -i 's/present/absent/g' raham.yml
  124  ansible-playbook raham.yml
  125  vim raham.yml
  126  cat raham.yml
  127  sed -i 's/present/absent/g' raham.yml
  128  cat raham.yml
  129  sed -i 's/install/remove/g' raham.yml
  130  cat raham.yml
  131  ansible-playbook raham.yml
  132  vim raham.yml
  133  ansible-playbook raham.yml
  134  vim raham.yml
  135  ansible-playbook raham.yml
  136  cat raham.yml
  137  ansible all -a "cat /etc/os-release"
  138  ansible setup -m all
  139  ansible all -m setup
  140  ansible all -m setup | grep -i family
  141  ansible all -m setup | grep -i cpus
  142  ansible all -m setup | grep -i memory
  143  ansible all -m setup | grep -i mem
  144  history

===========================================



HANDLERS:
in handlers, one task is dependent on another task
here task-2(restarting httpd) depends on task-1(installing httpd)
once task-1 is executed then it will notify to task-2 to perform the action.


- hosts: all
  tasks:
    - name: installing httpd
      yum: name=httpd state=present
      notify: starting httpd

  handlers:
    - name: starting httpd
      service: name=httpd state=started


ansible-playbook raham.yml 
sed -i 's/present/absent/g' raham.yml
ansible-playbook raham.yml


ROLES:
roles is a way of organizing playbooks in a structured format.
main purpose of roles is to encapsulate the data.
we can reuse the roles multiple times.
length of the playbook is decreased.
it contains on vars, templates, task -----
in real time we use roles for our daily activities.

 mkdir playbooks
cd playbooks/

mkdir -p roles/pkgs/tasks
vim roles/pkgs/tasks/main.yml

- name: installing pkgs
  yum: name=git state=present
- name: install maven
  yum: name=maven state=present
- name: installing httpd
  yum: name=httpd state=present

mkdir -p roles/users/tasks
vim roles/users/tasks/main.yml

- name: create users
  user: name={{item}} state=present
  with_items:
    - uday
    - naveen
    - rohit
    - lokesh
    - saipallavi
    - supriya

mkdir -p roles/abc/tasks
vim roles/abc/tasks/main.yml

- name: installing httpd
  yum: name=httpd state=present

- name: starting httpd
  service: name=httpd state=started

.
├── master.yml
└── roles
    ├── abc
    │   └── tasks
    │       └── main.yml
    ├── pkgs
    │   └── tasks
    │       └── main.yml
    └── users
        └── tasks
            └── main.yml

COLORS:
yellow	: success
green	: already done
red	: failed
blue	: skipped

ansible_nodename
ansible_os_family
ansible_processor_vcpus
block_available
ansible_memtotal_mb
ansible_memfree_mb


Debug: used to print the msgs/customize ops

- hosts: all
  tasks:
    - name: to pring mgs
      debug:
        msg: "the node name os: {{ansible_hostname}}, the total mem is: {{ansible_memtotal_mb}}, free mem is {{ansible_memfree_mb}}, the flavour of ec2 is: {{ansible_os_family}}, toto cpus is: {{ansible_processor_vcpus}}"


JINJA2 TEMPLATE: used to get the customized op, here its a text file which can extract the variables and these values will change as per time.

LOOKUPS: this module used to get data from files, db and key values

- hosts: dev
  vars:
    a: "{{lookup('file', '/root/creds.txt') }}"
  tasks:
    - debug:
        msg: "hai my user name is {{a}}"

cat creds.txt
user=raham


:
