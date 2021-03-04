# kubernetes_multipass

```
multipass launch --cpus 2 --mem 4G --disk 15G --name master --cloud-init cloudinit.yml  && multipass launch --cpus 2 --mem 2G --disk 15G --name worker1 --cloud-init cloudinit.yml  && multipass launch --cpus 2 --mem 2G --disk 15G --name worker2 --cloud-init cloudinit.yml && echo "[k8s]" > invent.ini && multipass list | awk '{ print $3 }' | sed '1d'  >> invent.ini && echo "[k8s-master]" >>  invent.ini && multipass list | grep master | awk '{ print $3 }' >> invent.ini && echo "[k8s-worker]" >> invent.ini && multipass list | grep worker | awk '{ print $3 }' >> invent.ini && export ANSIBLE_HOST_KEY_CHECKING=False && ansible-playbook -u ubuntu --private-key xoa_rsa -i invent.ini prod.yml
```

Thanks Mateus for the help:

https://github.com/mateusmuller/k8s-cluster-spinup
