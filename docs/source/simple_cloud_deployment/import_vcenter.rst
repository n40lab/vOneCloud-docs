.. _import_vcenter:

=======================
Import Existing vCenter
=======================

Importing a vCenter infrastructure into vOneCloud can be carried out easily through the Sunstone Web UI. Follow the next steps to import an existing vCenter cluster as well as any already defined VM Template and Networks.

You will need the IP or hostname of the vCenter server, as well as a user declared as Administrator in vCenter.

Alternatively, in some enterprise environments declaring the user as Administrator is not allowed, in that case, you will need to grant the following permissions to a user depending on what OpenNebula's functionality you want to enable:

+------------------------+---------------------------------------------+---------------------------------------------------+
|   vCenter Operation    |      Privileges required by OpenNebula      |             OpenNebula's functionality            |
+------------------------+---------------------------------------------+---------------------------------------------------+
| CloneVM_Task           | VirtualMachine.Provisioning.DeployTemplate  | Creates a clone of a particular VM                |
+------------------------+---------------------------------------------+---------------------------------------------------+
| ReconfigVM_Task        | VirtualMachine.Interact.DeviceConnection    | Reconfigures a particular virtual machine.        |
|                        | VirtualMachine.Interact.SetCDMedia          |                                                   |
|                        | VirtualMachine.Interact.SetFloppyMedia      |                                                   |
|                        | VirtualMachine.Config.Rename                |                                                   |
|                        | VirtualMachine.Config.Annotation            |                                                   |
|                        | VirtualMachine.Config.AddExistingDisk       |                                                   |
|                        | VirtualMachine.Config.AddNewDisk            |                                                   |
|                        | VirtualMachine.Config.RemoveDisk            |                                                   |
|                        | VirtualMachine.Config.CPUCount              |                                                   |
|                        | VirtualMachine.Config.Memory                |                                                   |
|                        | VirtualMachine.Config.RawDevice             |                                                   |
|                        | VirtualMachine.Config.AddRemoveDevice       |                                                   |
|                        | VirtualMachine.Config.Settings              |                                                   |
|                        | VirtualMachine.Config.AdvancedConfig        |                                                   |
|                        | VirtualMachine.Config.SwapPlacement         |                                                   |
|                        | VirtualMachine.Config.HostUSBDevice         |                                                   |
|                        | VirtualMachine.Config.DiskExtend            |                                                   |
|                        | VirtualMachine.Config.ChangeTracking        |                                                   |
|                        | VirtualMachine.Provisioning.ReadCustSpecs   |                                                   |
|                        | VirtualMachine.Inventory.CreateFromExisting |                                                   |
|                        | VirtualMachine.Inventory.CreateNew          |                                                   |
|                        | VirtualMachine.Inventory.Move               |                                                   |
|                        | VirtualMachine.Inventory.Register           |                                                   |
|                        | VirtualMachine.Inventory.Remove             |                                                   |
|                        | VirtualMachine.Inventory.Unregister         |                                                   |
|                        | DVSwitch.CanUse                             |                                                   |
|                        | DVPortgroup.CanUse                          |                                                   |
|                        | Datastore.AllocateSpace                     |                                                   |
|                        | Datastore.BrowseDatastore                   |                                                   |
|                        | Datastore.LowLevelFileOperations            |                                                   |
|                        | Datastore.RemoveFile                        |                                                   |
|                        | Network.Assign                              |                                                   |
|                        | Resource.AssignVirtualMachineToResourcePool |                                                   |
+------------------------+---------------------------------------------+---------------------------------------------------+
| PowerOnVM_Task         | VirtualMachine.Interact.PowerOn             | Powers on a virtual machine                       |
+------------------------+---------------------------------------------+---------------------------------------------------+
| PowerOffVM_Task        | VirtualMachine.Interact.PowerOff            | Powers off a virtual machine                      |
+------------------------+---------------------------------------------+---------------------------------------------------+
| Destroy_Task           | VirtualMachine.Inventory.Delete             | Deletes a VM (including disks)                    |
+------------------------+---------------------------------------------+---------------------------------------------------+
| SuspendVM_Task         | VirtualMachine.Interact.Suspend             | Suspends a VM                                     |
+------------------------+---------------------------------------------+---------------------------------------------------+
| RebootGuest            | VirtualMachine.Interact.Reset               | Reboots VM's guest Operating System               |
+------------------------+---------------------------------------------+---------------------------------------------------+
| ResetVM_Task           | VirtualMachine.Interact.Reset               | Resets power on a virtual machine                 |
+------------------------+---------------------------------------------+---------------------------------------------------+
| ShutdownGuest          | VirtualMachine.Interact.PowerOff            | Shutdown guest Operating System                   |
+------------------------+---------------------------------------------+---------------------------------------------------+
| CreateSnapshot_Task    | VirtualMachine.State.CreateSnapshot         | Creates a new snapshot of a virtual machine.      |
+------------------------+---------------------------------------------+---------------------------------------------------+
| RemoveSnapshot_Task    | VirtualMachine.State.RemoveSnapshot         | Removes a snapshot form a virtual machine         |
+------------------------+---------------------------------------------+---------------------------------------------------+
| RevertToSnapshot_Task  | VirtualMachine.State.RevertToSnapshot       | Revert a virtual machine to a particular snapshot |
+------------------------+---------------------------------------------+---------------------------------------------------+
| CreateVirtualDisk_Task | Datastore.FileManagement                    | On all VMFS datastores represented by OpenNebula  |
+------------------------+---------------------------------------------+---------------------------------------------------+
| CopyVirtualDisk_Task   | Datastore.FileManagement                    | On all VMFS datastores represented by OpenNebula  |
+------------------------+---------------------------------------------+---------------------------------------------------+
| DeleteVirtualDisk_Task | Datastore.FileManagement                    | On all VMFS datastores represented by OpenNebula  |
+------------------------+---------------------------------------------+---------------------------------------------------+

.. note:: For security reasons, you may define different users to access different ESX Clusters. A different user can be defined in OpenNebula per ESX cluster, which is encapsulated in OpenNebula as an OpenNebula host.

Step 1. Sunstone login
-----------------------

Log in into Sunstone as **CloudAdmin**, as explained in :ref:`the previous section <download_and_deploy>`.

The *CloudAdmin* user comes pre configured and is the **Cloud Administrator**, in full control of all the physical and virtual resources and using the vCenter view. Views will be explained later in its own section.


.. _acquire_resources:

Step 2. Acquire vCenter Resources
---------------------------------

To import new vCenter clusters to be managed in voneCloud, proceed in Sunstone to the ``Infrastructure --> Hosts`` tab and click on the "+" green icon.

.. image:: /images/import_cluster.png
    :align: center

.. warning:: vOneCloud does not support spaces in vCenter cluster names.

In the dialog that pops up, select vCenter as Type in the drop-down. You now need to fill in the data according to the following table:

+--------------+------------------------------------------------------+
| **Hostname** | vCenter hostname (FQDN) or IP address                |
+--------------+------------------------------------------------------+
| **User**     | Username of a vCenter user with administrator rights |
+--------------+------------------------------------------------------+
| **Password** | Password for the above user                          |
+--------------+------------------------------------------------------+

.. image:: /images/vcenter_create.png
    :align: center

.. _import_running_vms:

Now it's time to check that the vCenter import has been successful. In ``Infrastructure --> Hosts`` check if vCenter cluster has been imported, and if all the ESX hosts are available in the ESX tab.

.. note:: Take into account that one vCenter cluster (with all its ESX hosts) will be represented as one vOneCloud host.

.. image:: /images/import_vcenter_esx_view.png
    :align: center

Step 3. Import / Reacquire vCenter Resources
---------------------------------------------------------------------------------

**Existing VMs**

If the vCenter infrastructure has running or powered off **Virtual Machines**, vOneCloud can import and subsequently manage them. To import vCenter VMs, proceed to the  WILDS tab in the Host info tab representing the vCenter cluster where the VMs are running in, select the VMs to be imported and click on the import button.

.. image:: /images/import_wild_vms.png
    :width: 90%
    :align: center

.. _operations_on_running_vms:

After the VMs are in the Running state, you can operate on their life-cycle, assign them to particular users, attach or detach network interfaces, create snapshots, do capacity resizing (change CPU and MEMORY after powering the VMs off), etc.

All the funcionality that vOneCloud supports for regular VMs is present for imported VMs with some exceptions. The following operations *cannot* be performed on an imported VM:

- Recover --recreate
- Undeploy (and Undeploy --hard)
- Migrate (and Migrate --live)
- Stop


Running VMs with open VNC ports are imported with the ability to establish VNC connection to them via vOneCloud. To activate the VNC ports, you need to right click on the VM while it is shut down and click on "Edit Settings", and set the ``remotedisplay.*`` settings shown in the following images.

.. note:: Every VM needs it's own VNC port, so use different values for remotedisplay.vnc.port when importing existing VMs.

.. image:: /images/vm_advanced_settings.png
    :align: center

.. image:: /images/set_vnc_port.png
    :align: center

**VM Templates**

vCenter **VM Templates** can be imported and reacquired using the ``Import`` button in ``Virtual Resources --> Templates``. Fill in the credentials and the IP or hostname of vCenter and click on the "Get Templates" button.

.. image:: /images/import_vcenter_templates.png
    :align: center

.. _operations_on_templates:

These vOneCloud VM templates can be modified selecting the VM Template in ``Virtual Resources --> Templates`` and clicking on the Update button, so the resulting VMs are adjusted to user needs. Among other options available through the Sunstone web interface:

- Information can be passed into the instantiated VM, through either :ref:`Contextualization or Customization <guest_configuration>`.
- Network interface cards can be added to give VMs access to different networks
- Capacity (MEMORY and CPU) can be modified
- VNC capabilities can be disabled

.. _name_prefix_note:

.. note:: VMs instantiated through vOneCloud will be named in vCenter as 'one-<vid>-<VM Name>', where <vid> is the id of the VM and VM Name is the name given to the VM in vOneCloud. This value can be changed using a special attribute set in the vCenter cluster representation in vOneCloud, ie, the vOneCloud host. This attribute is called "VM_PREFIX", and will evaluate one variable, $i, to the id of the VM. A value of "one-$i-" in that parameter would have the same behaviour as the default. This attribute can be set in the "Attributes" section of the vOneCloud host, in the info panel that shows after clicking on the desire host.

.. _vmtemplates_and_networks:

Regarding the vCenter VM Templates and Networks, is important to take into account:

- vCenter **VM Templates with already defined NICs** that reference Networks in vCenter will be imported without this information in vOneCloud. These NICs will be invisible for vOneCloud, and therefore cannot be detached from the Virtual Machines. The imported Templates in vOneCloud can be updated to add NICs from Virtual Networks imported from vCenter (being Networks or Distributed vSwitches).

- We recommend therefore to use **VM Templates in vCenter without defined NICs**, to add them later on in the vOneCloud VM Templates

**Networks**

Similarly, **Networks** and Distributed vSwitches can also be imported / reacquired from using a similar ``Import`` button in ``Infrastructure --> Virtual Networks``.

Virtual Networks can be further refined with the inclusion of different :onedoc:`Address Ranges <operation/network_management/manage_vnets.html#address-space>`. This refinement can be done at import time, defining the size of the network one of the following supported Address Ranges:

- IPv4: Need to define at least starting IP address. MAC address can be defined as well
- IPv6: Can optionally define starting MAC address, GLOBAL PREFIX and ULA PREFIX
- Ethernet: Does not manage IP addresses but rather MAC addresses. If a starting MAC is not provided, vOneCloud will generate one.

The networking information will also be passed onto the VM in the :ref:`Contextualization <build_template_context>` process.

.. _import_images_and_ds:

**Datastores and Images**

Datastores and VMDK images can be imported / reacquired from the ``Storage --> Datastores`` and ``Storage --> Images`` respectively.

Datastore will be monitored for free space and availability. Images can be used for:

- disk attach/detach on VMs
- enrich VM Templates to add additional disks or CDROMs

.. _cluster_prefix:

.. note:: vOneCloud does not support spaces in VMDK and datastores names.

.. note:: Resources imported from vCenter will have their names appended with a the name of the cluster where this resources belong in vCenter, to ease their identification within vOneCloud.

.. note:: vCenter VM Templates, Networks, Distributed vSwitches, Datastores, VMDKs and Virtual Machines can be imported regardless of their position inside VM Folders, since vOneCloud will search recursively for them.

Step 4. Instantiate a VM Template
---------------------------------

Everything is ready! Now vOneCloud is prepared to manage Virtual Machines. In Sunstone, go to ``Virtual Resources --> Templates``, select one of the templates imported in **Step 3** and click on Instantiate. Now you will be able to control the life cycle of the VM.

More information on available operations over VMs :onedoc:`here <operation/vm_management/vm_instances.html>`.
