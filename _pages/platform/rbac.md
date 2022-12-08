---
categories: basics
layout: article
title: Role-Based Access Control
---

On Algorithmia, resource access is controlled using roles and organizations. If you aren't familiar with how roles work on Algorithmia, you can review the glossary definitions for [cluster admin](/glossary#cluster-admin-account), [cluster user](/glossary#user-account), and [org admin](/glossary#org-admin-account).

In the table below, we use the terms **cluster user** and **org member** to differentiate between these roles in the **cluster** and **organization** contexts. However, note that an org member is just any account (cluster admin or cluster user) that's a member of an org. The fact that someone is a cluster admin or cluster user has no effect\* on their permissions to take actions on resources within the org. \*(Because a cluster admin can impersonate other users, including org admins, technically a cluster admin can do anything that an org admin can do. However, this is unrelated to the cluster admin's status in the org; it's just based on their being a cluster admin).

|*(Context)*|*(Cluster)*|*(Cluster)*||*(Organization)*|*(Organization)*|
|--- |--- |--- |--- |--- |--- |
|**Role**|**Cluster admin**|**Cluster user**||**Org admin**|**Org member**|
|||||||
|Access [browser UI](/glossary#browser-ui)|Y|Y||Y|Y|
|Access browser UI [admin panel](/glossary#admin-panel)|Y|N||N|N|
|Create cluster account invite code|Y|N||N|N|
|Share cluster account invite code|Y|N||N|N|
|Create [organization](/glossary#org-organization)|Y|Y||Y|Y|
|Invite user to org|N|N||Y|N|
|Create [reservation](/glossary#reservation)|Y|N||N|N|
|View cluster logs|Y|N||N|N|
|View algorithm error logs|Y|Y (N before v20.5.57)||Y (N before v20.5.57)|Y (N before v20.5.57)|
|View algorithm build logs|Y (w/ superpowers)|Y (their own algorithms)||Y (org-owned)|Y (org-owned)|
|Impersonate user|Y|N||N|N|
|Add sudo user|Y|N||N|N|
|Create [standard API key](/glossary#standard-api-key)|Y|Y||Y|Y|
|Create [admin API key](/glossary#admin-api-key)|Y|N||N|N|
|Upload publicly visible data|Y|Y||Y|Y|
|Upload privately visible data|Y|Y||Y|Y|
|Modify data visibility|Y|Y||Y|Y|
|Create org-owned algorithm|N|N||Y|Y|
|Create user-owned algorithm|Y|Y||Y|Y|
|Build/compile algorithm|Y|Y||Y|Y|
|Publish algorithm publicly|Y|Y||Y|N|
|Publish algorithm privately|Y|Y||Y|Y|
|Delete [publicly published algorithm](/glossary#public-algorithm)|Y|N||N|N|
|Delete [privately published algorithm](/glossary#private-algorithm)|Y|Y||Y|Y|
|Invoke algorithm|Y|Y||Y|Y|
|Provide algorithm-invoke access|Y|Y||Y|Y|
|Modify algorithm source code|Y (their own algorithms)|Y (their own algorithms)||N (user-owned), Y (org-owned)|Y (user-owned), Y (org-owned)|
|Modify algorithm settings|Y (their own algorithms)|Y (their own algorithms)||N (user-owned), Y (org-owned)|Y (user-owned), Y (org-owned)|
