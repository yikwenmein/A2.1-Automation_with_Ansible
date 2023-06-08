## Introduction
This project implements load balancers in cloud. There is a load balancer(Haproxy) and 3 websevers(devA, devB and devC) both in cloud. The loadbalancer has an HAproxy round-roubin loadbalancer configuration while the 3 websevers has a flask app. The loadbalancer forms an overlay for websevers. It recives traffic targeted for the webservers and distribute thr traffic in a round-roubin fashion to the webservers.There is also an nginx loadbalancer that loadbalances udp traffic.

## Implementation
Since these are in cloud, ansible has been used to automate the setup. All servers, networking and associates configuration has been done in cloud( cleura cloud).The ansible playbook(site.yaml)developed and run locally(`ansible-playbook -i hosts site.yaml`)that will install all necessay packages and copy neccesary configurations locally into respective servers and loadbalancer.The files include;
1. app.py: has the flask app.It replies with just time and hostname of the serving server
2. conf: For ssh configuration
3. config.cfg: Ansible configuration(tells the ansible playbook where to look for the inventory file)
4. default: Both HAproxy and nginx servers all seem to run on port 80 by default.The file is needed to force nginx to run on 8080 since HAproxy was already on 80
5. haproxy.cfg.j2: Has the HAproxy configuration(tcp loadbalancer)
6. hosts: Ansible host file or inventory file
7. nginx: Has the nginx configuration(udp loadbalancer)
8. site.yaml: The ansible playbook the will setup our loadbalacer and servers
9. snmpd.conf: SNMP daemon server

HAproxy takes care of the flask app while Nginx takes care of SNMP

The logical network/service map would be as follows: 

```(Internet)--->HAproxy(HTTP/80:HTTP/80)+Nginx(UDP/1611:UDP/161) 
             (internal network using private address range; 10.0.1.0/27) 
              +--->devA 
              +--->devB 
              +--->devC 
(Internet)--->Bastion
``` 


## Execution
Curl the ip of the load balancer.
`curl http://91.106.198.36:80`
`snmpwalk -v 2c -c public HAproxy`

## Results
![HAproxy loadbalancer](/images/one.png)
![Nginx loabalancer](/images/two.png)