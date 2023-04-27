# CPE-243_Hands_on_Activity_12.1

- name: Add admin group
  group:
    name: admin
    state: present

- name: Add local user
  user:
    name: admin
    group: admin
    shell: /bin/bash
    home: /home/admin
    create_home: yes
    state: present

- name: Add SSH public key for user
  authorized_key:
    user: admin
    key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
    state: present

- name: Add sudoer rule for local user
  copy:
    dest: /etc/sudoers.d/admin
    src: etc/sudoers.d/admin
    owner: root
    group: root
    mode: 0440
    validate: /usr/sbin/visudo -csf %s
