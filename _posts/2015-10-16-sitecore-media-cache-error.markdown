---
layout: post
title: Sitecore Media Cache Error
date: '2015-10-16 10:55:16'
tags:
- impersonate
- access-denied
- network-service
- iusr
- sitecore
---


I recently ran into an instance of Sitecore where the logs were riddled with the below Media Cache “Access to path is denied” errors

 
> 6788 12:15:51 ERROR Could not add media to cache. Media item id: {75CF2D38-8147-49C8-BDB8-69F9341F541D} Exception: System.UnauthorizedAccessException Message: Access to the path '[projectname]\Website\App_Data\MediaCache\[projectname]\58' is denied. Source: mscorlib at System.IO.__Error.WinIOError(Int32 errorCode, String maybeFullPath) at System.IO.Directory.InternalCreateDirectory(String fullPath, String path, Object dirSecurityObj, Boolean checkHost) at System.IO.Directory.InternalCreateDirectoryHelper(String path, Boolean checkHost) at Sitecore.IO.FileUtil.OpenFileStream(String filePath, FileMode mode, FileAccess access, FileShare share, Boolean createFolder, Boolean logErrors) at Sitecore.IO.FileUtil.OpenCreate(String filePath) at Sitecore.IO.StreamSharer..ctor(Stream masterStream, String bufferFilePath) at Sitecore.Resources.Media.MediaCacheRecord.Initialize(Media sitecoreMedia, MediaOptions mediaOptions, MediaStream mediaStream) at Sitecore.Resources.Media.MediaCache.CreateCacheRecord(Media media, MediaOptions options, MediaStream stream) at Sitecore.Resources.Media.MediaCache.AddStream(Media media, MediaOptions options, MediaStream stream, MediaStream&amp; cachedStream) at Sitecore.Resources.Media.Media.AddStreamToCache(MediaOptions options, MediaStream mediaStream, MediaStream&amp; cachedStream)

Time to investigate!


## File System Permissions

My first thoughts were to double check that all permissions are setup correctly.  First, I went through the instructions in the Installation Guide, which can be found on </span>[http://dev.sitecore.net](http://dev.sitecore.net)site when you download the latest version of Sitecore.   

The guide defines the following requirements:

#### File System Permission for Anonymous Request

Default Anonymous Internet user account name: **IUSR**

Permissions: *Read *permissions to all files, folders, and subfolders under the **/Website **folder.

#### File System Permissions for ASP.NET Request

Default ASP.NET account name: **Network Service**

Permissions: *Modify *permissions to all files, folders, and subfolders, under **/Website **and **/Data** folders.

The instance I was troubleshooting had these permissions setup correctly, so I continued my investigation.


## Solution

We used [Process Monitor Tool](http://technet.microsoft.com/en-us/sysinternals/bb896645.aspx) to give us back some more detailed information on file system activity.  This revealed that when trying to create the Media Cache files it was using the **IUSR** account rather than the **Network Service** account.  This was strange, and after some more investigation I found that the web.config had this setting

 
`<identity impersonate="true"/>`

This setting by default is set to false.

### Impersonation Explained

ASP.NET impersonation is used when you want your application to run under a different security context.  In this case the Application runs with the visitor’s account, which is **IUSR**.  The application now won’t have the ability to modify files in the Website folder.  As a result, it can’t create the Media Cache files, and thus throws these errors.  It turns out that the application didn’t need this setting set to *true*, so after setting it to *false *the errors went away and the application behaved as it should.

Lesson learned.


