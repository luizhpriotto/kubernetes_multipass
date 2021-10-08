# kubernetes_multipass

```
multipass launch --cpus 2 --mem 4G --disk 15G --name master --cloud-init cloudinit.yml  && multipass launch --cpus 2 --mem 2G --disk 15G --name worker1 --cloud-init cloudinit.yml  && multipass launch --cpus 2 --mem 2G --disk 15G --name worker2 --cloud-init cloudinit.yml && echo "[k8s]" > invent.ini && multipass list | awk '{ print $3 }' | sed '1d'  >> invent.ini && echo "[k8s-master]" >>  invent.ini && multipass list | grep master | awk '{ print $3 }' >> invent.ini && echo "[k8s-worker]" >> invent.ini && multipass list | grep worker | awk '{ print $3 }' >> invent.ini && export ANSIBLE_HOST_KEY_CHECKING=False && ansible-playbook -u ubuntu --private-key xoa_rsa -i invent.ini prod.yml
```
```
multipass launch --cpus 2 --mem 4G --disk 15G --name master --cloud-init cloudinit.yml  && \
multipass launch --cpus 2 --mem 2G --disk 15G --name worker1 --cloud-init cloudinit.yml  && \
multipass launch --cpus 2 --mem 2G --disk 15G --name worker2 --cloud-init cloudinit.yml && \
echo "[k8s]" > invent.ini && multipass list | awk '{ print $3 }' | sed '1d'  >> invent.ini && \
echo "[k8s-master]" >>  invent.ini && multipass list | grep master | awk '{ print $3 }' >> invent.ini && \ 
echo "[k8s-worker]" >> invent.ini && multipass list | grep worker | awk '{ print $3 }' >> invent.ini && \ 
export ANSIBLE_HOST_KEY_CHECKING=False && ansible-playbook -u ubuntu --private-key ubuntu -i invent.ini docker.yml && \
export MASTER=$(cat invent.ini | grep k8s-master -A 1 | tail -n 1) && \
export WORKER1=$(cat invent.ini | grep k8s-worker -A 1 | tail -n 1) && \
export WORKER2=$(cat invent.ini | grep k8s-worker -A 2 | tail -n 1) && \
sed -e "s/\${master}/$MASTER/" -e "s/\${worker1}/$WORKER1/" -e "s/\${worker2}/$WORKER2/" template.yml > cluster.yml && \
./rke_linux-amd64 up
```
Thanks Mateus for the help:

https://github.com/mateusmuller/k8s-cluster-spinup
