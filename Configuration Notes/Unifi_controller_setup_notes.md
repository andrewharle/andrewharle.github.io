#Setup the encrypted discs
#Note that the xts cipher doesn't seem to work on rk_crypto module. Rk_crypto max key size is 256 bits.

sudo cryptsetup -v --type luks2 --cipher aes-cbc-plain --key-size 256 --hash sha512 --iter-time 10000 --use-random --verify-passphrase luksFormat /dev/sd#
Sudo cryptsetup luksOpen /dev/sd# LUKS_sd# 
Sudo cryptsetup -v status LUKS_sd# 

pv -tpreb /dev/zero | sudo dd of=/dev/mapper/LUKS_sd# bs=128M 

sudo mkfs.ext4 /dev/mapper/LUKS_sd#

#Close luks
sudo cryptsetup close /dev/mapper/LUKS_sd#