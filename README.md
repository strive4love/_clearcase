# ClearCase基本概念
1. VOB--Versioned Object Base, ClearCase将所有管理的文件的各种版本都存储在这个VOB中，VOB可以看作是整个ClearCase SCM系统的中心数据库。

2. View--View分为SnapShot View和Dynamic View，Snapshot view是clearcase在服务器上存储的文件和目录的一个本地镜像，用户可以在本地进行修改，然后进行同步，要经常Update View保持最新的版本，Dynamic View是动态试图，他并不在本地存储任何文件，始终和服务器保持一致。

3. Reserved checkout vs unreserved checkout -- 一个文件可以被多个用户的多个view来unreserved checkout，但是同时只能有一个用户reserved checkout。当一个用户reserved checkout的时候，其他unreserved checkout的文件，不能checkin，只能等reserved checkout的文件被checkin之后才能够checkin。

4. hijacked文件--当用户创建Snapshot View的时候，本地的文件属性都是只读的，如果用户没有check out的情况下就对文件进行了修改，这个文件就为hijacked文件，此时这个文件已经脱离的ClearCase的控制，所以最好不要Hijacked 文件。

5. mastership--很多情况下，ClearCase都被部署为MultiSite的形式，特别是跨地域开发的时候，每个地方的开发人员都在本地的一个VOB副本上工作，叫做replica, ClearCase负责同步这些不同的VOB。为了避免冲突，ClearCase提供了一个排他修改的属性，叫做mastership。所有的VOB对象都有一个master replica。master replica 对这个对象有排他的修改操作权限，因此对于一个VOB对象只有master replica才能对他进行修改或删除。所以在你把新的文件或目录Add to source control的时候，最好要选择 “Make current replica the master of all newly created branches”。

6. merge文件--当一个用户check out一个文件进行了修改，在check in的时候如果clearcase发现这个文件和最新的版本有冲突的时候（可能是其他用户也对该文件进行了修改并已经check in），会提示要merge文件，这时候就可能需要手工的merge了。

# ClearCase的四种功能
## Workspace Management
