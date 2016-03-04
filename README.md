this is set of scripts to easily backup/clone/migrate machiens in xen/libvirt technology stack

requirements:
xen over 4 versions 
libvirtd over 0.10
virsh over 0.10
virt-clone 0.6 and up
root access or sudo access for mount/umount and calls to virsh and virt-clone


usage :

Prepare config 

Suppose that machine already exists and is up and want to be backued up.
set parameters for domU in config  (parameters like vm_name ,stay_online, stay_started ) and  generate config for dd or tar-fs backup via script generate_disk_configs.
This step will prepare config files for dd and for filebased backup named as parameters disk_list_for_dd/disk_list_for_tar in config.
Content of these files will  be list of disks which domU uses.
Change if needed to specify which disks goes to backup.


to start dd backup of disks  do
./make_dd_backup

to start filesystem backup (if applicable)  do

./make_tar_backup_of_fs

to send backup to remote locaion 

./send_backup_to_remote

to clone machine to vm_name_suffix do

./clone_machine -a suffix

to create lvm create scripts for disks :

./generate_lvm_create_script
result : generate_lvm_disk_script which can be used to generate exact lvm disks for domU

to restore image made wirh make_dd_backup use :

./restore_dd_image  -i <mage_file> -l <lvm_path>

to migrate machine from one host to other use :

make lvm on destination machine with script created with  ./generate_lvm_create_script (named generate_lvm_disk_script)
for one  lvm  disk start command  on receiver :
./restore_dd_image_via_nc_receiver -p <port> -l <lvm_path>
on sender 
./restore_dd_image_via_nc_sender -i <ip_address> -p <port> -l <lvm_path>
define machine with virsh define afterwards on destination.




Limitations:

-for tar backup of filesystem , if domU use lvm inside , script will not work . Not implemented yet. Only can be used with partitions set with parameter part_number in config. 
