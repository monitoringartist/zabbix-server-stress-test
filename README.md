# Zabbix Server Stress Test

This is basic synthetic stress test. It's more complicated practically, e.g. you 
have triggers, which need to be evaluated, .... It provides basic picture about 
performance of your Zabbix server.

Please donate to author, so he can continue to publish other awesome projects 
for free:

[![Paypal donate button](http://jangaraj.com/img/github-donate-button02.png)]
(https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=8LB6J222WRUZ4)

# Instructions

**1. Agent setup**

You will need a few remote agents for monitoring. One agent can provide 5k nvps 
typically. You need to verify this value before testing.
First you need to setup agent to load provided [zabbix_module_stress.so]
(https://drone.io/github.com/monitoringartist/zabbix-server-stress-test/files/zabbix24/src/modules/zabbix_module_stress/zabbix_module_stress.so) 
module (for RHEL 7, CentOS 7, Ubuntu 14, ...).
[![Build Status](https://drone.io/github.com/monitoringartist/zabbix-server-stress-test/status.png)]
(https://drone.io/github.com/monitoringartist/zabbix-server-stress-test/latest)

Basically you need to setup 2 agent settings:

```
LoadModulePath=<absolute_path_to_stress_module>/
LoadModule=zabbix_module_stress.so
``` 

Restart Zabbix agent and check Zabbix agent log for any module problems (e.g. 
problem with read permission).
See official [Zabbix module doc]
(https://www.zabbix.com/documentation/2.4/manual/config/items/loadablemodules) 
for more information.

You have to compile module, if the provided binary module doesn't work on your system.
Basic compilation steps:

    # Required CentOS/RHEL tools: yum install -y svn autoconf automake gcc
    # Required Debian/Ubuntu tools: apt-get install -y wget autoconf automake gcc subversion make pkg-config
    cd ~
    mkdir zabbix30
    cd zabbix30
    svn co svn://svn.zabbix.com/branches/3.0 .
    ./bootstrap.sh
    ./configure --enable-agent
    mkdir src/modules/zabbix_module_stress
    cd src/modules/zabbix_module_stress
    wget https://raw.githubusercontent.com/monitoringartist/zabbix-server-stress-test/master/src/modules/zabbix_module_stress/zabbix_module_stress.c
    wget https://raw.githubusercontent.com/monitoringartist/zabbix-server-stress-test/master/src/modules/zabbix_module_stress/Makefile
    make

Output will be a binary file (dynamically linked shared object library) 
zabbix_module_stress.so, which can be loaded by Zabbix agent.

When stress module is loaded, you should execute [Zabbix agent stress test]
(https://github.com/monitoringartist/zabbix-agent-stress-test) to determine 
how many npvs can be provided by the agent. It varies based on the used HW 
and network latency and agent config (e.g. *StartAgents* setting).

```
[root@zabbix-server]# ./zabbix-agent-stress-test.py -s <remote_agent_ip> -k "stress.ping[]" -t 20
Warning: you are starting more threads, than your system has available CPU cores (8)!
Starting 20 threads, host: remote_agent_ip:10050, key: stress.ping[]
Success: 6077   Errors: 0       Avg rate: 6201.08 qps   Execution time: 1.00 sec
Success: 12825  Errors: 0       Avg rate: 7070.72 qps   Execution time: 2.00 sec
Success: 19712  Errors: 0       Avg rate: 13519.47 qps  Execution time: 3.00 sec
Success: 26441  Errors: 0       Avg rate: 7238.94 qps   Execution time: 4.01 sec
Success: 33281  Errors: 0       Avg rate: 7881.25 qps   Execution time: 5.01 sec
Success: 40119  Errors: 0       Avg rate: 5957.95 qps   Execution time: 6.01 sec
Success: 46933  Errors: 0       Avg rate: 7150.82 qps   Execution time: 7.01 sec
Success: 53755  Errors: 0       Avg rate: 8467.07 qps   Execution time: 8.01 sec
Success: 60620  Errors: 0       Avg rate: 10828.77 qps  Execution time: 9.01 sec
Success: 67403  Errors: 0       Avg rate: 5495.64 qps   Execution time: 10.01 sec
Success: 74280  Errors: 0       Avg rate: 9848.34 qps   Execution time: 11.01 sec
Success: 81112  Errors: 0       Avg rate: 8552.36 qps   Execution time: 12.02 sec
Success: 87859  Errors: 0       Avg rate: 6019.28 qps   Execution time: 13.02 sec
Success: 94707  Errors: 0       Avg rate: 7011.25 qps   Execution time: 14.02 sec
Success: 101561 Errors: 0       Avg rate: 9591.18 qps   Execution time: 15.02 sec
Success: 108420 Errors: 0       Avg rate: 7460.78 qps   Execution time: 16.02 sec
Success: 115348 Errors: 0       Avg rate: 6783.12 qps   Execution time: 17.02 sec
Success: 122293 Errors: 0       Avg rate: 5833.30 qps   Execution time: 18.02 sec
Success: 128981 Errors: 0       Avg rate: 6648.71 qps   Execution time: 19.02 sec
Success: 135696 Errors: 0       Avg rate: 7053.04 qps   Execution time: 20.03 sec
Success: 142320 Errors: 0       Avg rate: 5939.79 qps   Execution time: 21.03 sec
Success: 148962 Errors: 0       Avg rate: 5824.79 qps   Execution time: 22.03 sec
Success: 155639 Errors: 0       Avg rate: 8174.85 qps   Execution time: 23.03 sec
Success: 162433 Errors: 0       Avg rate: 12415.46 qps  Execution time: 24.03 sec
Success: 169191 Errors: 0       Avg rate: 8027.23 qps   Execution time: 25.03 sec
Success: 175772 Errors: 0       Avg rate: 10819.94 qps  Execution time: 26.03 sec
Success: 182565 Errors: 0       Avg rate: 6678.57 qps   Execution time: 27.04 sec
Success: 189289 Errors: 0       Avg rate: 15228.94 qps  Execution time: 28.04 sec
Success: 196026 Errors: 0       Avg rate: 8424.91 qps   Execution time: 29.04 sec
Success: 202789 Errors: 0       Avg rate: 7598.61 qps   Execution time: 30.04 sec
^C
Success: 482513 Errors: 0       Avg rate: 5792.09 qps   Execution time: 20.70 sec
Avg rate based on total execution time and success connections: 6637.30 qps
```

This agent can provide on avg 6637.30 new values (queries) per second for Zabbix 
server stress test. Calculate with a lower value, e.g. 5k value to be safe.  
Calibrate another agent, so sum of save agent values will be at least equal to 
required Zabbix server. It means that you will need 10 agents with similar 
performance with 5k qps for Zabbix server 50k nvps stress test. Don't use Zabbix 
agent on localhost of your Zabbix server.

**2. Prepare performance metrics**

Application template for Zabbix server is provided out of the box. Be sure, that 
template is assigned and data are collected. 
You will need also OS performance metrics (CPU, mem, HDD, ...), so also OS 
template (usually OS Linux) should be assigned and metrics should be collected. 
Database server is usually the bottleneck of Zabbix, so be prepared for detailed DB 
monitoring. Use proper DB template and especially watch IOPs metrics (*iostat -xd 10 10*)

Screen for Zabbix performance overview is recommended. Example:

![Zabbix server performance screen]
(https://raw.githubusercontent.com/monitoringartist/zabbix-server-stress-test/master/doc/zabbix-server-screen.png)

**3. Import and link provided stress templates**

Stress templates contain a huge number of items collected every second - they 
generate stress. If you have import error "File is too big, max upload size is 
nnnnn bytes.", then you need to increase the php config *upload_max_filesize* value - 
at least 10MB and *memory_limit* value - at least 512MB (Don't forget to 
restart your web server). Link stress template to host. Use only hosts/Zabbix 
agents with configured Zabbix stress module. Wait 10 minutes before linking 
additional stress template and keep your eyes on Zabbix server performance metrics.

**4. Evaluate graphs and bottlenecks**

Tune your DB/Zabbix server/Zabbix agent configurations.

**5. Unlink and clear data stress templates**  

# Available items

| Key | Description |
| --- | ----------- |
| **stress.ping[\<anything\>]** | Return value is always int 1 |  
| **stress.echo[\<message\>]**  | Return value is string message from parameter |
| **stress.random[from,to]**  | Return value in defined range, e.g. *stress.random[1,1000]* |

# Dockerized version

Visit project https://github.com/monitoringartist/zabbix-agent-xxl 

# Recommended documentation

- https://www.zabbix.com/documentation/2.4/manual/appendix/performance_tuning
- https://www.percona.com/blog/2014/11/14/optimizing-mysql-zabbix/
- http://www.zabbix.com/files/Presentations/Tune_your_Zabbix_for_Better_Performance_-_eng.ppt
- https://ma.ttias.be/debugging-performance-problems-zabbix-internal-items/

# Author

[Devops Monitoring zExpert](http://www.jangaraj.com), who loves monitoring 
systems, which start with letter Z. Those are Zabbix and Zenoss.

Professional monitoring services:

[![Monitoring Artist](http://monitoringartist.com/img/github-monitoring-artist-logo.jpg)]
(http://www.monitoringartist.com)
