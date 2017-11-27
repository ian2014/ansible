# How to use?

+ Modify hosts file

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

    
    
    
  
