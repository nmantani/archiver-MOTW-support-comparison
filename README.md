# Comparison of MOTW (Mark of the Web) propagation support of archiver software for Windows

English | [Japanese](README.ja.md)

## Background
On 3 March 2022, Microsoft [announced](https://docs.microsoft.com/en-us/deployoffice/security/internet-macros-blocked) that the default behavior of Office applications on Windows will be changed to block macros in files from the internet (such as email attachment).

An excerpt from the announcement:
> VBA macros are a common way for malicious actors to gain access to deploy malware and ransomware. Therefore, to help improve security in Office, we’re changing the default behavior of Office applications to block macros in files from the internet.
>
> ...
>
> This change only affects Office on devices running Windows and only affects the following applications: Access, Excel, PowerPoint, Visio, and Word.
>
> The change will begin rolling out in Version 2203, starting with Current Channel (Preview) in early April 2022. Later, the change will be available in the other update channels, such as Current Channel and Monthly Enterprise Channel.

This is a great improvement of defense against malicious Office document files.

According to the announcement, whether blocking macro or not is determined based on MOTW (Mark of the Web) attribute of the file. Applications such as web browsers and email clients put MOTW on downloaded files and email attachments that come from the internet. MOTW is stored in Zone.Identifier NTFS alternate data stream.

To block macro of malicious Office document files that are extracted from archive files, an archiver software has to propagate MOTW to extracted files when an archive file has MOTW. If archiver software does not propagate MOTW, malicious Office documents in archive files can circumvent blocking.

A question came up: **"What archiver software can propagate MOTW to extracted files?"** So I tested some archiver software and summarized the result.

## Comparison table of MOTW propagation support (as of 4 October 2025)
|Name|Tested version|License|MOTW propagation|Enabled by default|Note|
|----|--------------|-------|----------------|------------------|----|
|"Extract all" built-in function of Windows Explorer|Windows 11 24H2|proprietary|Yes :heavy_check_mark:|Yes :heavy_check_mark:||
|[7-Zip](https://www.7-zip.org/)|25.01|GNU LGPL|Yes :heavy_check_mark:|No :x: <a href="#*1">*1</a>||
|[Bandizip](https://en.bandisoft.com/bandizip/)|Standard Edition 7.40|freeware|Yes :heavy_check_mark:|Yes :heavy_check_mark:|Only for specific file extensions <a href="#*2">*2</a>|
|[CubeICE](https://www.cube-soft.jp/cubeice/)|3.5.1|freeware / proprietary|Yes :heavy_check_mark:|Yes :heavy_check_mark:||
|[Explzh](https://www.ponsoftware.com/en/)|9.87|proprietary for commercial use|Yes :heavy_check_mark:|Yes :heavy_check_mark:||
|[NanaZip](https://github.com/M2Team/NanaZip)|6.0 Preview 1 (6.0.1461.0)|MIT|Yes :heavy_check_mark:|Yes :heavy_check_mark:|Configurable to propagate for specific file extensions <a href="#*3">*3</a>|
|[PeaZip](https://peazip.github.io/)|10.6.1|GNU LGPL|Yes :heavy_check_mark:|Yes :heavy_check_mark:||
|[TC4Shell](https://www.tc4shell.com/)|21.3.0 (trial)|proprietary|Yes :heavy_check_mark:|Yes :heavy_check_mark:||
|[Total Commander](https://www.ghisler.com/)|11.56 (trial)|proprietary|Yes :heavy_check_mark:|Yes :heavy_check_mark:||
|[WinRAR](https://www.win-rar.com/)|7.13 (trial)|proprietary|Yes :heavy_check_mark:|Yes :heavy_check_mark:|Only for specific file extensions by default <a href="#*4">*4</a>|
|[WinZip](https://www.winzip.com/)|77.0 (trial)|proprietary|Yes :heavy_check_mark:|Yes :heavy_check_mark:||
|[Ashampoo ZIP Free](https://www.ashampoo.com/en-us/zip-free)|1.0.7|freeware (registration required)|No :x:|||
|[CAM UnZip](https://www.camunzip.com/)|5.25.4.0|proprietary for commercial use|No :x:|||
|Expand-Archive cmdlet of [PowerShell](https://github.com/PowerShell/PowerShell/)|7.5.3|MIT|No :x:|||
|[Express Zip](https://www.nchsoftware.com/zip/)|11.28|proprietary for commercial use|No :x:|||
|[File Compact](https://www.sourcenext.com/product/pc/oth/pc_oth_001267/)|7.02|proprietary|No :x:|||
|[IZArc](https://www.izarc.org/)|4.6|freeware|No :x:||Despite the availability of the"Propagate Mark of the Web" option <a href="#*5">*5</a>|
|[LhaForge](https://claybird.sakura.ne.jp/garage/lhaforge/index.html)|2.0.1|MIT|No :x:|||
|[Lhaplus](https://www7a.biglobe.ne.jp/~schezo/)|1.74|freeware|No :x:|||
|[PowerArchiver](https://www.powerarchiver.com/)|22.00.11 (trial)|proprietary|No :x:|||
|[StuffIt Expander](https://stuffit.com/)|15.0.8|freeware|No :x:|||
|[tar.exe (bsdtar)](https://github.com/libarchive/libarchive) of Windows 11|3.8.1|BSD 2-clause|No :x:|||
|[Universal Extractor 2](https://github.com/Bioruebe/UniExtract2)|2.0.0 RC 3|GNU GPLv2|No :x:|||
|[ZipGenious](https://zipgenius.it/en/)|6.3.2.3116|freeware|No :x:|||
|[Zipware](https://www.zipware.org/)|1.6|freeware|No :x:|||

<a id="*1">*1</a>: Though 7-Zip has supported MOTW propagation since version 22.00, it is disabled by default. You can enable it for 7-Zip GUI with the "Propagate Zone Id stream:" option in "Tools" -> "Options" -> "7-Zip" of 7-Zip File Manager.

![images/7-zip-setting.png](images/7-zip-setting.png)

When you set the option to Yes, 7-Zip propagates MOTW to all extracted files. When you set it to "For Office files", 7-Zip propagates MOTW to files with the following file extensions:
- .doc .docb .docm .docx .dot .dotm .dotx .wbk .wll .wwl
- .pot .potm .potx .ppa .ppam .pps .ppsm .ppsx .ppt .pptm .pptx .sldm .sldx
- .xla .xlam .xlm .xls .xlsb .xlsm .xlsx .xlt .xltm .xltx

You can also enable MOTW propagation by setting the registry HKEY_CURRENT_USER\SOFTWARE\7-Zip\Options\WriteZoneIdExtract DWORD to 1.

For 7-Zip CLI, -snz switch is required to propagate MOTW regardless of the option above.

<a id="*2">*2</a>: Accoring to [the document of Bandizip](https://www.bandisoft.com/bandizip/help/zone-identifier/), Bandizip propagates MOTW to files with the following file extensions:
- .exe .com .msi .scr .bat .cmd .pif .bat .lnk
- .zip .zipx .rar .7z .alz .egg .cab .bh
- .iso .img .isz .udf .wim .bin .i00
- .js .jse .vbs .vbe .wsf
- .url .reg
- .docx .doc .xls .xlsx .ppt .pptx .wiz

I previously tested Bandizip with a ZIP archive file that contained only text files, and I misunderstood that Bandizip does not propagate MOTW.

<a id="*3">*3</a>: NanaZip has enabled MOTW propagation by default since version 6.0 Preview 1, You can configure it with the "Propagate Zone Id stream" option in "Options" -> "Integration" of NanaZip GUI.

![images/nanazip-setting.png](images/nanazip-setting.png)

When you set it to "For unsafe files", NanaZip propagate MOTW to files with the following file extensions:
- .doc .dot .wbk
- .docx .docm .dotx .dotm .docb .wll .wwl
- .xls .xlt .xlm
- .xlsx .xlsm .xltx .xltm .xlsb .xla .xlam
- .ppt .pot .pps .ppa .ppam
- .pptx .pptm .potx .potm .ppam .ppsx .ppsm .sldx .sldm
- .bat .cmd .com .exe .hta .js .jse .lnk .msi .pif .ps1 .scr .vbe .vbs .wsf
- .7z .iso .rar .tar .vhd .vhdx .zip

NanaZip supports [system-wide policies](https://github.com/M2Team/NanaZip/blob/main/Documents/Policies.md#propagate-zoneid-stream) with registry. These policies override user settings.

<a id="*4">*4</a>: WinRAR 7.0 introduced the "Propagate Mark of the Web" option. You can choose the following values:
- Never
- For office files
- For executable and office files
- For all files
- For user defined types

The option is supported only by WinRAR GUI. WinRAR CLI does not propagate MOTW regardless of the option.

![images/winrar-setting.png](images/winrar-setting.png)

The default is "For executable and office files" and WinRAR propagates MOTW to files with the following file extensions:
- .exe .bat .cmd .hta .lnk .msi .pif .ps1 .scr .vbs
- .doc .docb .docm .docx .dot .dotm .dotx .wbk
- .ppa .ppam .pot .potm .potx .pps .ppsm .ppsx .ppt .pptm .pptx .sldm .sldx
- .xls .xlsb .xlsm .xlsx .xlm .xlt .xltm .xltx

When you set the option to "For office files", WinRAR propagates MOTW to files with the following file extensions:
- .doc .docb .docm .docx .dot .dotm .dotx .wbk
- .ppa .ppam .pot .potm .potx .pps .ppsm .ppsx .ppt .pptm .pptx .sldm .sldx
- .xls .xlsb .xlsm .xlsx .xlm .xlt .xltm .xltx

You can specify file extensions when you set the option to "For user defined types".

<a id="*5">*5</a>: IZArc version 4.6 introduced the "Propagate Mark of the Web" option, and it is enabled by default. However, it seems that IZArc does not propagate MOTW regardless of the option.

## Comparison table of MOTW propagation behavior (as of 4 October 2025)
|Name|Tested version|MOTW propagation behavior|
|----|--------------|-------------------------|
|"Extract all" built-in function of Windows Explorer|Windows 11 24H2|<ul><li>MOTW is propagated only if ZoneId value of the MOTW is 3 (Internet) or 4 (Untrusted sites)</li><li>ZoneId field of the archive file is inherited</li><li>The absolute path of the archive file is set for the ReferrerUrl field</li><li>All other fields are ignored</li><li>Extraction of .exe .lnk .vbs files is blocked when the ZoneId value is 4 (Untrusted sites)</li></ul>|
|[7-Zip](https://www.7-zip.org/)|25.01|<ul><li>MOTW of the archive file is propagated without modification</li><li>Only for specific file extensions if the "Propagate Zone Id stream:" option is set to "For Office files" <a href="#*1">*1</a></li></ul>|
|[Bandizip](https://en.bandisoft.com/bandizip/)|Standard Edition 7.40|<ul><li>MOTW of the archive file is propagated without modification</li><li>Only for specific file extensions <a href="#*2">*2</a></li></ul>|
|[CubeICE](https://www.cube-soft.jp/cubeice/)|3.5.1|<ul><li>MOTW is propagated only if ZoneId value of the MOTW is 3 (Internet) or 4 (Untrusted sites)</li><li>Only ZoneId field of the archive file is inherited and all other fields are ignored</li></ul>|
|[Explzh](https://www.ponsoftware.com/en/)|9.87|<ul><li>MOTW is propagated only if ZoneId value of the MOTW is 3 (Internet)</li><li>Only ZoneId field of the archive file is inherited and all other fields are ignored</li></ul>|
|[NanaZip](https://github.com/M2Team/NanaZip)|6.0 Preview 1 (6.0.1461.0)|<ul><li>MOTW of the archive file is propagated without modification</li><li>Only for specific file extensions if the "Propagate Zone Id stream" option is set to "For unsafe files" <a href="#*3">*3</a></li></ul>|
|[PeaZip](https://peazip.github.io/)|10.6.1|<ul><li>MOTW of the archive file is propagated without modification</li></ul>|
|[TC4Shell](https://www.tc4shell.com/)|21.3.0 (trial)|<ul><li>Only ZoneId field of the archive file is inherited and all other fields are ignored</li></ul>|
|[Total Commander](https://www.ghisler.com/)|11.56 (trial)|<ul><li>MOTW of the archive file is propagated except for the ReferrerUrl field</li></ul>|
|[WinRAR](https://www.win-rar.com/)|7.13 (trial)|<ul><li>Only ZoneId field of the archive file is inherited and all other fields are ignored</li><li>Only for specific file extensions <a href="#*7">*7</a></li></ul>|
|[WinZip](https://www.winzip.com/)|77.0 (trial)|<ul><li>MOTW is propagated only if ZoneId value of the MOTW is 3 (Internet) or 4 (Untrusted sites)</li><li>ZoneId field of the archive file is inherited</li><li>The absolute path of the archive file is set for the ReferrerUrl field</li><li>All other fields are ignored</li><li>Extraction of .exe .lnk .vbs files is blocked when the ZoneId value is 4 (Untrusted sites)</li></ul>|

### MOTW propagation examples
In these examples, MOTW was manually set for a ZIP archive file motw-test.zip with Set-MOTW.ps1, then MOTW of an extracted file is displayed with Get-MOTW.ps1. Set-MOTW.ps1 and Get-MOTW.ps1 are available at my [PS-MOTW](https://github.com/nmantani/PS-MOTW) repository.

- MOTW of a file extracted with Windows Explorer or WinZip (except version 28.0):
![images/explorer.png](images/explorer.png)

- MOTW of a file extracted with 7-Zip, Bandizip, NanaZip, or PeaZip:
![images/bandizip.png](images/bandizip.png)

- MOTW of a file extracted with CubeICE, Explzh, TC4Shell, or WinRAR:
![images/explzh.png](images/explzh.png)

- MOTW of a file extracted with Total Commander:
![images/total-commander.png](images/total-commander.png)

## FAQ
- ### What is MOTW (Mark of the Web)?
  Please see these blog articles:
  - [Details about the Mark-of-the-Web (MOTW)](https://nolongerset.com/mark-of-the-web-details/) by Mike Wolfe ([@NoLongerSet](https://twitter.com/NoLongerSet))
  - [Downloads and the Mark-of-the-Web](https://textslashplain.com/2016/04/04/downloads-and-the-mark-of-the-web/) by Eric Lawrence ([@ericlaw](https://twitter.com/ericlaw))
  - [Mark-of-the-Web from a red team’s perspective](https://outflank.nl/blog/2020/03/30/mark-of-the-web-from-a-red-teams-perspective/) by Stan Hegt ([@stanhacked](https://twitter.com/stanhacked))

  They are very helpful to understand it.

- ### My favorite archiver software is not listed.
  Please provide your test result from [Issues](https://github.com/nmantani/archiver-MOTW-support-comparison/issues) or [Pull requests](https://github.com/nmantani/archiver-MOTW-support-comparison/pulls). Because I am Japanese, the comparison table contains some Japanese archiver software that you may not know.

- ### How to test my favorite archiver software?
  Please see [Details about the Mark-of-the-Web (MOTW)](https://nolongerset.com/mark-of-the-web-details/). It compares behavior of the built-in Windows unzip utility and 7-zip. You can test your favorite archiver software in a  similar fashion.

  I created [PS-MOTW](https://github.com/nmantani/PS-MOTW), PowerShell scripts to manually set / show / remove MOTW. You can use it for testing archiver software.

- ### Information is incorrect or outdated.
  Please provide the details from [Issues](https://github.com/nmantani/archiver-MOTW-support-comparison/issues) or the fix from [Pull requests](https://github.com/nmantani/archiver-MOTW-support-comparison/pulls). I am happy to fix it.

- ### Can a malicious Office document in a disk image file (such as .iso and .vhd) circumvent blocking?
  ~~Yes. If the file format of a disk image file does not support NTFS alternate data stream, MOTW is not set for the files in the disk image file. Please see also the following:~~
  - [Mark-of-the-Web from a red team’s perspective](https://outflank.nl/blog/2020/03/30/mark-of-the-web-from-a-red-teams-perspective/) by Stan Hegt ([@stanhacked](https://twitter.com/stanhacked))
  - [The Dangers of VHD and VHDX Files](https://insights.sei.cmu.edu/blog/the-dangers-of-vhd-and-vhdx-files/) by Will Dormann ([@wdormann](https://twitter.com/wdormann))
  - [Subvert Trust Controls: Mark-of-the-Web Bypass](https://attack.mitre.org/techniques/T1553/005/) (an article in [MITRE ATT&CK](https://attack.mitre.org/) knowledge base).

  **Update on 11 April 2022:**  
  According to the blog article [.ISO Files With Office Maldocs & Protected View in Office 2019 and 2021](https://blog.didierstevens.com/2022/04/04/iso-files-with-office-maldocs-protected-view-in-office-2019-and-2021/) by Didier Stevens ([@DidierStevens](http://twitter.com/DidierStevens)), Office 2019 and 2021 use protected view to open Office document stored inside an ISO file with MOTW. This behavior was introduced in August 2021.

  **Update on 30 November 2022:**  
  According to [the tweet](https://twitter.com/BillDemirkapi/status/1590062146486140928) by Bill Demirkapi ([@BillDemirkapi](https://twitter.com/BillDemirkapi/)), Microsoft fixed handling of MOTW for virtual disk container files such as ISO and VHD on Windows by the security updates released on 8 November 2022. When applications open files inside a virtual disk container file downloaded from the Internet, the files will inherit the MOTW of the virtual disk container file.

## References
- Macros from the internet will be blocked by default in Office  
https://docs.microsoft.com/en-us/deployoffice/security/internet-macros-blocked

- Details about the Mark-of-the-Web (MOTW)  
https://nolongerset.com/mark-of-the-web-details/

- Downloads and the Mark-of-the-Web  
https://textslashplain.com/2016/04/04/downloads-and-the-mark-of-the-web/

- Mark-of-the-Web from a red team’s perspective  
https://outflank.nl/blog/2020/03/30/mark-of-the-web-from-a-red-teams-perspective/

- The Dangers of VHD and VHDX Files  
https://insights.sei.cmu.edu/blog/the-dangers-of-vhd-and-vhdx-files/

- Subvert Trust Controls: Mark-of-the-Web Bypass  
https://attack.mitre.org/techniques/T1553/005/

## Author
Nobutaka Mantani ([@nmantani](https://twitter.com/nmantani))
