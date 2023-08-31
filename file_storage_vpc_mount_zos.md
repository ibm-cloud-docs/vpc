---

copyright:
  years: 2022, 2023
lastupdated: "2023-08-08"

keywords: file share, file storage, mount helper, mount target, mount path, secure connection

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Mounting file shares on z/OS
{: #file-storage-vpc-mount-zos}

Use these instructions to connect a z/OS-based {{site.data.keyword.cloud}} Compute Instance to a Network File System (NFS) file share.
{: shortdesc}

## Before you begin
{: #fs-zos-prereq}

1. Verify that the [virtual server instance](/docs/vpc?topic=vpc-about-advanced-virtual-servers) where you want to mount the share is in the same zone as the file share. 
2. Confirm that a mount target for the share exists for the VPC that the instance resides in. If a new mount target is needed, follow the instructions in [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create). 
3. Get the mount path of the file share from the mount target. Mount path information can be obtained from the File share details page in the [UI](/docs/vpc?topic=vpc-file-storage-view&interface=ui#fs-get-mountpath-ui-vpc), from the [CLI](/docs/vpc?topic=vpc-file-storage-view&interface=cli#fs-get-mountpath-cli), with the [API](/docs/vpc?topic=vpc-file-storage-view&interface=api#fs-get-target-api), or [Terraform](/docs/vpc?topic=vpc-file-storage-view&interface=terraform#fs-view-mount-target-terraform).

## Mounting the file share
{: #fs-zos-mount-file-share}

Follow these steps to mount a file share on a z/OS host.

1. Create a `/u/ibmuser` directory.

   ```sh
   mkdir /u/ibmuser/test
   ```
   {: codeblock}

2. Start the NFS client. You can issue commands `D OMVS,P` to verify whether the NFS client is started or not.

   * If the entry for NFS file system is in the `sys1.parmlib.bpxprm00`, you can safely skip this step.

   * If the entry for NFS file system is not in the `sys1.parmlib.bpxprm00`, you need to complete the following steps to start the NFS client:

     1. To set up the NFS client, run the following job on your z/OS virtual server instance.
        ```sh
        //NFSCLNT  JOB CLASS=A,MSGCLASS=H,MSGLEVEL=(1,1),NOTIFY=&SYSUID.,
        //         REGION=0M
        //*********************************************************************
        //* Configuration for the NFS client.                                 *
        //*********************************************************************
        //ALIAS    EXEC PGM=IDCAMS
        //SYSPRINT DD SYSOUT=*
        //SYSIN    DD *
          DEF ALIAS(NAME(MVSCLNT) RELATE(CATALOG.VS01.SYS)) -
            CAT(CATALOG.VS01.MASTER)
        /*
        //DEFLOG1  EXEC PGM=IEFBR14
        //SYSPRINT DD SYSOUT=*
        //LOG1     DD DSN=MVSCLNT.LOG1,
        //         DISP=(NEW,CATLG),UNIT=3390,SPACE=(CYL,(2,5),RLSE),
        //         RECFM=VB,LRECL=137,BLKSIZE=6144,VOL=SER=OPEVS1
        //*
        //DEFLOG2  EXEC PGM=IEFBR14
        //SYSPRINT DD SYSOUT=*
        //LOG2     DD DSN=MVSCLNT.LOG2,
        //         DISP=(NEW,CATLG),UNIT=3390,SPACE=(CYL,(2,5),RLSE),
        //         RECFM=VB,LRECL=137,BLKSIZE=6144,VOL=SER=OPEVS1
        //*
        //DEFRACF  EXEC PGM=IKJEFT01
        //SYSPRINT DD SYSOUT=*
        //SYSTSPRT DD SYSOUT=*
        //SYSTSIN  DD *
          RDEFINE STARTED MVSNFSC.* UACC(NONE) +
            STDATA(USER(SYSTEM) GROUP(SYS1) TRUSTED(YES))

        SETR REFRESH RACLIST(STARTED)
        /*
        //COPYSAMP EXEC PGM=IEBCOPY
        //SYSPRINT DD SYSOUT=*
        //SYSUT1   DD UNIT=SYSDA,SPACE=(TRK,(5,1))
        //SYSUT2   DD UNIT=SYSDA,SPACE=(TRK,(5,1))
        //SYSUT3   DD UNIT=SYSDA,SPACE=(TRK,(5,1))
        //SYSUT4   DD UNIT=SYSDA,SPACE=(TRK,(5,1))
        //INDD     DD DSN=SYS1.NFSSAMP,DISP=SHR
        //OUTDD    DD DSN=SYS1.PROCLIB,DISP=SHR
        //SYSIN    DD *
          COPY I=INDD,O=OUTDD
          SELECT MEMBER=((GFSCPROC,MVSNFSC))
        /*
        ```
        {: codeblock}

     2. Add the following command to the `SYS1.PARMLIB(BPXPRM00)` after `AUTOMNT`.
        ```sh
        FILESYSTYPE TYPE(NFS) ENTRYPOINT(GFSCINIT) ASNAME(MVSNFSC)
        ```
        {: pre}

     3. To start the NFS client address space `MVSNFSC`, issue the following commands from the console `/SDSF Log`.

        ```sh
        set OMVS=00
        ```
        {: pre}

     4. To verify that the NFS client is started, issue the commands `D OMVS,P`.

     If the principal `MVSNFSC` is in the list, the NFS client is started successfully.
     {: note}

3. Run the following commands from TSO to mount the file share on z/OS in ASCII format:

   ```sh
   mount type(NFS) filesystem(nfs02) mountpoint('/u/ibmuser/test') parm('<mount path>,xlat(y),vers(4)')
   ```
   {: pre}

   You can also run the following commands to mount the file share on z/OS in binary format:

   ```sh
   mount type(NFS) filesystem(nfs02) mountpoint('/u/ibmuser/test') parm('<mount path>,vers(4)')
   ```
   {: pre}

   For more information, see [Additional mount command examples](https://www.ibm.com/docs/en/zos/2.5.0?topic=examples-additional-mount-command){: external}.

4. To verify that the file share is successfully mounted to z/OS, you can run command `df` to display the directory `/u/ibmuser/test`.
