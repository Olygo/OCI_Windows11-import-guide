# BRING WINDOWS 11 ON OCI


## Disclaimer

This document provides an example procedure for importing, configuring, and using **Windows 11 on OCI** solely for informational and educational purposes. It demonstrates technical feasibility but does not constitute official guidance, endorsement, or support from Oracle Corporation or its affiliates.

## No Official Support

Oracle does not support running Windows 11 on OCI. This setup is unofficial, untested by Oracle, and may violate Microsoft's Windows licensing terms or OCI service policies. Any issues, including performance degradation, security vulnerabilities, data loss, or service disruptions, will not be addressed by Oracle support. Users must rely on community resources or their own troubleshooting.

The only official and fully supported method for **running Windows 11 on OCI** is through [OCI Secure Desktop](https://docs.oracle.com/en-us/iaas/secure-desktops/home.htm)

## User Responsibility

Implementing this procedure is entirely at your own risk and under your sole responsibility. You assume all liability for any consequences, such as system instability, compliance violations, financial losses, or legal repercussions. Oracle disclaims all warranties, express or implied, including fitness for a particular purpose.

## Recommendations

Verify compliance with applicable laws, licenses (e.g., Microsoft Volume Licensing), and OCI services.

Use only for non-production, testing environments.

Back up data regularly and test thoroughly before any critical use.

Consult legal and IT experts if needed.

***This example is provided "as is" without guarantees of functionality or future compatibility. Changes in OCI, Windows updates, or hardware emulation may render it obsolete. Proceed with caution.***


## Introduction to the Procedure

This step-by-step guide demonstrates how to import, configure, and run Windows 11 on OCI using a virtual machine initially created on VMware Workstation. It covers every action from VM creation in VMware Workstation through to successful boot-up in OCI.

## Scope of the Guide

The procedure starts with setting up a new VM in VMware Workstation Pro/Player, including hardware configuration (CPU, RAM, disk, network), OS installation, Windows 11 activation, driver installation, and preparation for export. It then details the VMDK export, upload to OCI Object Storage, import as a custom image, instance launch, setup and final boot with RDP access.

Each step includes screenshots and commands.

## Step by Step

![00](./.images/00.png)
![01](./.images/01.png)
![02](./.images/02.png)
![03](./.images/03.png)
![04](./.images/04.png)

### ENABLE TPM ENCRYPTION
![05](./.images/05.png)

### UEFI IS MANDATORY, SECURE BOOT IS OPTIONAL

![06](./.images/06.png)

![07](./.images/07.png)

![08](./.images/08.png)

![09](./.images/09.png)

![10](./.images/10.png)

![11](./.images/11.png)

![12](./.images/12.png)

### USING A SINGLE FILE IS MANDATORY TO IMPORT A CUSTOM IMAGE

MAX SIZE : **400GB**

![13](./.images/13.png)

![14](./.images/14.png)

![15](./.images/15.png)

### PRESS ENTER TO START SETUP

![16](./.images/16.png)

![17](./.images/17.png)

![18](./.images/18.png)

![19](./.images/19.png)

### SELECT YOUR WINDOWS EDITION

![20](./.images/20.png)

![21](./.images/21.png)

![22](./.images/22.png)

### MAXIMUM DISK SIZE TO IMPORT A CUSTOM IMAGE is 400GB

![23](./.images/23.png)

![24](./.images/24.png)

![25](./.images/25.png)

![26](./.images/26.png)

![27](./.images/27.png)

![28](./.images/28.png)

![29](./.images/29.png)

![30](./.images/30.png)

### BYPASS ONLINE ACCOUNT CREATION 

PRESS ***SHIFT*** + ***F10*** TO OPEN A TERMINAL

```
start ms-cxh:localonly
```

![31](./.images/31.png)

![32](./.images/32.png)

![33](./.images/33.png)

### APPLY ALL WINDOWS UPDATES

![34](./.images/34.png)

### ENABLE REMOTE DESKTOP

![35](./.images/35.png)

![36](./.images/36.png)

### ADD YOUR LOCAL ACCOUNT AS REMOTE DESKTOP USER

![37](./.images/37.png)

![38](./.images/38.png)

### CHECK IF RDP TCP 3389 IS ALLOWED IN WINDOWS DEFENDER

![39](./.images/39.png)

### DOWNLOAD VIRTIO DRIVERS FOR WINDOWS

[Downloading the Oracle VirtIO Drivers for Microsoft Windows](https://docs.oracle.com/en/operating-systems/oracle-linux/kvm-virtio/kvm-virtio-DownloadingtheOracleVirtIODriversforMicrosoftWindows.html)


![40](./.images/40.png)

### INSTALL VIRTIO DRIVERS FOR WINDOWS

[Installing the Oracle VirtIO Drivers for Microsoft Windows](https://docs.oracle.com/en/operating-systems/oracle-linux/kvm-virtio/kvm-virtio-InstallingtheOracleVirtIODriversforMicrosoftWindows.html)

![41](./.images/41.png)

### ADJUST TIME, DATE AND TIMEZONE

THIS IS MANDATORY TO ALIGN WITH THE OCI TARGET REGION, TO ENSURE PROPER COMMUNICATION FROM THE ***ORACLE CLOUD AGENT***

![43](./.images/43.png)

### SHUTDOWN THE VIRTUAL MACHINE

![44](./.images/44.png)

### OPEN VIRTUAL MACHINE SETTINGS

![45](./.images/45.png)

### REMOVE THE TPM MODULE

![46](./.images/46.png)

### REMOVE THE VIRTUAL MACHINE ENCRYPTION

![47](./.images/47.png)

![48](./.images/48.png)

![49](./.images/49.png)

### START THE VIRTUAL MACHINE

![50](./.images/50.png)

### LOGIN AND SHUTDOWN THE VIRTUAL MACHINE

![51](./.images/51.png)

### UPLOAD THE .VMDK FILE TO AN OCI OBJECT STORAGE BUCKET

![52](./.images/52.png)

### IMPORT CUSTOM IMAGE

![53](./.images/53.png)

### CONFIGURE CUSTOM IMAGE IMPORT

- OPERATING SYSTEM : ***WINDOWS***
- VERSION: ***SERVER 2025 STANDARD***
- SOURCE: ***OBJECT STORAGE BUCKET***
- TYPE: ***VMDK***
- LAUNCH MODE: ***PARAVIRTUALIZED***

![54](./.images/54.png)

### EDIT CUSTOM IMAGE DETAILS

![55](./.images/55.png)

### SELECT THE SHAPES YOU PLAN TO USE

![56](./.images/56.png)

### EDIT CUSTOM IMAGE CAPABILITIES

![57](./.images/57.png)

### CONFIGURE IMAGE CAPABILITIES

- DISABLE BIOS

IF THE VIRTUAL MACHINE HAS BEEN CREATED WITHOUT SECURE BOOT:

- DISABLE SECURE BOOT

![58](./.images/58.png)

### CREATE COMPUTE INSTANCE FROM THE CUSTOM IMAGE

![59](./.images/59.png)

### YOU CAN INCREASE THE BOOT VOLUME SIZE (OPTIONAL)

![60](./.images/60.png)

### CHANGE INSTANCE LICENSE TYPE

- TYPE: ***BRING YOUR OWN LICENSE***

![61](./.images/61.png)

![62](./.images/62.png)

### CHECK INSTANCE METADATA


```
curl -H "Authorization: Bearer Oracle" -L http://169.254.169.254/opc/v2/instance/
```

![63](./.images/63.png)

### DELETE RECOVERY PARTITION (OPTIONAL)

```
diskpart
list disk
select disk 0
list partition
select partition 4
delete partition override
```

![64](./.images/64.png)

![65](./.images/65.png)

### EXTEND C: PARTITION

![66](./.images/66.png)

### ACTIVATE WINDOWS

![67](./.images/67.png)

![68](./.images/68.png)

### INSTALL ORACLE CLOUD AGENT (OPTIONAL)

[Contact Oracle Support to manually install Oracle Cloud Agent on a Windows instance](https://docs.oracle.com/en-us/iaas/Content/Compute/Tasks/manage-plugins.htm#install-agent)

![69](./.images/69.png)

### CHECK COMPUTE INSTANCE METRICS ARE COLLECTED (OPTIONAL)

![99](./.images/99.png)