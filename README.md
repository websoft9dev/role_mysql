Ansible Role: mysql
=========

This role help you Install MySQL, add databases, add user and set default password

## Requirements

运行本 Role，请确认符合如下的必要条件：

| **Items**      | **Details** |
| ------------------| ------------------|
| Operating system | CentOS7.x Ubuntu18.04 AmazonLinux|
| Python 版本 | Python2  |
| Python 组件 |    |
| Runtime |  |


## Related roles

本 Role 在语法上不依赖其他 role 的变量，但程序运行时需要确保已经运行：common。以 mysql 为例：

```
  roles:
   - {role: role_common, tags: "role_common"}   
   - {role: role_cloud, tags: "role_cloud"}
   - {role: role_mysql, tags: "role_mysql"}
```


## Variables

本 Role 主要变量以及使用方法如下：

| **Items**      | **Details** | **Format**  | **是否初始化** |
| ------------------| ------------------|-----|-----|
| mysql_version | [ 5.5, 5.6, 5.7, 8.0 ] | 字符串 |是|
| mysql_root_password | [ "123456"] | 字符串 |是|
| mysql_remote | [ "true", "false" ] | 布尔型 |是|
| mysql_databases | []   | 字典 |否|
| mysql_users | []   | 字典 |否|
| mysql_configuration_extras | MySQL配置文件 健值对 | 字典队列 | 否 |

注意：
1. mysql_version, mysql_remote  的值在 mysql.yml 中由用户选择输入；
2. mysql_root_password，mysql_databases，mysql_users 的值在主变量文件[main.yml](https://github.com/Websoft9/ansible-mysql/blob/master/vars/main.yml)中定义。

## Example

### Init password
```
#1 Create databases
# defautl encoding and collation is: utf8mb4/utf8mb4_general_ci, and encoding must set with collation together

mysql_databases:
 - name: wordpress
   
mysql_databases:
 - name: wordpress
   encoding: utf8
   collation: utf8_general_ci
   
mysql_databases:
 - name: wordpress
 - name: joomla
   encoding: utf8
   collation: utf8_general_ci

#2 Create users
# default configuration: ( host: localhost | password: 123456 | priv: '*.*:USAGE')

mysql_users:
 - name: wordpress
   
mysql_users:
 - name: wordpress
   priv: 'wordpress.*:ALL'
   
mysql_users:
 - name: wordpress
   host: localhost
   priv: 'wordpress.*:ALL'
   
mysql_users:
 - name: wordpress
 - name: joomla
   host: localhost
   priv: 'wordpress.*:ALL'
   
#3 Set MySQL my.cnf

mysql_configuration_extras:
  - name: innodb_buffer_pool_size
    value: 2G
  - name: innodb_log_file_size
    value: 500M
  - name: init-connect
    value: "'SET NAMES utf8mb8'"
```

## Installation

```
git clone https://github.com/Websoft9/role_mysql.git
ansible-playbook role_mysql/tests/test.yml -e mysql_version="8.0" -e mysql_install_server="docker"
```

## License

[LGPL-3.0](/License.md), Additional Terms: It is not allowed to publish free or paid image based on this repository in any Cloud platform's Marketplace.

Copyright (c) 2016-present, Websoft9

## FAQ


