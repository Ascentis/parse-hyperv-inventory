# parse-hyperv-inventory
Small application to parse hyper-v reports inventory and produce an Excel compatible csv file

This application takes as input a file with an inventory report produced by Get-HyperVInventory.ps1 PowerShell script and converts into an Excel compatible .csv file.

## Usage

ParseHyperVReport [options]

**Options:**

**--source** Specifies the filename of the hyperV report file

**--record-pattern** String specifying how a given text line should begin to be considered the marker
of a new record

**--attributes** An array of strings containing the list of attributes to extract from the report
file. Each entry can be a simple string matching the text before the colon (:) separating the value or a name=regex spec
ifying the name of the attribute and a regex specifying how to extra the value in from the right side of the colon marker. If using a regex it must contain at last one capturing group matching the value attempting to extract. The capturing group number 1 is the group used to extract the value (capturing group zero is the entire match)

**--version** Show version information

**-?, -h, --help** Show help and usage information

## Example input file contents

```
### Hyper-V Environment Inventory ###
## Report mode: VM Inventory, Cluster and Hosts ##
Created on: Wednesday, January 6, 2021 4:49:34 AM
Created by: domain\user
Local server: ServerName
Script folder: C:\tools
Script version: <a href="https://gallery.technet.microsoft.com/Get-HyperVInventory-Create-2c368c50" target="_blank">v2.4</a>

### Virtual Machine information ###
## VMs in cluster DevCluster02 ##
Number of VMs in cluster: 138


# VM: CLOUDDC2 #
Clustered VM: True
Cluster group: CLOUDDC2
Cluster startup priority: 2000
Host: HostName
State: Running
Status: Operating normally
VM ID: 859b5aad-9b80-4ca2-a59c-8ad6ec0b5bbb
Generation: 2
Version: 9.0
Created on: 06/20/2019 22:47:23
Guest FQDN: CLOUDDC2.domain.com
Guest OS: Windows Server 2019 Standard
Integration Services version: 10.0.17763
Integration Services state: 
Automatic stop action: Save
Automatic start action: Nothing
Automatic start delay: 0
Configuration path: C:\ClusterStorage\MGR-Volume\VMs\CLOUDDC2
Checkpoint path: C:\ClusterStorage\MGR-Volume\VMs\CLOUDDC2
Current checkpoint type: Production
Replication: not configured
VMconnect.exe access granted to: nobody

 Checkpoints of CLOUDDC2 
  none

 VM Security 
Shielded VM: False
TPM Enabled: False
Key Storage Drive enabled: False
State and Migration encrypted: False

 Virtual hardware 
Number of CPUs: 4
Compatibility for older operating systems enabled: False
Compatibility for migration enabled: False
Host Resource Protection enabled: False
Nested virtualization enabled: False

RAM type: Static Memory
RAM: 8192 MB

...
```

## Usage example

```
ParseHyperVReport --source ..\..\..\Hyper-V-Inventory-20210106-045035.txt --record-pattern "# VM" --attributes "# VM=(.*) #" "State" "Number of CPUs" > hyperv-report.csv
```

This command produces the following output:

```
# VM=(.*) #,State,Number of CPUs
CLOUDDC2,Running,4
vm1,Off,8
vm2,Running,16
vm3,Off,8
usr-vm4,Off,4
usr-vm5,Off,4
usr-vm6,Running,8
usr-vm7,Running,8
...
```


