# Dynamic Computer Resource Management Thesis

## Current VM Hardware
- **X** core of Intel Xeon
- 4 GB of RAM

## Current VM Software
- XenServer 6.5
- XenCenter (Windows only)
- XenTools

## Technique being Used
### Virtual CPU Management
1. Specifying specific vCPU to specific VM
The virtual CPU (vCPU) behaviour can be modified by altering the vCPUs-params parameter of a VM. vCPU pinning is the term for mapping vCPUs of a VM to specific physical resources. Run the following command to tune a vCPU's pinning:
**`[root@xenserver ~]# xe vm-param-set uuid=<VM UUID> VCPUs-params:mask=1,3,7`**
The VM in the preceding example runs on physical CPUs 1, 3, and 7 only.

2. vCPU priority weight
The VCPU priority weight parameters can also be modified to grant a specific VM more CPU time than others. Run the following command:
**`[root@xenserver ~]# xe vm-param-set uuid=<VM UUID> VCPUs-params:weight=512`**
The VM in the preceding example with a weight of 512 gets twice as much CPU as a domain with a weight of 256 on a busy XenServer Host where all CPU resources are in use. Valid weights range is from 1 to 65535 and the default is 256.

3. vCPU cap
The CPU cap optionally fixes the maximum amount of CPU a VM can use. Run the following command:
**`[root@xenserver ~]# xe vm-param-set uuid=<VM UUID> VCPUs-params:cap=80`**
The VM in the preceding example with a cap of 80 is able to use only 80 per cent of one physical CPU even if the XenServer Host has idle CPU cycles.
The cap is expressed in percentage of one physical CPU: 100 is one physical CPU, 50 is half a CPU, 400 is 4 CPUs, and so on. The default, 0 (zero), indicates there is no upper cap.

### Memory Management
1. Static
Specifying spesific amount of memory available to spesific VM

2. Dynamic
Specifying minimum and maximum amount of RAM to spesific VM

## Limitation
- This research only researching resource allocation in Linux
- XenServer doesn't support Memory Overcommitment (Can't initialize VM with larger memory than the physical memory)

### Experiment Setup

#### First Experiment
This experiment compare performance between Dynamic Memory and Static Memory in Ubuntu Server 15.04

| No | Name  | Resource Allocation | vCPU | Topology | Memory | OS |
| -  | ----- | ------------------- | ---- | -------- | ------ | -- |
| 1  |  VM1  | Dynamic Memory, Static CPU | 1 | 1 socket, 1 core | 256 MB - 512 MB | Ubuntu Server 15.04 |
| 2  |  VM2  | Static Memory, Static CPU | 1 | 1 socket, 1 core | 256 MB - 512 MB | Ubuntu Server 15.04 |

#### Second Experiment
This experiment compare performance between Dynamic Memory and Static Memory in Lubuntu Desktop 15.04

| No | Name  | Resource Allocation | vCPU | Topology | Memory | OS |
| -  | ----- | ------------------- | ---- | -------- | ------ | -- |
| 1  |  VM3  | Dynamic Memory, Static CPU | 1 | 1 socket, 1 core | 256 MB - 512 MB | Ubuntu Server 15.04 |
| 2  |  VM4  | Static Memory, Static CPU | 1 | 1 socket, 1 core | 256 MB - 512 MB | Ubuntu Server 15.04 |


## Current Issues
- Can't find other alternative to static and dynamic memory management
- Ubuntu take **300 MB** on idle startup and take around **450 MB** on idle after startup

## Developement Progress
- [x] Successfully enable dynamic memory management
- [x] Successfully configure a VM using vCPU management system
- [] Conduct experiment about Dynamic vs Static Memory