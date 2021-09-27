# How to Download Genome Sequence Data Using `sFTP`
###### tags: `tutorials` `sftp` `genome sequence data`

## Setups
1. Check that you have all the correspondence you need: host, username, and password. 
2. Make sure you have access to Duke Compute Cluster (DCC). Following this instruction assumes that you will use `sftp` to transfer the genome sequence file *directly onto the cluster*, bypassing your local computer. 


## Instructions
1. Log into your cluster using ssh 
2. Type in `sftp user.name@sftp.genewiz.com`
    * `sftp` stands for "secure file transfer protocol"
    * Use the actual username given by Genewiz
3. Type `yes` to a prompted command about something fingerprint (I think it doesn't recognize a new computer in the system)
4. Enter the password provided by Genewiz (Sarah found out that you can copy and paste the password)
5. Set up the local directory where you want to download these files into
```
    lcd /datacommons/noor2/lethal_seq/2021_09_21_fastq
```
7. Navigate through sftp folders until you reach the directory that contains all fastq files
8. There are two ways to download the files:
    a. Download all files within the project folder using `mget *` 
    b. Download the folder AND everything within that folder by using **recursive** command `get -r Parent_folder`
    
    
For detailed instructions go [here.](https://f.hubspotusercontent00.net/hubfs/3478602/Sell%20Sheet%20Collateral%20Library/NGS/NGS%20User%20Guides/NGS_sFTP-Data-Download-Guide_Option%201_Nov03_2020.pdf)

## Tips
If you are working on DCC's shared space, and need a writing permission to download the files. You, or the owner of the directory, need to change the permission of the directory to "writable" for everyone. 

    chmod -R 777 Folder_name

This command line will fully open access to all (user/group/others).

For more information about file permissions, go here. 