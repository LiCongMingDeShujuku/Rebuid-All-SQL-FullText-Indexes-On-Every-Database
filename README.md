![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 在每个数据库上重建所有FullText索引
#### Rebuid All SQL FullText Indexes On Every Database
**发布-日期: 2014年07月14日 (评论)**

![#](images/##############?raw=true "#")

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
使用此脚本可以跨每个数据库重建所有FullText索引。将其设置为维护窗口上的重复作业是明智的。记得标准版不允许重建索引（在线=打开），因此在重建过程中FTI将不可用。如果你使用的是Enterprise Edition，则可以始终与（online = on）合并，因为Enterprise是支持的。


## English
Use this script to rebuild all FullText Indexes across every database. It would be wise to set this up as a repeated Job on a maintenance window. Remember; Standard Editions does not allow indexes to be rebuilt (online=on) so FTI’s will be unavailable during the course of the rebuild. If you are using Enterprise Edition you could always incorporate with (online=on) as this is supported for Enterprise.

---
## Logic
```SQL
use master;
set nocount on
 
declare @rebuild_all_fti varchar(max)
set @rebuild_all_fti = ''
select @rebuild_all_fti = @rebuild_all_fti
+
'use [' + name + ']; '
+ char(10) +
'declare @rebuild_fti_'
+ cast(database_id as varchar(200)) + ' varchar(255)' + char(10) +
'set @rebuild_fti_'
+ cast(database_id as varchar(200)) + ' = '''''
+ char(10) +
'select @rebuild_fti_'
+ cast(database_id as varchar(200)) + ' = @rebuild_fti_' + cast(database_id as varchar(200)) + ' + '''
+ char(10) +
'alter fulltext catalog '' + sftc.name + '' rebuild; ''
+ char(10) + char(10)
from sys.fulltext_catalogs as sftc order by [name] asc'
+ char(10) +
'exec (@rebuild_fti_' + cast(database_id as varchar(200)) + ')' + char(10) + char(10)
from sys.databases where name not in ('master', 'model', 'msdb', 'tempdb') order by name asc
 
exec (@rebuild_all_fti) --for xml path(''), type


```

希望这对你会有帮助(Hope this is useful.)


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

