fix-e1000e.service
# sytemd service to disable tso, gso, gro on boot, to resolve Intel nic e1000e hardware unit hang causing proxmox nodes to go offline.
# to use, create the .service file under /etc/systemd/system/
# then run: 
# systemctl daemon-reload
# systemctl enable --now fix-e1000e.service
# to check if it is working:
# ethtool -k <intel_nic> | grep -E "tcp-segmentation|generic-segmentation|generic-receive"
# output should look like: 
# tcp-segmentation-offload: off
#        tx-tcp-segmentation: off
# generic-segmentation-offload: off
# generic-receive-offload: off
# this confirms that tso, gso, gro have been turned off
# further reading
# proxmox ve helper script:
# https://community-scripts.org/scripts/nic-offloading-fix?id=nic-offloading-fix
# issue on proxmox community:
# https://forum.proxmox.com/threads/e1000e-eno1-detected-hardware-unit-hang.59928/
# further fixes for intel nic
# if the issue continues, may be helpful to set the following as well, esp if on a home laptop
# in /etc/default/grub set:
# GRUB_CMDLINE_LINUX_DEFAULT="quiet pcie_aspm=off e1000e.SmartPowerDownEnable=0 e1000e.EEE=0"
# run:
# update-grub
# reboot
# keep in mind those are power mgmt settings w/ pcie being global, other two nic specific
