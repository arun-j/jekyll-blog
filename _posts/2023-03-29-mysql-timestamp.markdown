---
layout: post
title: MySQL 8 Timestamp behaviour with 0000
---
# Checking with null in table structure
{% highlight ruby %}
mysql> select @@explicit_defaults_for_timestamp;
+-----------------------------------+
| @@explicit_defaults_for_timestamp |
+-----------------------------------+
| 1                                 |
+-----------------------------------+
1 row in set (0.00 sec)
{% endhighlight %}

{% highlight ruby %}
mysql> insert into test (id,updtd_dt) values (1,null);
Query OK, 1 row affected (0.02 sec)
mysql> select * from test;
+------+----------+
| id | updtd_dt   |
+------+----------+
| 1 | NULL        |
+------+----------+
1 row in set (0.00 sec)
{% endhighlight %}

{% highlight ruby %}
mysql> insert into test (id,updtd_dt) values (2,now());
Query OK, 1 row affected (0.01 sec)
mysql>
mysql> select * from test;
+------+---------------------+
| id | updtd_dt              |
+------+---------------------+
| 1 | NULL                   |
| 2 | 2023-03-08 05:47:15    |
+------+---------------------+
2 rows in set (0.00 sec)
{% endhighlight %}

{% highlight ruby %} 
mysql> insert into test (id) values (3);
Query OK, 1 row affected (0.01 sec)
mysql> select * from test;
+------+---------------------+
| id | updtd_dt |            |
+------+---------------------+
| 1 | NULL                   |
| 2 | 2023-03-08 05:47:15    |
| 3 | 2023-03-08 05:47:27    |
+------+---------------------+
3 rows in set (0.00 sec)
{% endhighlight %}

# Checking with not null in table structure

{% highlight ruby %}
mysql> CREATE TABLE `invoices_test` (
-> `id` int,
-> `updtd_dt` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP);
Query OK, 0 rows affected (0.04 sec)
{% endhighlight %}

{% highlight ruby %}
mysql> insert into invoices_test(id,updtd_dt) values (1,null);
ERROR 1048 (23000): Column 'updtd_dt' cannot be null
mysql> insert into invoices_test(id,updtd_dt) values (1,now());
Query OK, 1 row affected (0.00 sec)
{% endhighlight %}

{% highlight ruby %}
mysql> select * from invoices_test;
+------+---------------------+
| id | updtd_dt              |
+------+---------------------+
| 1 | 2023-03-08 05:54:29    |
+------+---------------------+
1 row in set (0.00 sec)
{% endhighlight %}

{% highlight ruby %}
mysql> insert into invoices_test(id) values (2);
Query OK, 1 row affected (0.00 sec)
mysql> select * from invoices_test;
+------+---------------------+
| id | updtd_dt              |
+------+---------------------+
| 1 | 2023-03-08 05:54:29    |
| 2 | 2023-03-08 05:54:52    |
+------+---------------------+
2 rows in set (0.00 sec)
{% endhighlight %}


# Disabling explicit_defaults_for_timestamp variable and checking structure with null

{% highlight ruby %}
mysql> select @@explicit_defaults_for_timestamp;
+-----------------------------------+
| @@explicit_defaults_for_timestamp |
+-----------------------------------+
| 1                                 |
+-----------------------------------+
1 row in set (0.00 sec)
{% endhighlight %}

{% highlight ruby %}
mysql> set explicit_defaults_for_timestamp=0;
Query OK, 0 rows affected, 1 warning (0.00 sec)
mysql> select @@explicit_defaults_for_timestamp;
+-----------------------------------+
| @@explicit_defaults_for_timestamp |
+-----------------------------------+
| 0                                 |
+-----------------------------------+
1 row in set (0.00 sec)
{% endhighlight %}
{% highlight ruby %}
mysql> CREATE TABLE `test` (
-> `id` int,
-> `updtd_dt` timestamp NULL DEFAULT CURRENT_TIMESTAMP);
Query OK, 0 rows affected (0.04 sec)
mysql> insert into test(id,updtd_dt) values (1,null);
Query OK, 1 row affected (0.01 sec)
{% endhighlight %}

{% highlight ruby %}
mysql> select * from test;
+------+----------+
| id | updtd_dt   |
+------+----------+
| 1 | NULL        |
+------+----------+
1 row in set (0.00 sec)
mysql> insert into test(id,updtd_dt) values (2,now());
Query OK, 1 row affected (0.01 sec)
{% endhighlight %}

{% highlight ruby %}
mysql> insert into test(id) values (3);
Query OK, 1 row affected (0.01 sec)
mysql> select * from test;
+------+---------------------+
| id | updtd_dt              |
+------+---------------------+
| 1 | NULL                   |
| 2 | 2023-03-08 05:58:38    |
| 3 | 2023-03-08 05:58:48    |
+------+---------------------+
3 rows in set (0.00 sec)
{% endhighlight %}
# Disabling explicit_defaults_for_timestamp variable and checking structure with not null

{% highlight ruby %}
mysql> CREATE TABLE `invoices_test` (
-> `id` int,
-> `updtd_dt` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP);
Query OK, 0 rows affected (0.04 sec)
{% endhighlight %}

{% highlight ruby %}
mysql> insert into invoices_test (id,updtd_dt) values (1,null);
Query OK, 1 row affected (0.01 sec)
{% endhighlight %}

{% highlight ruby %}
mysql> select * from invoices_test;
+------+---------------------+
| id | updtd_dt              |
+------+---------------------+
| 1 | 2023-03-08 06:00:29    |
+------+---------------------+
1 row in set (0.00 sec)
{% endhighlight %}

{% highlight ruby %}
mysql> insert into invoices_test (id,updtd_dt) values (2,now());
Query OK, 1 row affected (0.01 sec)
mysql> select * from invoices_test;
+------+---------------------+
| id | updtd_dt              |
+------+---------------------+
| 1 | 2023-03-08 06:00:29    |
| 2 | 2023-03-08 06:01:15    |
+------+---------------------+
2 rows in set (0.00 sec)
{% endhighlight %}

{% highlight ruby %}
mysql> insert into invoices_test (id) values (3);
Query OK, 1 row affected (0.01 sec)
mysql> select * from invoices_test;
+------+---------------------+
| id | updtd_dt              |
+------+---------------------+
| 1 | 2023-03-08 06:00:29    |
| 2 | 2023-03-08 06:01:15    |
| 3 | 2023-03-08 06:01:28    |
+------+---------------------+
3 rows in set (0.00 sec)
{% endhighlight %}