# How to Apply Security Baselines to Non-Domain Joined machines running Windows 10 Version 1903 

1. Download Windows 10 Version 1903 and Windows Server Version 1903 Security Baseline.zip:

   https://www.microsoft.com/en-us/download/details.aspx?id=55319
   
1a. Also download LGPO.zip from the Security compliance toolkit from the URL above and extract the LGPO.exe file to:
    Windows 10 Version 1903 and Windows Server Version 1903 Security Baseline\Local_Script\Tools
    
    or if running the Sept 2019 re-release of Windows 10 1903:
    
   Windows 10 Version 1903 and Windows Server Version 1903 Security Baseline - Sept2019Update\Scripts\Tools
   
2. Extract the contents of the zip file. 
 

3. Open Powershell in administrative mode and temporarily set the execution policy of powershell to unrestricted with the following code:

   ```
   Set-ExecutionPolicy Unrestricted
   ```
   
4. Run the following from the Local_Script folder from the zip file you extracted from Step 2:

   ```
   .\BaselineLocalInstall.ps1 -Win10NonDomainJoined
   ```
   If you extracted the September 2019 GPO baseline then run the following in Windows 10 Version 1903 and Windows Server Version 1903    Security Baseline - Sept2019Update\Scripts:
   
   ```
   .\Baseline-LocalInstall.ps1 -Win10NonDomainJoined
   ```
   

     
   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/StandAloneHardening1903-2.PNG)
   
6. At this point the following baselines referenced below will automatically install:
 
   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/StandAloneHardening1903-3.PNG)
   
7. Set execution policy back to restricted mode
  
   ```
   Set-ExecutionPolicy Restricted
   ```
   
 
9. Type the following in an administrative command prompt window to put Windows Defender into a sandboxed mode

    ```
    setx /M MP_FORCE_USE_SANDBOX 1
    ```
    The following reference url will explain more about this setting: https://www.microsoft.com/security/blog/2018/10/26/windows-defender-antivirus-can-now-run-in-a-sandbox/
    
10. Remove PowerShell 2.0 from administrative command prompt window:

```
Dism /online /Disable-Feature /FeatureName:"MicrosoftWindowsPowerShellV2Root"
```

## Bitlocker Security

By default bitlocker is configured with XTS-AES-128 encryption and preboot authentication is left off by default. Its highly recommended that you turn on preboot authentication. Preboot authentication is explained in the following document for bitlocker countermeasures:

https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-countermeasures

Preboot authentication can be turned on in the following location for your operating system.

Computer Configuration\Administrative Templates\Windows Components\Bitlocker Drive Encryption\Operating System Drives\Require Additional Authentication at startup

If your host machine has a TPM chip set the following options:

![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/StandAlone1903-1.PNG)

If your host machine does not have a TPM chip set the following options:

![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/StandAlone1903-2.PNG)

(Optional) Configure OS encryption to XTS-AES-256

The option to use AES256 over AES128 is optional. Both provide an adequate layer data encryption protection of hard disks when the device is powered off. However, if you are in the goverment sector or in a federally regulated environment you should be using an AES256 suite to protect data. Further guidance can be found at:

https://apps.nsa.gov/iaarchive/programs/iad-initiatives/cnsa-suite.cfm

Encryption options can be set at the following location:

Computer Configuration\Administrative Templates\Windows Components\Bitlocker Drive Encryption

![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/StandAlone1903-4.PNG)

At this point right click on your C (OS Drive) and turn on bitlocker

## Office 365 Security

The following can be used in conjuction with Windows 10 hardening. It is extremely important to have adequate Microsoft Office security protections enabled as they can be a frequent for hackers. For Example DDE attacks https://www.sentinelone.com/blog/malware-embedded-microsoft-office-documents-dde-exploit-macroless/

1. To Start download the Office 365 Pro Plus zip file from the security compliance toolkit

   https://www.microsoft.com/en-us/download/details.aspx?id=55319
   
1a. Also download LGPO.zip from the Security compliance toolkit from the URL above and extract the LGPO.exe file to:
    Windows 10 Version 1903 and Windows Server Version 1903 Security Baseline\Local_Script\Tools

2. Download the Office 365 admin templates from the following link: (Hint: 64 bit is the default install now for Office in unmanaged environments)

   https://www.microsoft.com/en-us/download/details.aspx?id=49030
   
3. Extact the Office 365 templates to a directory of your chosing. For my example I put it in a c:\temp directory called "C:\temp\Office365 Templates\"

4. Copy the ADMX files from my example above from the following locations:
   
   From: C:\temp\Office365 Templates\admx
   
   To: C:\Windows\PolicyDefinitions
   
5. Copy the ADML files from my example about to the following locations:

   From: C:\temp\Office365 Templates\admx\en-us
   
   To: C:\Windows\PolicyDefinitions\en-US
   
 6. Exact the Office 365 zip file from the security compliance toolkit to c:\temp\
 
 7. Navigate to the following folder in a elevated powershell session:
    
    C:\temp\Office365-ProPlus-Sept2019-FINAL\Scripts
    
 8. Open Powershell in administrative mode and temporarily set the execution policy of powershell to unrestricted with the following    code:

   ```
   Set-ExecutionPolicy Unrestricted
   ``` 
 
 9. Run the following powerhsell command as shown:
 
   ```
   .\Baseline-LocalInstall.ps1 
   ```
   
   The following switches are supported if you don't want to install certain Office 365 hardening techniques:
   
   -NoRequiredMacroSigning - Doesn't install the GPO that disallows execution of unsigned macros.
                             If this switch is not specified, unsigned macros will not execute.

   -NoLegacyFileBlock      - Doesn't install the GPO that disallows the loading or saving of
                             legacy file formats, such as .doc, .dot, .xls, etc.

   -NoExcelDDEBlock        - Doesn't install the GPO that blocks Excel from performing DDE
                             lookup or launch.

 ## Windows 10 References and Post hardening checklist

Ensure Windows 10 Block at First sight is turned on
    
Reference URL: https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-antivirus/configure-block-at-first-sight-windows-defender-antivirus
    
 
     

 
