# Xiaomi POCO X3 Pro linux mainline guides

## Troubleshooting

### Mount failed when chrooting

Run this to fix the filesystem
```sh
apt install e2fsprogs
sudo fsck.ext4 -f -y /dev/block/by-name/ubuntu
```

### Boot stuck after "exiting boot services" logs line

Either that you didnt put vmlinuz and dtb files in /boot/EFI or you forgot to rename the dtb file as said in the guide.