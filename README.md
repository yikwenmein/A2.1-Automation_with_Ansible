## Introduction
This project implements load balancer in cloud. There is a load balancer(Haproxy) and 3 websevers(devA, devB and devC) both in cloud. The loadbalancer has an HAproxy round-roubin loadbalancer configuration while the 3 websevers has a flask app. The loadbalancer forms an overlay for websevers. It recives traffic targeted for the webservers and distribute thr traffic in a round-roubin fashion to the webservers.

## Implementation
Since these are in cloud, ansible has been used to automate the setup. All servers, networking and associates configuration has been done in cloud( cleura cloud).The ansible playbook(site.yaml)developed and run locally, install all necessay packages in the cloud servers and copy the local haproxy confuration file(haproxy.cfg.j2)to the remote server(HAproxy) and the flask app(app.py)to the remote webservers(devA, devB and devC). The ansible playbook reads the inventory file(hosts) for the hosts it will update. ssh communication has been setup in the config file.

## Execution
Curl the ip of the load balancer.