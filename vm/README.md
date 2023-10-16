# Setup using ubuntu example:

## Create VM

```
cd ubuntu

# create and install VM
vagrant up
```

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


