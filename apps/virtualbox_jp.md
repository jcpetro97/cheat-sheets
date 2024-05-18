# Virtualbox

Virtualbox is a free virtualization platform from Oracle which allow you to run virtual machines locally.

---

####  Install VirtualBox Extension Pack using command line

1. cd to the directory where you downloaded the extension pack to.
2. using the **`VBoxManage`** tool, install the extension pack with **`sudo`** privileges

```bash
sudo VBoxManage extpack install --replace <filename>
```

3. verify that the extension pack installed correctly

```bash
VBoxManage list extpacks
```


#### List all virtual machines

```bash
VBoxManage list vms
```

#### List all virtual machines ( detailed )

```bash
VBoxManage list --long vms
```

#### List all running virtual machines

```bash
VBoxManage list runningvms
```

#### Reset virtual machine

```bash
VBoxManage controlvm <vmname> reset
```

#### Power off  virtual machine

```bash
VBoxManage controlvm <vmname> poweroff
```

#### Power on virtual machine ( Headless )

```bash
VBoxManage startvm <vmname> --type headless
```

#### Take Snapshot

```bash
vboxmanage snapshot <vmname> take "description"
```

#### List all snapshots for a specific virtual machine

```bash
VBoxManage snapshot <vmname> list
```

#### Restore Snapshot

```bash
vboxmanage snapshot <vmname> restore <snapshot name>
```

#### Delete Snapshot

Note:  get either the **Name** or **UUID** of the snapshot first using the snapshot list command.

```bash
vboxmanage snapshot <guest name> delete <UUID|Name>
```

#### Delete Virtual Machine and delete files

```bash
vboxmanage unregistervm <vmname> --delete
```

#### Delete Virtual Machine and **Keep** files

```bash
vboxmanage unregistervm <vmname>
```

#### Clone Virtual Machine

```bash
vboxmanage clonevm <vmname> --options keepdisknames --snapshot <name> --name <newname> --basefolder <folder location> --register
```



#### If the disk doesn't get removed

If you get an error when trying to build a box, that a disk already exists, try the following:

`cat ~/.config/VirtualBox/VirtualBox.xml | grep <name of the old host>

Grab the output of the UUID.

`$ vboxmanage closemedium disk "{d89ef84a-d754-4da2-b2a1-cc37063d0c6d}" --delete`

**NOTE:** make sure to use the "curly bracket"