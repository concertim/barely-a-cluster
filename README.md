# Barely a cluster

Login node and 2 compute nodes on OpenStack. Hardcoded /etc/hosts for DNS. NFS share of /home and /data. 

No IPA, no scheduler, no concertim-specific tooling.

## Launch 

Create (make sure `mycluster1` hasn't already been used as stack name):
```
openstack stack create -t cluster.yaml --parameter clustername="mycluster1" "mycluster1" --wait
```
Note: Garycloud isn't fast, it will take around 5 mins for systems to boot and be accessible. Check progress in the GUI (https://openstack.garycloud.alces.network/project/instances/) with the console view of the login node

Get external IP for connecting to login1:
```
openstack server show login1.mycluster1.cluster.network -c addresses
```

Login to the 10.151 address with user rocky and openflight shared key

To login to compute node either copy the openflight shared private key to the rocky user or use ssh proxyjumping (because shared home but no shared SSH with rocky user until one is created manually or via user suite):
```
ssh -J rocky@10.151 rocky@10.10.1.1 # node01
ssh -J rocky@10.151 rocky@10.10.1.2 # node02
```

## Destroy

Delete: 
```
openstack stack delete 'mycluster1'
```

## Troubleshooting

To see if something has gone wrong with initial setup check `/root/setup.log` and `/var/log/cloud-init-output.log` on all systems.
