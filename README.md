# Homelab

## Proxmox

- ISO images stored in `/var/lib/vz/template/iso`

### Setup

1. Install Proxmox on R710
2. Update

   ```shell
   apt update
   apt upgrade -y
   ```

3. Install utilities

   ```shell
   apt install vim fish exa fd-find
   chsh -s /usr/bin/fish
   ```

   ```fish
   # ~/.config/fish/config.fish

   alias rm='rm -i'
   alias cp='cp -i'
   alias mv='mv -i'

   alias fd='fdfind'
   alias ls='exa'
   ```

   ```vimrc
   set hlsearch    " highlight all search results
   set ignorecase  " do case insensitive search 
   set incsearch   " show incremental search results as you type
   set number      " display line number
   set noswapfile  " disable swap file
   ```

4. Set up PCI pass through for RAID controller (<https://pve.proxmox.com/wiki/Pci_passthrough>)
   1. Make sure drives are each set up as independent RAID0 configurations in RAID
      controller
   2. Add `intel_iommu=on` to `GRUB_CMDLINE_LINUX_DEFAULT` in `/etc/default/grub`
      (space delimited) and then run `update-grub`
   3. Edit `/etc/modules` and add

      ```
      vfio
      vfio_iommu_type1
      vfio_pci
      vfio_virqfd
      ```

   4. Allow unsafe interrupt remapping

      ```shell
      echo "options vfio_iommu_type1 allow_unsafe_interrupts=1" > /etc/modprobe.d/iommu_unsafe_interrupts.conf
      ```

   5. Reboot
