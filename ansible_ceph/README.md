# About this project

This project will create 3 nodes of a ceph cluster.
It use ceph-deploy tool to do somethings as follows:
  + Create ceph cluster
  + Create ceph mon
  + Create ceph osd

`You have to prepare ceph packages on ceph-node`
If you want to use source.list of ceph, you have to modify `roles/ceph/node/tasks/main.yml` and `roles/admin/tasks/main.yml`
  + Remark roles/ceph/node/tasks/main.yml as follows:
  
  ```
  #- name: Install ntp and ceph package
  #apt:
  #  name: "{{ item }}"
  #  allow_unauthenticated: yes
  #  force: yes
  #  update_cache: yes
  #with_items:
  #  - ntp
  #  - ceph
  #  - ceph-osd
  #  - ceph-mds
  #  - ceph-mon
  #  - radosgw
  ```
  
  + Remove remark roles/admin/tasks/main.yml as follows:
  
  ```
  - name: Install ceph package
    command: ceph-deploy install "{{ nodes[0]['hostname'] }}" "{{ nodes[1]['hostname'] }}" "{{ nodes[2]['hostname'] }}"
    args:
      chdir: /home/localadmin/ceph_cluster
    become_user: "{{ ceph_user }}"
  ```

# Architecture

```
├── hosts
├── nodes-dev
├── playbook.retry
├── playbook.yml
├── README.md
└── roles
    ├── admin
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   └── vars
    │       └── main.yml
    ├── ceph
    │   ├── node
    │   │   ├── tasks
    │   │   │   └── main.yml
    │   │   └── vars
    │   │       └── main.yml
    │   └── osd
    │       ├── tasks
    │       │   └── main.yml
    │       └── vars
    │           └── main.yml
    ├── test
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   └── vars
    │       └── main.yml
    └── util
        ├── common
        │   └── tasks
        │       └── main.yml
        ├── genSSHKey
        │   ├── tasks
        │   │   └── main.yml
        │   └── vars
        │       └── main.yml
        └── hostname
            └── tasks
                └── main.yml

```

# How to use?

+ Modify hosts file
  - CEPH_ADMIN_HOST_IP
  - CEPH1_NODE_HOSTNAME
  - CEPH2_NODE_HOSTNAME
  - CEPH3_NODE_HOSTNAME

```
[ceph-admin]
CEPH_ADMIN_HOST_IP ansible_connection=local

[ceph-node]
CEPH1_NODE_HOSTNAME ansible_connection=ssh ...
CEPH2_NODE_HOSTNAME ansible_connection=ssh ...
CEPH3_NODE_HOSTNAME ansible_connection=ssh ...

...
```

+ Modify nodes-dev file
  - You can change/add some value of variable as follows:
    * hostname
    * ip
    * disk[1-2]: if you add disk3, you have to modify roles/ceph/osd/tasks/main.yml
    * ssd1: if you add ssd2, you have to modify roles/ceph/osd/tasks/main.yml
    * poolSize: osd pool default size
    * pubNetwork: public network (storage network (10Gi))
    * priNetwork: cluster network (sync data network(40Gi))
    * journalSize: osd journal size (partition size of SSD)
    
+ Run ansible script on ceph-admin node

```
ansible-playbook -i hosts playbook.yml
```

    
    
    
  
