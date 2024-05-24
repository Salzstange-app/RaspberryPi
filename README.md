---
- name: 'Basic RaspberryPi Infrastructure'
  hosts: local
  tasks:

  - name: 'Update the System'
    become: yes
    become_user: root
    ansible.builtin.apt:
      update_cache: true
      upgrade: 'full'
      
   

  - name: 'Basic Tools '
    become: yes
    become_user: root
    ansible.builtin.apt:
      name:
        - nano
        - git
        - curl

  - name: 'Basic File Configuration'
    file:
      path: /home/apt/
      state: directory
      mode: '0755'

  - name: 'create Test file'
    copy:
      content: "This is a test file to check if the directory is created"
      dest: /home/apt/check_file.txt

    
  - name: 'Install Docker'
    become: yes
    become_user: root
    shell: |
      cd /home/apt/
      sudo apt-get install ca-certificates curl
      sudo install -m 0755 -d /etc/apt/keyrings
      sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
      sudo chmod a+r /etc/apt/keyrings/docker.asc
      echo \ "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \ $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \ sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

      sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
      
