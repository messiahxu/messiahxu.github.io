---
layout: post
date: 2015-03-03 17:24:13
title: "how to install postgresql on ubuntu 12 04"
---

Create the file /etc/apt/sources.list.d/pgdg.list, and add a line for the repository

<pre>
deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main
</pre>

Import the repository signing key, and update the package lists

<pre>
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
</pre>

<pre>
sudo apt-get install postgresql-client postgresql
</pre>

初次安装后，默认生成一个名为 postgres 的数据库和一个名为 postgres 的数据库用户。这里需要注意的是，同时还生成了一个名为 postgres 的 Linux 系统用户。

新建一个 Linux 用户（我直接使用了当前用户）

<pre>sudo adduser new_user</pre>

然后切换到 postgres 用户。

<pre>sudo su - postgres</pre>

下面将使用 psql 登陆到 PostgreSQL。

<pre>psql</pre>

这里相当于是以系统用户 postgres 的身份，登陆到名为 postgres 的的数据库。如果一切正常，系统提示符会变为 "postgres=#"

然后这里可以为 postgres 用户设置一个密码（我没设）

<pre>\password postgres</pre>

然后创建数据库用户。

<pre>CREATE ROLE new_user WITH LOGIN CREATEDB PASSWORD 'lalala';</pre>

这里创建的数据库用户有登陆、创建数据库的权限，并且设置密码。

如果给用户赋予 superuser 权限，那么所有的权限将全部拥有。

<pre>CREATE ROLE new_user WITH SUPERUSER;</pre>

登出之后以新创建的用户登陆 psql。

<pre>
\q
exit
psql postgres # 这里是以 new_user 身份连接了 postgres 数据库
</pre>

其实 psql 命令就是下面的简化。

<pre>psql new_user</pre>

但因为，目前并不存在 new_user 这个数据库，所以只能先指定连接到其他数据库了。
然后可以创建一个与数据库用户同名的数据库，下次就可以直接通过 psql 登陆。

<pre>CREATE DATABASE new_user;</pre>

在数据库控制台里，切换连接的数据库。

<pre>\c new_user</pre>

恩，这样就差不多了。
