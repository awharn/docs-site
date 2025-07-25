# zwe certificate keyring-jcl connect

[zwe](./../.././zwe.md) > [certificate](./.././zwe-certificate.md) > [keyring-jcl](././zwe-certificate-keyring-jcl.md) > [connect](./zwe-certificate-keyring-jcl-connect.md)

	zwe certificate keyring-jcl connect [parameter [parameter]...]

## Description

Connect existing certificate to Zowe keyring.




### Inherited from parent command

WARNING: This command is for experimental purposes and could be changed in the future releases.

## Examples

```
zwe certificate keyring-jcl connect --dataset-prefix my-dataset-prefix --jcllib my-jcllib --security-dry-run --keyring-owner my-keyring-owner --keyring-name my-keyring-name --connect-user cert-owner --connect-label cert-label

```

## Parameters

Full name|Alias|Type|Required|Help message
|---|---|---|---|---
--dataset-prefix,--ds-prefix||string|yes|Dataset prefix where Zowe is installed.
--jcllib||string|yes|JCLLIB data set name where the JCL will be placed.
--security-dry-run||boolean|no|Whether to dry run security related setup.
--security-product||string|no|Security product. Can be a value of RACF, ACF2 or TSS.
--keyring-owner||string|yes|Owner of the keyring.
--keyring-name||string|yes|Name of the keyring.
--trust-cas||string|no|Labels of extra certificate authorities should be trusted, separated by comma (Maximum 2).
--connect-user||string|yes|Certificate owner. Can be `SITE` or a user ID.
--connect-label||string|yes|Certificate label to connect.
--trust-zosmf||boolean|no|Whether to trust z/OSMF CA.
--zosmf-ca||string|no|Labels of z/OSMF root certificate authorities. Specify `_auto_` to let Zowe to detect automatically. This works for RACF and TSS.
--zosmf-user||string|no|z/OSMF user name. This is used to automatically detect z/OSMF root certificate authorities.
--ignore-security-failures||boolean|no|Whether to ignore security setup job failures.


### Inherited from parent command

Full name|Alias|Type|Required|Help message
|---|---|---|---|---
--help|-h|boolean|no|Display this help.
--debug,--verbose|-v|boolean|no|Enable verbose mode.
--trace|-vv|boolean|no|Enable trace level debug mode.
--silent|-s|boolean|no|Do not display messages to standard output.
--log-dir,--log|-l|string|no|Write logs to this directory.
--config|-c|string|no|Path to Zowe configuration zowe.yaml file.
--configmgr||boolean|no|Enable use of configmgr capabilities.


## Errors

Error code|Exit code|Error message
|---|---|---
ZWEL0175E|175|Failed to connect existing certificate to Zowe keyring "%s".


### Inherited from parent command

Error code|Exit code|Error message
|---|---|---
||100|If the user pass `--help` or `-h` parameter, the zwe command always exits with `100` code.
ZWEL0101E|101|ZWE_zowe_runtimeDirectory is not defined.
ZWEL0102E|102|Invalid parameter %s.
ZWEL0103E|103|Invalid type of parameter %s.
ZWEL0104E|104|Invalid command %s.
ZWEL0105E|105|The Zowe YAML config file is associated to Zowe runtime "%s", which is not same as where zwe command is located.
ZWEL0106E|106|%s parameter is required.
ZWEL0107E|107|No handler defined for command %s.
ZWEL0108E|108|Zowe YAML config file is required.
ZWEL0109E|109|The Zowe YAML config file specified does not exist.
ZWEL0110E|110|Doesn't have write permission on %s directory.
ZWEL0111E|111|Command aborts with error.
ZWEL0112E|112|Zowe runtime environment must be prepared first with "zwe internal start prepare" command.
ZWEL0114E|114|Reached max retries on allocating random number.
ZWEL0120E|120|This command must run on a z/OS system.
ZWEL0121E|121|Cannot find node. Please define NODE_HOME environment variable.
ZWEL0122E|122|Cannot find java. Please define JAVA_HOME environment variable.
ZWEL0123E|123|This function is only available in Zowe Containerization deployment.
ZWEL0131E|131|Cannot find key %s defined in file %s.
ZWEL0132E|132|No manifest file found in component %s.
ZWEL0133E|133|Data set %s already exists.
ZWEL0134E|134|Failed to find SMS status of data set %s.
ZWEL0135E|135|Failed to find volume of data set %s.
ZWEL0136E|136|Failed to APF authorize data set %s.
ZWEL0137E|137|z/OSMF root certificate authority is not provided (or cannot be detected) with trusting z/OSMF option enabled.
ZWEL0138E|138|Failed to update key %s of file %s.
ZWEL0139E|139|Failed to create directory %s.
ZWEL0140E|140|Failed to translate Zowe configuration (%s).
ZWEL0142E|142|Failed to refresh APIML static registrations.
ZWEL0151E|151|Failed to create temporary file %s. Please check permission or volume free space.
ZWEL0172E||Component %s has %s defined but the file is missing.
ZWEL0200E||Failed to copy USS file %s to MVS data set %s.
ZWEL0201E||File %s does not exist.
ZWEL0202E||Unable to find samplib key for %s.
ZWEL0203E||Env value in key-value pair %s has not been defined.
ZWEL0319E||NodeJS required but not found. Errors such as ZWEL0157E may occur as a result. The value 'node.home' in the Zowe YAML is not correct.
ZWEL0322E|322|%s is not a valid directory.
