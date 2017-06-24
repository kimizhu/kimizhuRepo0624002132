--- 
TOCTitle: Junction
Title: Junction
layout: LandingPage
ms:assetid: '16f763c0-cb78-4d67-a865-63e79bef0c58'
ms:mtpsurl: 'https://technet.microsoft.com/en-us/Bb896768(v=MSDN.10)'
---

Junction v1.07
==============

**By Mark Russinovich**

Published: July 4, 2016

**[![](/media/landing/sysinternals/download_sm.png)
 Download
Junction](https://download.sysinternals.com/files/junction.zip)  
(212 KB)**


## Introduction

Windows 2000 and higher supports directory symbolic links, where a
directory serves as a symbolic link to another directory on the
computer. For example, if the directory D:\\SYMLINK specified
C:\\WINNT\\SYSTEM32 as its target, then an application accessing
D:\\SYMLINK\\DRIVERS would in reality be accessing
C:\\WINNT\\SYSTEM32\\DRIVERS. Directory symbolic links are known as NTFS
junctions in Windows. Unfortunately, Windows comes with no tools for
creating junctions—you have to purchase the Win2K Resource Kit, which
comes with the linkd program for creating junctions. I therefore decided
to write my own junction-creating tool: *Junction*. *Junction* not only
allows you to create NTFS junctions, it allows you to see if files or
directories are actually reparse points. Reparse points are the
mechanism on which NTFS junctions are based, and they are used by
Windows' Remote Storage Service (RSS), as well as volume mount points.

Please read this [Microsoft KB
article](http://support.microsoft.com/?kbid=205524) for tips on using
junctions.

***Note**that Windows does not support junctions to directories on
remote shares.*

If you want to view reparse information, the usage for *Junction* is the
following:

 

## Using Junction

Use junction to list junctions:

**Usage: \[-s\]**

**-s    **Recurse subdirectories

**Examples:**

To determine if a file is a junction, specify the file name:

**junction c:\\test**

To list junctions beneath a directory, include the –s switch:

**junction -s c:\\**

To create a junction c:\\Program-Files for "c:\\Program Files":

**C:\\&gt;md Program-Files**

**C:\\&gt;junction c:\\Program-Files "c:\\Program Files"**

To delete a junction, use the –d switch:

**junction -d c:\\Program-Files**

  

[![No](/media/landing/sysinternals/download_sm.png "Download")
](https://download.sysinternals.com/files/junction.zip)

[**Download Junction**  
](https://download.sysinternals.com/files/junction.zip)**(212 KB)**

  
  


<div>

<div class="RightAdRail">


## Download

[![No](/media/landing/sysinternals/download_sm.png "Download")
](https://download.sysinternals.com/files/junction.zip)

[  
**Download
Junction**](https://download.sysinternals.com/files/junction.zip)  
**(212 KB)**

  
**Runs on:**

-   Client: Windows Vista and higher
-   Server: Windows Server 2008 and higher
-   Nano Server: 2016 and higher



