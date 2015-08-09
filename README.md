Zabbix Server Stress Test
=========================


Build
=====

[Download latest build (RHEL 7, CentOS 7, Ubuntu 14, ...)](https://drone.io/github.com/jangaraj/zabbix-server-stress-test/files/zabbix24/src/modules/zabbix_module_stress/zabbix_module_stress.so)
[![Build Status](https://drone.io/github.com/jangaraj/zabbix-server-stress-test/status.png)](https://drone.io/github.com/jangaraj/zabbix-server-stress-test/latest)<br>
If provided build doesn't work on your system, please see section [Compilation](#compilation). 

Available items
===============

| Key | Description |
| --- | ----------- |
| **stress.ping[<anything>]** | Return value is always int 1 |  
| **stress.echo[<message>]**  | Return value is string message from parameter |
| **stress.random[from,to]**  | Return value in defined range, e.g. **stress.random[1,1000]** | 

Installation
============

* Import provided template TODO.
* Configure your Zabbix agent(s) - load downloaded/compiled zabbix_module_stress.so<br>
https://www.zabbix.com/documentation/2.4/manual/config/items/loadablemodules


Compilation
===========

You have to compile module, if provided binary doesn't work on your system.
Basic compilation steps:

    cd ~
    mkdir zabbix24
    cd zabbix24
    svn co svn://svn.zabbix.com/branches/2.4 .
    ./bootstrap.sh
    ./configure --enable-agent
    mkdir src/modules/zabbix_module_stress
    cd src/modules/zabbix_module_stress
    wget https://raw.githubusercontent.com/jangaraj/zabbix-server-stress-test/master/src/modules/zabbix_module_stress/zabbix_module_stress.c
    wget https://raw.githubusercontent.com/jangaraj/zabbix-server-stress-test/master/src/modules/zabbix_module_stress/Makefile
    make

Output will be binary file (dynamically linked shared object library) zabbix_module_stress.so, which can be loaded by zabbix agent.

Author
======

[Devops Monitoring zExpert](http://www.jangaraj.com), who loves monitoring systems, which start with letter Z. Those are Zabbix and Zenoss. [LinkedIn] (http://uk.linkedin.com/in/jangaraj/).
