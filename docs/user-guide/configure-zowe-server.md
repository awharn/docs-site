## Configuring the Zowe server started task.

When the Zowe runtime is launched, it is run under a z/OS started task (STC) with the PROCLIB member named `ZWESVSTC`.  A sample PROCLIB is created during install into the PDS SZWESAMP.  To launch Zowe as a started task the member should be copied to a PDS that is in the proclib concatenation path.  

If a site has their own technique for PROCLIB creation they may follow this and copy the `ZWESVSTC` as-is.  For cusstomers who wish to create a pipeline or otherwise automate the PROCLIB copying a convenience script `zowe-install-proc.sh` is provided in the `<ROOT_DIR>/scripts/utils` folder. The script has two arguments

**First Parameter**=Source PDS Prefix

Dataset prefix of the source PDS where .SZWESAMP(ZWESVSTC) was installed into.  

For an installation from a convenience build this will be the value of the `-h` argument when `zowe-install.sh` was executed.   

For an SMP/E installation thils will be the value of 
`$datasetPrefixIn` in the member AZWE001.F1(ZWE3ALOC)

**Second Parameter**=Target PROCLIB PDS

Target PROCLIB PDS where ZWESVSTC will be placed.  If parameter is omitted the script scans the JES PROCLIB concatenation path and uses the first dataset where the user has write access

***Example*** Executing the command `zowe-install-proc.sh MYUSERID.ZWE USER.PROCLIB` copies the PDS member `MYUSERID.ZWE.SZWESAMP(ZWESVSTC)` to `USER.PROCLIB(ZSWESAMP)`

There are two ways in which the started task can be executed.  

### Starting Zowe from a USS shell

From a USS shell issue the command `<zowe-instance-dir>/zowe-start.sh` to launch the started task `ZWESVSTC`.  This will the configuration values from the `zowe-instance.env` file in the zowe instance directory.  

### Starting Zowe with a /S TSO command

If you issue the SDSF command `/S ZWESVSETC` will fail because the script needs to know the instance directory containing the configuration details.  

If you have a default instance directory you wish you always start Zowe with you can tailor the JCL member `ZWESVSTC` at this line

```
//ZWESVSTC   PROC INSTANCE='{{instance_directory}}'
```

to replace the `instance_directory` with the location of the Zowe instanceDir that contains the configurable Zowe instance directory. 

If the JCL value `instance-directory` is not specified in the JCL, in order to start the Zowe server from SDSF you will need to and the INSTANCE parameter on the START command when you start Zowe in SDSF:

```
/S ZWESVSTC,INSTANCE='$ZOWE_INSTANCE_DIR',JOBNAME='ZWEXSV'
```

The `JOBNAME='ZWEXSV'` is optional and the started task will operate correctly without it, however having it specified ensures that the address spaces will be prefixed with `ZWEXSV` which makes them easier to find in SDSF or locate in RMF records.  

### Configuring ZWESVSTC to run under the correct user ID

The ZWESVSTC must be configured as a started task (STC) under the IZUSVR user ID.  This only needs to be done once per z/OS system and would be typically done the first time you configure a Zowe runtime.  If the Zowe runtime is uninstalled or a new Zowe is installed and configured, you do not need to re-run the step to associate the ZWESVSTC STC with the Zowe user ID of IZUSVR.  

To configure ZWESVSTC to run as a STC under the user ID of IZUSVR, you can run the convenience script `scripts/configure/zowe-config-stc.sh` in the runtime folder.  

Alternatively, if you do not wish to run this script, you can manually configure ZWESVSTC to run under the IZUSVR user ID by taking the following steps.

**Note:** You must replace `ZWESVSTC` in the commands below with the name of your PROCLIB member that you specified as `memberName=ZWESVSTC` in the `scripts/configure/zowe-install.yaml` file.

- If you use RACF, issue the following commands:

  ```
  RDEFINE STARTED ZWESVSTC.* UACC(NONE) STDATA(USER(IZUSVR) GROUP(IZUADMIN) PRIVILEGED(NO) TRUSTED(NO) TRACE(YES))  
  SETROPTS REFRESH RACLIST(STARTED)
  ```

- If you use CA ACF2, issue the following commands:

  ```
  SET CONTROL(GSO)
  INSERT STC.ZWESVSTC LOGONID(IZUSVR) GROUP(IZUADMIN) STCID(ZWESVSTC)
  F ACF2,REFRESH(STC)
  ```

- If you use CA Top Secret, issue the following commands:

  ```
  TSS ADDTO(STC) PROCNAME(ZWESVSTC) ACID(IZUSVR)
  ```

### Granting users permission to access Zowe

TSO user IDs using Zowe must have permission to access the z/OSMF services that are used by Zowe.  They should be added to the the IZUUSER group for standard users or IZUADMIN for administrators,

- If you use RACF, issue the following command:

  ```
  CONNECT (userid) GROUP(IZUADMIN)
  ```

- If you use CA ACF2, issue the following commands:

  ```
  ACFNRULE TYPE(TGR) KEY(IZUADMIN) ADD(UID(<uid string of user>) ALLOW)
  F ACF2,REBUILD(TGR)
  ```

- If you use CA Top Secret, issue the following commands:

  ```
  TSS ADD(userid)  PROFILE(IZUADMIN)
  TSS ADD(userid)  GROUP(IZUADMGP)
  ```