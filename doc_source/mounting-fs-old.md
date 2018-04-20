# Mounting File Systems Without the EFS Mount Helper<a name="mounting-fs-old"></a>

**Note**  

In the following section, you can learn how to install the Network File System \(NFS\) client and how to mount your Amazon EFS file system on an Amazon EC2 instance\. You also can find an explanation of the `mount` command and the available options for specifying your file system's Domain Name System \(DNS\) name in the `mount` command\. In addition, you can find how to use the file `fstab` to automatically remount your file system after any system restarts\.

**Note**  
Before you can mount a file system, you must create, configure, and launch your related AWS resources\. For detailed instructions, see [Getting Started with Amazon Elastic File System](getting-started.md)\.

**Topics**
+ [NFS Support](#mounting-fs-nfs-info)
+ [Installing the NFS Client](#mounting-fs-install-nfsclient)
+ [Mounting on Amazon EC2 with a DNS Name](mounting-fs-mount-cmd-dns-name.md)
+ [Mounting with an IP Address](mounting-fs-mount-cmd-ip-addr.md)
+ [Mounting Automatically](mount-fs-auto-mount-onreboot-old.md)

## NFS Support<a name="mounting-fs-nfs-info"></a>

Amazon EFS supports the Network File System versions 4\.0 and 4\.1 \(NFSv4\) and NFSv4\.0 protocols when mounting your file systems on Amazon EC2 instances\. Although NFSv4\.0 is supported, we recommend that you use NFSv4\.1\. Mounting your Amazon EFS file system on your Amazon EC2 instance also requires an NFS client that supports your chosen NFSv4 protocol\.

To get the best performance out of your file system, use an Amazon EC2 Amazon Machine Image \(AMI\) that includes a Linux kernel that is version 4\.0 or newer\. We recommend using **Amazon Linux AMI 2016\.03\.0** or **Amazon Linux AMI 2016\.09\.0** as the AMI for the Amazon EC2 instance to mount your file system to\.

**Note**  
Using Amazon EFS with Microsoft Windows Amazon EC2 instances is not supported\.

### Troubleshooting AMI and Kernel Versions<a name="ami-kernel-versions-troubleshooting"></a>

To troubleshoot issues related to certain AMI or kernel versions when using Amazon EFS from an EC2 instance, see [Troubleshooting AMI and Kernel Issues](troubleshooting.md#troubleshooting-efs-ami-kernel)\.

## Installing the NFS Client<a name="mounting-fs-install-nfsclient"></a>

To mount your Amazon EFS file system on your Amazon EC2 instance, first you need to install an NFS client\. To connect to your EC2 instance and install an NFS client, you need the public DNS name of the EC2 instance and a user name to log in\. That user name for your instance is typically `ec2-user`\.

**To connect your EC2 instance and install the NFS client**

1. Connect to your EC2 instance\. Note the following about connecting to the instance:
   + To connect to your instance from a computer running Mac OS or Linux, specify the \.pem file to your Secure Shell \(SSH\) client with the `-i` option and the path to your private key\.
   + To connect to your instance from a computer running Windows, you can use either MindTerm or PuTTY\. If you plan to use PuTTY, you need to install it and use the following procedure to convert the \.pem file to a \.ppk file\. 

   For more information, see the following topics in the *Amazon EC2 User Guide for Linux Instances*:
   +  [Connecting to Your Linux Instance from Windows Using PuTTY](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) 
   +  [Connecting to Your Linux Instance Using SSH](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)

     The key file cannot be publicly viewable for SSH\. You can use the `chmod 400` *filename*`.pem` command to set these permissions\. For more information, see [Create a Key Pair](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html#create-a-key-pair)\.

1. \(Optional\) Get updates and reboot\.

   ```
   $  sudo yum -y update  
   $  sudo reboot
   ```

1. After the reboot, reconnect to your EC2 instance\.

1. Install the NFS client\.

   If you're using an Amazon Linux AMI or Red Hat Linux AMI, install the NFS client with the following command\.

   ```
   $ sudo yum -y install nfs-utils
   ```

   If you're using an Ubuntu Amazon EC2 AMI, install the NFS client with the following command\.

   ```
   $ sudo apt-get -y install nfs-common
   ```

If you use a custom kernel \(that is, if you build a custom AMI\), you need to include at a minimum the NFSv4\.1 client kernel module and the right NFS4 userspace mount helper\.

**Note**  
If you choose **Amazon Linux AMI 2016\.03\.0** or **Amazon Linux AMI 2016\.09\.0** when launching your Amazon EC2 instance, you don't need to install `nfs-utils` because it's already included in the AMI by default\.

**Next: Mount Your File System**  
Use one of the following procedures to mount your file system\.
+ [Mounting on Amazon EC2 with a DNS Name](mounting-fs-mount-cmd-dns-name.md)
+ [Mounting with an IP Address](mounting-fs-mount-cmd-ip-addr.md)
+ [Mounting Your Amazon EFS File System Automatically](mount-fs-auto-mount-onreboot.md)