# Xiaomi POCO X3 Pro linux mainline guides

## Partitioning guide
This is a temporary page until a recovery with a partitioning script appears; Because we have to count for people having windows so currently no guide is supplied ~~because I am too lazy to write one~~

Currently what you will need to run Ubuntu (only supported distro currently) is:

- A partition called "ubuntu" with ext4 filesystem; Which will store our rootfs. (Minimum 6GB of space is required)

- A partition called "esp" with fat32 filesystem. You can share the windows esp with mainline one, We will store our kernel images in this partition. (Minimum 50MB of free space is required)

If you have those, you can proceed with the guide. 
