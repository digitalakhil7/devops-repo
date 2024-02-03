### nexus.service
```bash
[Unit]
Description=nexus service
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/etc/init.d/nexus start
ExecStop=/etc/init.d/nexus stop
User=nexus
Group=nexus
Restart=on-abort
TimeoutSec=600

[Install]
WantedBy=multi-user.target
```
### nexus.yaml
```yaml
- name: Install, Configure and Start Nexus
  become: yes
  hosts: 10.182.0.8
  gather_facts: yes
  tasks:
  - name: Create directories
    ansible.builtin.file:
      path: "{{item.dest}}"
      state: directory
      mode: 0775
      recurse: true
    loop:
       - {dest: '/opt/jdk8', message: jdk_folder_created}
       - {dest: '/opt/nexus_software', message: nexus_folder_created}
  - name: Download java with custom HTTP headers
    ansible.builtin.get_url:
     url: http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.rpm
     dest: /opt/jdk8
     headers:
        cookie: oraclelicense=accept-securebackup-cookie
  - name: Install jdk8 on Centos
    ansible.builtin.yum:
     name: /opt/jdk8/jdk-8u131-linux-x64.rpm
     state: present
     update_cache: yes
  - name: Download nexus software
    ansible.builtin.get_url:
     url: https://download.sonatype.com/nexus/3/nexus-3.45.0-01-unix.tar.gz
     dest: /opt/nexus_software
  - name: Unarchive a file that is already on the remote machine
    ansible.builtin.unarchive:
     src: /opt/nexus_software/nexus-3.45.0-01-unix.tar.gz
     dest: /opt
     remote_src: yes
  - name: Create nexus user
    ansible.builtin.user:
      name: "{{item.name}}"
      create_home: yes
      state: present
    with_items:
      - {name: nexus, message: nexus_user_created}
  - name: Add nexus user to sudoers file
    ansible.builtin.lineinfile:
     path: /etc/sudoers
     state: present
     regexp: '^%wheel ALL='
     line: 'nexus ALL=(ALL) NOPASSWD: ALL'
     validate: /usr/sbin/visudo -cf %s
  - name: change ownership and permissions
    ansible.builtin.file:
      path: "{{item.perms}}"
      owner: nexus
      group: nexus
      mode: 0775
      recurse: true
    loop:
      - {perms: '/opt/nexus-3.45.0-01', message: ownership_permissions_updated}
      - {perms: '/opt/sonatype-work', message: ownership_permissions_updated}
  - name: Update the nexus.rc file
    ansible.builtin.replace:
     path: /opt/nexus-3.45.0-01/bin/nexus.rc
     regexp: '^#run.*$'
     replace: 'run_as_user="nexus"'
     backup: yes
  - name: Create a symbolic link for nexus service
    ansible.builtin.file:
     src: /opt/nexus-3.45.0-01/bin/nexus
     dest: /etc/init.d/nexus
     owner: nexus
     group: nexus
     state: link
  - name: Copy Startup Service Script to Remote Server
    ansible.builtin.template:
     src: nexus.service
     dest: /etc/systemd/system
  - name: Restart Nexus Service
    ansible.builtin.service:
     name: nexus
     enabled: yes
     state: started
     daemon_reload: true
```
