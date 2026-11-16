# Day 1

## Info - Hypervisor Overview
<pre>
- Hypervisor is nothing but Virtualization
- Virtualization helps us run multiple OS on your laptop/desktop/workstation/server
- i.e more than 1 OS can run side-by-side on your machine
- there are 2 types of Hypervisors
  1. Type 1 - a.k.a Bare Metal Hypervisors
     - is used in Servers & Workstations
     - near native-hardware performance
     - examples
       - KVM ( supported in all Linux distributions, free & opensource )
       - VMWare vSphere(vCenter)
       - Microsoft Hyper-V
       
  2. Type 2 - a.k.a Hosted Hypervisors
     - is used in Laptops/Desktops & Workstations
     - slightly slower
     - examples
       - Oracle VirtualBox ( Free - Supports Windows, Linux & Mac )
       - VMWare Workstation ( Free - Supports Windows & Linux )
       - VMWare Fusion ( Supports only Mac and requires a commercial license )
       - Parallels ( Supports only Mac and requires a commercial license )
- each OS that runs on top Hypervisor is called a Guest OS
- Guest OS requires it own Virtual Machine (VM)
- each Virtual Machine must be allocated with dedicated Hardware resources
  - CPU Cores ( Actual )
  - RAM ( Actual )
  - Storage ( Actual )
  - Network Card ( Virtual )
  - Graphics Card ( Virtual )
- Because every VM requires dedicated H/w resources, this type of Virtualization is called heavy-weight
</pre>



