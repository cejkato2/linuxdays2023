# Setup using vagrant example:

## Create VM

There are examples for 2 distros: `ubuntu` and `oracle9`

For ubuntu: `cd ubuntu`

For oracle9: `cd oracle9`

```
# in the selected directory with Vagrantfile, create and install VM using:
vagrant up
```

# Connect into VM

`vagrant ssh`

## 1. check if NIC listed as Network devices using DPDK-compatible driver:

```
sudo dpdk-devbind.py -s
```

Expected output:

```
Network devices using DPDK-compatible driver
============================================
0000:00:08.0 '82540EM Gigabit Ethernet Controller 100e' drv=uio_pci_generic unused=e1000,vfio-pci
```

# 2. check DPDK hugepages

```
dpdk-hugepages.py -s
```

Expected output:

```
Node Pages Size Total
0    512   2Mb    1Gb

Hugepages mounted on /dev/hugepages
```

To allocate hugepages:

```
sudo dpdk-hugepages.py -m
sudo dpdk-hugepages.py --setup 1G
```

# 3a. Minimalistic start of ipfixprobe - DPDK:

```
sudo ipfixprobe -i 'dpdk;p=0;e=-a 0000:00:08.0' -p tls -p quic -p dns -o "unirec;i=f:/notebook/linuxdays2023.trapcap;p=(tls,quic,dns)"
```

# 3b. Minimalistic start of ipfixprobe - raw socket with management NIC:

```
sudo ipfixprobe -i 'raw;i=enp0s3' -p tls -p quic -p dns -o "unirec;i=f:/notebook/linuxdays2023.trapcap;p=(tls,quic,dns)"
```

# 3c. IPFIX output

See `ipfixprobe -h output` to get more details. Example to send IPFIX data to flow collect, replace `-o ...` in the previous command:
```
-o "ipfix;h=mycollector.example.com;p=4739"
```

# 4. Example jupyter notebook

```
cd /notebook; python -m notebook --ServerApp.ip=0.0.0.0 --port 8080"
```
