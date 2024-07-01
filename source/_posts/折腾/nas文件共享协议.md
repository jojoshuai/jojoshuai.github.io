---
title: nas文件共享协议
date: 2024-08-15 20:06:17
categories:
  - 折腾
tags:
  - smb
  - nfs
  - webdav
  - ftp
---

文件共享协议

> 包含文件的列表、添加、删除、属性获取(文件大小、创建时间、修改时间等)等管理操作。

1. smb 协议
   `SMB`（Server Messages Block，信息服务块）是一种在局域网上共享文件和打印机等资源的通信协议。SMB 协议是 C/S（客户机/服务器）协议，客户机通过该协议可以访问服务器上的共享文件系统、打印机及其他资源。这使得局域网内的不同计算机之间能够轻松共享文件及打印机等资源，提高了工作效率和便利性。

   服务信息块协议 SMB（Service Message Block），支持范围广，安卓/win/mac/linux 都有原生支持，挂载到系统上方便，几乎无感的访问文件。

   `Samba`作为 SMB 协议的一种`实现方法`，主要用来实现 Linux 系统的文件和打印服务。通过设置 Samba 服务器，Linux 用户可以实现与 Windows 用户的资源共享。Samba 由服务器及客户端程序构成，其中 smbd 和 nmbd 是 Samba 的核心进程。smbd 是 Samba 的 SMB 服务器，它使用 SMB 协议与客户端链接，完成用户认证、权限管理和文件共享服务。而 nmbd 则提供 NetBIOS 名字服务器的守护进程，可以帮助客户定位服务器和域。

   速度快

   > 参考：
   > https://xstarcd.github.io/wiki/Linux/samba.html
   > https://www.cnblogs.com/mchina/archive/2012/12/18/2816717.html

2. nfs 协议

   网络文件系统协议（Network File System），是 TCP/IP 协议集所提供的一种子协议，已经成为 UNIX/LINUX 类系统的标配。

   速度快，作为标配，读写速度相对 smb 更快一些

3. webdav 协议

   基于 HTTP1.1 协议的拓展升级版（Web-based Distributed Authoring and Versioning），可以很方便的跨越网关、防火墙。使得在公网上访问特别简单，并且通过 https 的公网访问拥有非常棒的安全性。·

   速度慢

4. ftp 协议

   文件传输协议（File Transfer Protocol）, 经典的文件共享协议。 为了补足 FTP 不安全的特性，又有 FTPS（FTP over SSL）、SFTP（SSH 文件传输协议）两种加密传输方式。 部分可参考 http 与 https 的关系。

   速度慢
