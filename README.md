# ToDo
- npm i -g npm
- ng build --watch

# Developer Notes
Notes for day to day work.

1. [C#](#c)
1. [Asp.Net Core](#aspnet-core)
1. [Azure](#azure)
1. [Git](#git)
1. [SQL Server](#sql-server)
1. [WordPress](#wordpress)
1. [Linux](#linux)
1. [Knockout.js](#knockoutjs)
1. [Other Handy Tools](#other-handy-tools)
1. [Visual Studio](#visual-studio)
1. [Other](#other)

## C# #
- Delegate: NotifyMoving?.Invoke(Id);
 
## Asp.Net Core
- _Layout.Mobile.cshtml
- [Asp.NET Core on Ubuntu - Multihosting](https://developingsoftware.com/aspnetcore-ubuntu#configure-nginx-as-a-reverse-proxy-to-asp.net-core) 
- [ASP.NET Core Multi-tenancy](http://benfoster.io/blog/aspnet-core-multi-tenancy-data-isolation-with-entity-framework)
- [Custom View Engine](http://weblogs.asp.net/imranbaloch/custom-viewengine-aspnet5-mvc6), [Custom Views](http://www.davepaquette.com/archive/2015/05/04/displaying-custom-asp-net-mvc-views-per-deployment.aspx)
- [Data Encryption](https://docs.asp.net/en/latest/security/data-protection/using-data-protection.html)
- [HangFire](http://hangfire.io/) - an easy way to perform background processing in .NET and .NET Core applications 
- [Login/Logout through REST](https://www.illucit.com/blog/2016/04/asp-net-5-mvc-6-identity-authentication/)
- [Mailkit](https://github.com/jstedfast/MailKit), [mail](http://stevejgordon.co.uk/how-to-send-emails-in-asp-net-core-1-0), [MailGun](http://benjii.me/2017/02/send-email-using-asp-net-core/)
- [Middleware - Sitemap](http://dotnetthoughts.net/generate-dynamic-xml-sitemaps-in-aspnet5)
- [Minify Html](https://github.com/deanhume/html-minifier)
- [Strongly Typed Configuration Settings](https://weblog.west-wind.com/posts/2016/May/23/Strongly-Typed-Configuration-Settings-in-ASPNET-Core)
- [Secret Manager Tool](http://www.fiyazhasan.me/dont-share-your-secrets-asp-net-core-secret-manager-tool)

## Azure
### App Service
- London - WEBSITE_TIME_ZONE:GMT Standard Time

## Git
- git fetch origin
- git merge origin/develop
- git tag -a v1.1.0.4rtm -m "Release version"
- git push origin master
- git push --tags
- git checkout develop *..switching*
- git config --global core.autocrlf true *..on Windows*

## SQL Server
- DBCC FREEPROCCACHE, DBCC DROPCLEANBUFFERS - reset cache
- STRING_SPLIT - returns table with a column named value (from SQL Server 2016)
```sql
SELECT * FROM STRING_SPLIT(‘Lorem ipsum dolor sit amet.’, ”)
```
- String split - temp alternative for small strings
```sql
SELECT Charindex(','+cast(id as varchar(8000))+',', @Ids)
```
- other handy bits
```sql
EXEC sp_change_users_login 'UPDATE_ONE','something','something'
EXEC sp_change_users_login 'REPORT'
CREATE CREDENTIAL YourNameCredential WITH IDENTITY='YourIdentity', SECRET='YourSecretKey'
BACKUP DATABASE DatabaseName TO URL = 'AzureUrl' WITH CREDENTIAL = 'YourNameCredential', FORMAT
RESTORE DATABASE DatabaseName FROM URL ='AzureUrl' WITH CREDENTIAL = 'YourNameCredential', STATS= 5
[, MOVE 'DatabaseName' to 'x:\data\DatabaseName.mdf' ,MOVE 'DatabaseNamelog' to 'x:\data\DatabaseName.ldf'] 
SELECT percent_complete, estimated_completion_time, * FROM sys.dm_exec_requests
```
- check index fragmentation
```sql
SELECT dbschemas.[name] as 'Schema',
dbtables.[name] as 'Table',
dbindexes.[name] as 'Index',
indexstats.avg_fragmentation_in_percent,
indexstats.page_count,
s.row_count
FROM sys.dm_db_index_physical_stats (DB_ID(), NULL, NULL, NULL, NULL) AS indexstats
INNER JOIN sys.tables dbtables on dbtables.[object_id] = indexstats.[object_id]
INNER JOIN sys.schemas dbschemas on dbtables.[schema_id] = dbschemas.[schema_id]
INNER JOIN sys.indexes AS dbindexes ON dbindexes.[object_id] = indexstats.[object_id]
AND indexstats.index_id = dbindexes.index_id
JOIN sys.dm_db_partition_stats s ON dbtables.object_id = s.object_id
	AND dbtables.type_desc = 'USER_TABLE'
	AND dbtables.name not like '%dss%'
	AND s.index_id IN (0,1)
WHERE indexstats.database_id = DB_ID()
	AND dbindexes.[name] IS NOT NULL
	AND avg_fragmentation_in_percent > 10
ORDER BY indexstats.avg_fragmentation_in_percent desc
```

## WordPress

[Ubuntu Installation](https://websiteforstudents.com/install-wordpress-on-ubuntu-16-04-17-10-18-04-with-apache2-mariadb-php-7-2-and-lets-encrypt-ssl-tls/)

Must plugins.

Plugin Name | Author | Note
---|---|---
My Private Site | David Gewirtz | to make it private, enable manually
WP Smush | WPMU DEV | to optimise images
Revision Control | Dion Hulse | to have only a few revisions
BackUpWordPress | Human Made Limited | WordPress Database Backup (Email, scheduled)
~~WP-DB-Backup~~ | Austin Matzko | WordPress Database Backup  (Email, scheduled)
Transient Cleaner | Code Art | Clean expired transients from your options table
Delete Expired Transients || crap stored as posts in db backup
Antivirus | Sergej Muller |
Check Email | |

## Linux
- sudo command
- df [-h] *..disk information*
- yum install htop, check-update | apt update, apt upgrade, [apt full-upgrade], apt autoremove
- apt-get autoremove (removes unused packages, cleans boot partition)
- reboot -h now
- systemctl start/restart/status/stop/enable mariadb/waagent
- tab+tab auto completition
- midnight commander - Norton like Commander
- ls -la list details
- ip addr show
- sudo -s (on Azure Ubuntu to switch to root)
- putty - Shift + Insert - Paste text

### Vi Editor
 - sudo vi /var/log/mariadb/mariadb.log 
 - G - end of file
 - :q! - type to exit type
 - ZZ - exit & save
 - x - delete selected character
 - dd - delete line
 
### cron
```30 1 * * * root /bin/systemctl stop httpd.service && (/opt/letsencrypt/letsencrypt-auto renew | tee -a /var/log/letsencrypt-renew.log) && /bin/systemctl start httpd.service```

### Knockout.js
- debug: ko.dataFor($0)

### Other Handy Tools
- http://hangfire.io/overview.html create, process and manage your background jobs
- http://fontello.com/ font extract/combine
- http://www.glyphrstudio.com/ edit svg font

### Visual Studio
- .editorconfig - overrides personal preferences, use in teams

### Other
- Print Spooler - restart this service
- Windows Audio
- Feedback Polls - [HotJjar] (https://www.hotjar.com/polls)
- Uninstall Telementry - rundll32 "%PROGRAMFILES%\NVIDIA Corporation\Installer2\InstallerCore\NVI2.DLL",UninstallPackage NvTelemetryContainer
