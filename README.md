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

## Comparison table of MOTW propagation support (as of 23 September 2023)
|Name|Tested version|License|MOTW propagation|Enabled by default|Note|
|----|--------------|-------|----------------|------------------|----|
|"Extract all" built-in function of Windows Explorer|Windows 11 22H2<br>Windows 10 22H2|proprietary|Yes :heavy_check_mark:|Yes :heavy_check_mark:|MOTW bypass vulnerabilities (fixed) <a href="#*1">*1</a>|
|[7-Zip](https://www.7-zip.org/)|23.01|GNU LGPL|Yes :heavy_check_mark:|No :x: <a href="#*2">*2</a>||
|[Bandizip](https://en.bandisoft.com/bandizip/)|Standard Edition 7.32|freeware|Yes :heavy_check_mark:|Yes :heavy_check_mark:|MOTW bypass vulnerability (fixed) <a href="#*3">*3</a><br>Only for specific file extensions <a href="#*4">*4</a>|
|[CubeICE](https://www.cube-soft.jp/cubeice/)|3.1.0|freeware / proprietary|Yes :heavy_check_mark:|Yes :heavy_check_mark:|MOTW bypass vulnerability (fixed) <a href="#*5">*5</a>|
|[Explzh](https://www.ponsoftware.com/en/)|9.12|proprietary for commercial use|Yes :heavy_check_mark:|Yes :heavy_check_mark:||
|[NanaZip](https://github.com/M2Team/NanaZip)|2.0.450.0|MIT|Yes :heavy_check_mark:|No :x: <a href="#*6">*6</a>||
|[PeaZip](https://peazip.github.io/)|9.4.0|GNU LGPL|Yes :heavy_check_mark:|Yes :heavy_check_mark:||
|[TC4Shell](https://www.tc4shell.com/)|21.3.0 (trial)|proprietary|Yes :heavy_check_mark:|Yes :heavy_check_mark:||
|[Total Commander](https://www.ghisler.com/)|11.01 (trial)|proprietary|Yes :heavy_check_mark:|Yes :heavy_check_mark:||
|[WinRAR](https://www.win-rar.com/)|6.24 beta 1 (trial)|proprietary|Yes :heavy_check_mark:|Yes :heavy_check_mark:|Only for specific file extensions <a href="#*7">*7</a>|
|[WinZip](https://www.winzip.com/)|28.0 (trial)|proprietary|Yes :heavy_check_mark:|Yes :heavy_check_mark:|MOTW is propagated only if ZoneId value of the MOTW is 4 (Untrusted sites) <a href="#*8">*8</a>|
|[Ashampoo ZIP Free](https://www.ashampoo.com/en-us/zip-free)|1.0.7|freeware (registration required)|No :x:|||
|[CAM UnZip](https://www.camunzip.com/)|5.22.6.0|proprietary for commercial use|No :x:|||
|Expand-Archive cmdlet of [PowerShell](https://github.com/PowerShell/PowerShell/)|7.3.6|MIT|No :x:|||
|[Express Zip](https://www.nchsoftware.com/zip/)|10.23|proprietary for commercial use|No :x:|||
|[File Compact](https://www.sourcenext.com/product/pc/oth/pc_oth_001267/)|7.02|proprietary|No :x:|||
|[IZArc](https://www.izarc.org/)|4.5|freeware|No :x:|||
|[LhaForge](https://claybird.sakura.ne.jp/garage/lhaforge/index.html)|1.6.7|MIT|No :x:|||
|[Lhaplus](http://hoehoe.com/)|1.74|freeware|No :x:|||
|[PowerArchiver](https://www.powerarchiver.com/)|22.00.09 (trial)|proprietary|No :x:|||
|[StuffIt Expander](https://stuffit.com/)|15.0.8|freeware|No :x:|||
|[tar.exe (bsdtar)](https://github.com/libarchive/libarchive) of Windows 11 and Windows 10|3.5.2|BSD 2-clause|No :x:|||
|[Universal Extractor 2](https://github.com/Bioruebe/UniExtract2)|2.0.0 RC 3|GNU GPLv2|No :x:|||
|[ZipGenious](https://zipgenius.com/)|6.3.2.3116|freeware|No :x:|||
|[Zipware](https://www.zipware.org/)|1.6|freeware|No :x:|||

<a id="*1">*1</a>: There were two MOTW bypass vulnerabilities of Windows and they were fixed by the security updates released on 8 November 2022.
- [CVE-2022-41049](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2022-41049) ([Twitter thread](https://twitter.com/wdormann/status/1544431763358875648) by Will Dormann ([@wdormann](https://twitter.com/wdormann)) and [detailed writeup](https://breakdev.org/zip-motw-bug-analysis/) by Kuba Gretzky ([@mrgretzky](https://twitter.com/mrgretzky)))
- [CVE-2022-41091](https://msrc.microsoft.com/update-guide/en-us/vulnerability/CVE-2022-41091) ([Twitter thread](https://twitter.com/wdormann/status/1587183012755685380) by Will Dormann ([@wdormann](https://twitter.com/wdormann)))

<a id="*2">*2</a>: Though 7-Zip has supported MOTW propagation since version 22.00, it is disabled by default. You can enable it for 7-Zip GUI with the "Propagate Zone Id stream:" option in "Tools" -> "Options" -> "7-Zip" of 7-Zip File Manager.

![images/7-zip-setting.png](images/7-zip-setting.png)

When you set the option to Yes, 7-Zip propagate MOTW to all extracted files. When you set it to "For Office files", 7-Zip propagate MOTW to files with the following file extensions:
- .doc .docb .docm .docx .dot .dotm .dotx .wbk .wll .wwl
- .pot .potm .potx .ppa .ppam .pps .ppsm .ppsx .ppt .pptm .pptx .sldm .sldx
- .xla .xlam .xlm .xls .xlsb .xlsm .xlsx .xlt .xltm .xltx

You can also enable MOTW propagation by setting the registry HKEY_CURRENT_USER\SOFTWARE\7-Zip\Options\WriteZoneIdExtract DWORD to 1.

For 7-Zip CLI, -snz switch is required to propagate MOTW regardless of the option above.

<a id="*3">*3</a>: There was a MOTW bypass vulnerability of Bandizip and it was fixed in Bandizip 7.29 released on 21 November 2022 ([release note](https://en.bandisoft.com/bandizip/history/)). The vulnerability is almost the same as CVE-2022-41049 of Windows (<a href="#*1">*1</a>) and it can be exploited by just setting read-only file attribute to ZIP file contents. I found the vulnerability and reported it to Bandisoft, the developer of Bandizip. Bandisoft fixed it very quickly.

<a id="*4">*4</a>: Accoring to [the document of Bandizip](https://www.bandisoft.com/bandizip/help/zone-identifier/), Bandizip propagates MOTW to files with the following file extensions:
- .exe .com .msi .scr .bat .cmd .pif .bat .lnk
- .zip .zipx .rar .7z .alz .egg .cab .bh
- .iso .img .isz .udf .wim .bin .i00
- .js .jse .vbs .vbe .wsf
- .url .reg
- .docx .doc .xls .xlsx .ppt .pptx .wiz

I previously tested Bandizip with a ZIP archive file that contained only text files, and I misunderstood that Bandizip does not propagate MOTW.

<a id="*5">*5</a>: CubeICE has supported MOTW propagation since version 3.0.0, but this version had a MOTW bypass vulnerability. The vulnerability was fixed in version 3.0.1 released on 5 April 2023 ([release note](https://clown.cube-soft.jp/entry/2023/04/03/cubeice-3.0.0-or-later)). The vulnerability is almost the same as CVE-2022-41049 of Windows (<a href="#*1">*1</a>) and it can be exploited by just setting read-only file attribute to ZIP file contents. I found the vulnerability and reported it to CubeSoft, the developer of CubeICE. CubeICE fixed it very quickly.

<a id="*6">*6</a>: Though NanaZip has supported MOTW propagation since version 2.0 Preview 1, it is disabled by default. You can enable it with the "Propagate Zone Id stream:" option in "Tools" -> "Options" -> "Integration" of NanaZip GUI.

When you set the option to Yes, NanaZip propagate MOTW to all extracted files. When you set it to "For Office files", NanaZip propagate MOTW to files with the following file extensions:
- .doc .docb .docm .docx .dot .dotm .dotx .wbk .wll .wwl
- .pot .potm .potx .ppa .ppam .pps .ppsm .ppsx .ppt .pptm .pptx .sldm .sldx
- .xla .xlam .xlm .xls .xlsb .xlsm .xlsx .xlt .xltm .xltx

<a id="*7">*7</a>: Jernej Simončič ([@jernej__s](https://twitter.com/jernej__s)) kindly contacted the developer of WinRAR and got [the answer](https://github.com/nmantani/archiver-MOTW-support-comparison/issues/1) that WinRAR propagates MOTW only to Microsoft Office document files. It seems that the supported file types are not documented. I did additional tests with WinRAR 6.11 and confirmed that it propagates MOTW to document files of Word, Excel, and PowerPoint (files of Access and Publisher are not supported).

I previously tested WinRAR with a ZIP archive file that contained only text files, and I misunderstood that WinRAR does not propagate MOTW.

<a id="*8">*8</a>: WinZip removes MOTW from archive files on extraction if the ZoneId value of the MOTW is 3 (Internet). This behavior was introduced in version 28.0.

## Comparison table of MOTW propagation behavior (as of 19 September 2023)
|Name|Tested version|MOTW propagation behavior|
|----|--------------|-------------------------|
|"Extract all" built-in function of Windows Explorer|Windows 11 22H2<br>Windows 10 22H2|<ul><li>MOTW is propagated only if ZoneId value of the MOTW is 3 (Internet) or 4 (Untrusted sites)</li><li>ZoneId field of the archive file is inherited</li><li>The absolute path of the archive file is set for the ReferrerUrl field</li><li>All other fields are ignored</li></ul>|
|[7-Zip](https://www.7-zip.org/)|23.01|<ul><li>MOTW of the archive file is propagated without modification</li><li>Only for specific file extensions if the "Propagate Zone Id stream:" option is set to "For Office files" <a href="#*2">*2</a></li></ul>|
|[Bandizip](https://en.bandisoft.com/bandizip/)|Standard Edition 7.32|<ul><li>MOTW of the archive file is propagated without modification</li><li>Only for specific file extensions <a href="#*4">*4</a></li></ul>|
|[CubeICE](https://www.cube-soft.jp/cubeice/)|3.1.0|<ul><li>MOTW is propagated only if ZoneId value of the MOTW is 3 (Internet) or 4 (Untrusted sites)</li><li>Only ZoneId field of the archive file is inherited and all other fields are ignored</li></ul>|
|[Explzh](https://www.ponsoftware.com/en/)|9.12|<ul><li>MOTW is propagated only if ZoneId value of the MOTW is 3 (Internet)</li><li>Only ZoneId field of the archive file is inherited and all other fields are ignored</li></ul>|
|[NanaZip](https://github.com/M2Team/NanaZip)|2.0.450.0|<ul><li>MOTW of the archive file is propagated without modification</li><li>Only for specific file extensions if the "Propagate Zone Id stream:" option is set to "For Office files" <a href="#*6">*6</a></li></ul>|
|[PeaZip](https://peazip.github.io/)|9.4.0|<ul><li>MOTW of the archive file is propagated without modification</li></ul>|
|[TC4Shell](https://www.tc4shell.com/)|21.3.0 (trial)|<ul><li>Only ZoneId field of the archive file is inherited and all other fields are ignored</li></ul>|
|[Total Commander](https://www.ghisler.com/)|10.52 (trial)|<ul><li>MOTW of the archive file is propagated except for the ReferrerUrl field</li></ul>|
|[WinRAR](https://www.win-rar.com/)|6.24 beta 1 (trial)|<ul><li>Only ZoneId field of the archive file is inherited and all other fields are ignored</li><li>Only for specific file extensions <a href="#*7">*7</a></li></ul>|
|[WinZip](https://www.winzip.com/)|28.0 (trial)|<ul><li>MOTW is propagated only if ZoneId value of the MOTW is 4 (Untrusted sites)</li><li>ZoneId field of the archive file is inherited</li><li>The absolute path of the archive file is set for the ReferrerUrl field</li><li>All other fields are ignored</li><li>MOTW is removed from archives files on extraction if the ZoneId value of the MOTW is 3 (Internet)<a href="#*8">*8</a></li></ul>|

### MOTW propagation examples
In these examples, MOTW was manually set for a ZIP archive file motw-test.zip with Set-MOTW.ps1, then MOTW of an extracted file is displayed with Get-MOTW.ps1. Set-MOTW.ps1 and Get-MOTW.ps1 are available at my [PS-MOTW](https://github.com/nmantani/PS-MOTW) repository.

- MOTW of a file extracted with Windows Explorer or WinZip (version 27.0 or earlier):
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
