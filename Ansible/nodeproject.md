## Manual Steps
Pre-requisites: Node Project ( package.json, server.js ) in tar format ex: node-ex.tar
```bash
sudo tar cf node-ex.tar node-ex
sudo tar xf node-ex.tar
```
1. Install npm and nodejs
2. Copy and unarchive node project from controller to node ( unarchive module)
3. Install Dependencies
4. Run Node server ( npm server.js )
5. Verify if App is running ( ps aux | grep node )
## nodeapp.yaml
```yaml
---
- name: Node App Deployment
  hosts: 10.182.0.6
  become: yes
  gather_facts: no
  tasks:
  - name: Install node and npm
    yum:
      name: ['npm','nodejs']
      state: present
      update_cache: yes
  - name: Untar the project to node
    unarchive:
      src: /opt/node-ex.tar
      dest: /home/ansible/
  - name: Install Dependencies
    npm:
      path: /home/ansible/node-ex
  - name: Start the server
    command: node /home/ansible/node-ex/server
    async: 10
    poll: 0
  - name: Verify App is running
    shell: ps aux | grep  node
    register: myappstatus
  - debug: msg={{myappstatus.stdout_lines}}
```
### package.json
```bash
{
    "name": "Nodejs project",
    "scripts": {
        "start": "node server.js"
    },
    "engines": {
        "node": "10.x.x"
    }
}
```
### server.js
```bash
var http = require('http');
var handleRequest = function(request, response) {
    response.writeHead(200);
    response.end("Installing and deploying node through ansible")
}
var www = http.createServer(handleRequest);
www.listen(8080)
```
