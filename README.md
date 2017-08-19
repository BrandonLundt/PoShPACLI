# PoShPACLI

## **Powershell PACLI Module for CyberArk EPV**

Exposes the native functions of the CyberArk PACLI command line utility via a PowerShell wrapper for interfacing with CyberArk EPV.

----------

## Latest Updates

- Any Pacli output on StdErr is now written to the PowerShell error stream.
- Boolean output from functions removed
  - **NOTE:** This is a breaking change if the previous $True/$False values are required by an existing script.
- SecureString values now required for any parameters relating to password input.
- Major Change to Module Folder/File Structure.
- All Functions reworked, reducing Parse errors and resolving lots of bugs.

## Getting Started

- Check the [relationship table](#Pacli_to_PoShPACLI) to determine what PoShPACLI function exposes which PACLI command.

### Prerequisites

- The CyberArk PACLI executable must be present on the same computer as the module.

### Install & Use

Save the Module to your powershell modules folder of choice.
Find your local PowerShell module paths with the following command:

```powershell
$env:PSModulePath
```

The name of the folder for the module should be "PoShPACLI".

Import the module:

```powershell
Import-Module PoShPACLI
```

Discover Commands:

```powershell
Get-Command -Module PoShPACLI
```

Function Initialize-PoShPACLI must be run before working with the other module functions:

```powershell
Initialize-PoShPACLI -pacliFolder D:\PACLI
```

This is required to locate the CyberArk PACLI executable in the SYSTEM path, or in a folder you specify, in order for the module to be able to execute the utility.

----------

An identical process to using the PACLI tool on its own should be followed:

Example method to use the module to add a password object to a safe:

```powershell
#Locate/set path to PACLI executable

Initialize-PoShPACLI

#Start PACLI Executable

Start-PVPacli

#Define Vault

New-PVVaultDefinition -vault "VAULT" -address "vaultAddress"

#Logon to vault

Connect-PVVault -vault "VAULT" -user "User" -logonFile "credfile.xxx"

#Open Safe

Open-PVSafe -vault "VAULT" -user "User" -safe "SAFE_Name"

#Add Password to Safe

Add-PVPasswordObject -vault "VAULT" -user "User" -safe "SAFE_Name" -folder "Root" -file "passwordFile" -password (Read-Host -AsSecureString)

#Add Device Type for password

Add-PVFileCategory -vault "VAULT" -user "User" -safe "SAFE_Name" -folder "Root" -file "passwordFile" -category "DeviceType" -value
"Device_Type"

#Add PolicyID for password

Add-PVFileCategory -vault "VAULT" -user "User" -safe "SAFE_Name" -folder "Root" -file "passwordFile" -category "PolicyID" -value "Policy_Name"

#Add Logon Domain for password

Add-PVFileCategory -vault "VAULT" -user "User" -safe "SAFE_Name" -folder "Root" -file "passwordFile" -category "LogonDomain" -value "Domain_Name"

#Add Address for password

Add-PVFileCategory -vault "VAULT" -user "User" -safe "SAFE_Name" -folder "Root" -file "passwordFile" -category 'Address' -value "Address_Value"

#Add UserName for password

Add-PVFileCategory -vault "VAULT" -user "User" -safe "SAFE_Name" -folder "Root" -file "passwordFile" -category "UserName" -value "Account_Name"

#Close Safe

Close-PVSafe -vault "VAULT" -user "User" -safe "SAFE_Name"

#Logoff From Vault

Disconnect-PVVault  -vault "VAULT" -user "User"

#Stop Pacli process

Stop-PVPacli
```

## Author

- **Pete Maan** - [pspete](https://github.com/pspete)

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Contributing

Any and all contributions to this project are appreciated.
See the [CONTRIBUTING.md](CONTRIBUTING.md) for a few more details.

## <a id="Pacli_to_PoShPACLI"></a>PACLI to PoShPACLI Function Relationship

The table shows how the the PoShPACLI module functions relate to the native PACLI commands:

|PACLI Command|PoshPACLI Function|
|---:|:---|
|INIT|Start-PVPacli|
|TERM|Stop-PVPacli|
|DEFINEFROMFILE|Import-PVVaultDefinition|
|DEFINE|New-PVVaultDefinition|
|DELETEVAULT|Remove-PVVaultDefinition|
|CREATELOGONFILE|New-PVLogonFile|
|LOGON|Connect-PVVault|
|LOGOFF|Disconnect-PVVault|
|CTLGETFILENAME|Get-PVCTL|
|CTLADDCERT|Add-PVCTLCertificate|
|CTLLIST|Get-PVCTLCertificate|
|CTLREMOVECERT|Remove-PVCTLCertificate|
|STOREFILE|Add-PVFile|
|FINDFILES|Find-PVFile|
|RETRIEVEFILE|Get-PVFile|
|LOCKFILE|Lock-PVFile|
|MOVEFILE|Move-PVFile|
|DELETEFILE|Remove-PVFile|
|RESETFILE|Reset-PVFile|
|UNDELETEFILE|Restore-PVFile|
|UNLOCKFILE|Unlock-PVFile|
|INSPECTFILE|Get-PVFileActivity|
|ADDFILECATEGORY|Add-PVFileCategory|
|LISTFILECATEGORIES|Get-PVFileCategory|
|DELETEFILECATEGORY|Remove-PVFileCategory|
|UPDATEFILECATEGORY|Set-PVFileCategory|
|FILESLIST|Get-PVFileList|
|FILEVERSIONSLIST|Get-PVFileVersionList|
|FOLDERSLIST|Get-PVFolder|
|MOVEFOLDER|Move-PVFolder|
|ADDFOLDER|New-PVFolder|
|DELETEFOLDER|Remove-PVFolder|
|UNDELETEFOLDER|Restore-PVFolder|
|GROUPDETAILS|Get-PVGroup|
|ADDGROUP|New-PVGroup|
|DELETEGROUP|Remove-PVGroup|
|UPDATEGROUP|Set-PVGroup|
|ADDGROUPMEMBER|Add-PVGroupMember|
|GROUPMEMBERS|Get-PVGroupMember|
|DELETEGROUPMEMBER|Remove-PVGroupMember|
|GETHTTPGWURL|Get-PVHttpGwUrl|
|LDAPBRANCHESLIST|Get-PVLDAPBranch|
|LDAPBRANCHADD|New-PVLDAPBranch|
|LDAPBRANCHDELETE|Remove-PVLDAPBranch|
|LDAPBRANCHUPDATE|Set-PVLDAPBranch|
|LOCATIONSLIST|Get-PVLocation|
|ADDLOCATION|New-PVLocation|
|DELETELOCATION|Remove-PVLocation|
|RENAMELOCATION|Rename-PVLocation|
|UPDATELOCATION|Set-PVLocation|
|MAILUSER|Send-PVMailMessage|
|NETWORKAREASLIST|Get-PVNetworkArea|
|MOVENETWORKAREA|Move-PVNetworkArea|
|ADDNETWORKAREA|New-PVNetworkArea|
|DELETENETWORKAREA|Remove-PVNetworkArea|
|RENAMENETWORKAREA|Rename-PVNetworkArea|
|ADDAREAADDRESS|New-PVNetworkAreaAddress|
|DELETEAREAADDRESS|Remove-PVNetworkAreaAddress|
|VALIDATEOBJECT|Set-PVObjectValidation|
|GENERATEPASSWORD|New-PVPassword|
|STOREPASSWORDOBJECT|Add-PVPasswordObject|
|RETRIEVEPASSWORDOBJECT|Get-PVPasswordObject|
|DELETEPREFFEREDFOLDER|Remove-PVPreferredFolder|
|ADDPREFERREDFOLDER|Add-PVPreferredFolder|
|REQUESTSLIST|Get-PVRequest|
|DELETEREQUEST|Remove-PVRequest|
|REQUESTCONFIRMATIONSTATUS|Get-PVRequestStatus|
|CONFIRMREQUEST|Set-PVRequestStatus|
|ADDRULE|Add-PVRule|
|RULESLIST|Get-PVRule|
|DELETERULE|Remove-PVRule|
|CLOSESAFE|Close-PVSafe|
|SAFEDETAILS|Get-PVSafe|
|ADDSAFE|New-PVSafe|
|OPENSAFE|Open-PVSafe|
|DELETESAFE|Remove-PVSafe|
|RENAMESAFE|Rename-PVSafe|
|RESETSAFE|Reset-PVSafe|
|UPDATESAFE|Set-PVSafe|
|INSPECTSAFE|Get-PVSafeActivity|
|SAFEEVENTSLIST|Get-PVSafeEvent|
|ADDEVENT|Set-PVSafeEvent|
|LISTSAFEFILECATEGORIES|Get-PVSafeFileCategory|
|ADDSAFEFILECATEGORY|New-PVSafeFileCategory|
|DELETESAFEFILECATEGORY|Remove-PVSafeFileCategory|
|UPDATESAFEFILECATEGORY|Set-PVSafeFileCategory|
|ADDSAFESHARE|Add-PVSafeGWAccount|
|DELETESAFESHARE|Remove-PVSafeGWAccount|
|CLEARSAFEHISTORY|Clear-PVSafeHistory|
|SAFESLIST|Get-PVSafeList|
|SAFESLOG|Get-PVSafeLog|
|ADDNOTE|Set-PVSafeNote|
|ADDOWNER|Add-PVSafeOwner|
|OWNERSLIST|Get-PVSafeOwner|
|DELETEOWNER|Remove-PVSafeOwner|
|UPDATEOWNER|Set-PVSafeOwner|
|ADDTRUSTEDNETWORKAREA|Add-PVTrustedNetworkArea|
|DEACTIVATETRUSTEDNETWORKAREA|Disable-PVTrustedNetworkArea|
|ACTIVATETRUSTEDNETWORKAREA|Enable-PVTrustedNetworkArea|
|TRUSTEDNETWORKAREALIST|Get-PVTrustedNetworkArea|
|DELETETRUSTEDNETWORKAREA|Remove-PVTrustedNetworkArea|
|USERDETAILS|Get-PVUser|
|LOCK|Lock-PVUser|
|ADDUSER|New-PVUser|
|DELETEUSER|Remove-PVUser|
|RENAMEUSER|Rename-PVUser|
|UPDATEUSER|Set-PVUser|
|UNLOCK|Unlock-PVUser|
|INSPECTUSER|Get-PVUserActivity|
|CLEARUSERHISTORY|Clear-PVUserHistory|
|USERSLIST|Get-PVUserList|
|SETPASSWORD|Set-PVUserPassword|
|GETUSERPHOTO|Get-PVUserPhoto|
|PUTUSERPHOTO|Set-PVUserPhoto|
|OWNERSAFESLIST|Get-PVUserSafeList|
|ADDUPDATEEXTERNALUSERENTITY|Add-PVExternalUser|