# Description

SDN Test Suite

# Intro

Vagrant-based SDN Test Suite.

 * OpenDaylight /w Mininet
 * RouteFlow
 * VXLAN /w OVS

`vagrant status
Current machine states:

opendaylight-mininet      not created (virtualbox)
routeflow                 not created (virtualbox)
devstack-control          not created (virtualbox)
devstack-compute-1        not created (virtualbox)
devstack-compute-2        not created (virtualbox)
devstack-compute-3        not created (virtualbox)
vxlan_router              not created (virtualbox)
vxlan_server1             not created (virtualbox)
vxlan_server2             not created (virtualbox)`

# OpenDaylight /w Mininet

SDN Controller, OpenDaylight TESTing with Mininet

### Start Vagrant

`host> vagrant up opendaylight-mininet`

### Components of VM

 * OpenDaylight(Helium)
 * Mininet 2.1.x
 * Wireshark /w OF Plugin

### Run OpenDaylight (Helium Pre-Built Binary)

 * Run ODL

      `host> vagrant ssh opendaylight-mininet`

      `vm> cd /home/vagrant/opendaylight-mininet`

      `vm> ./run-mininet.sh`

      `vm> karaf> feature:install odl-dlux-core odl-restconf odl-nsf-all odl-adsal-northbound odl-mdsal-apidocs odl-l2switch-switch`

 * Web-UI

      Browser: `http://{Vagrant Host IP}:8181/dlux/index.html`

### Run Mininet

 * Common Topology

      `host> vagrant ssh opendaylight-mininet`

      `vm> sudo mn --controller remote,ip=127.0.0.1,port=6633 --switch ovsk --topo tree,3`

![Mininet Tree_Common](https://gitlab.com/call518/sdn-test/raw/master/README.md.files/tree.png)

 * Custom Topologys

      `vm> cd /home/vagrant/topo-mininet`

# RouteFlow

RouteFlow, is an open source project to provide virtualized IP routing services over OpenFlow enabled hardware.

Home: https://sites.google.com/site/routeflow/home
Video: https://www.youtube.com/watch?v=YduxuBTyjEw

(Note) OpenFlow1.0 Based

### Start Vagrant

`host> vagrant up routeflow`

### Architecture of Demo

![RouteFlow Architecture of Tutorial-2](https://gitlab.com/call518/sdn-test/raw/master/README.md.files/setup-4sw.png)

### Components of VM

 * RouteFlow
 * OpenDaylight(Built Hydrogen) & RFProxy(for ODL)
 * Mininet
 * NOX(Default: Disabled)
   * `/home/vagrant/RouteFlow-Test/RouteFlow/rftest/rftest2`
 * LXC Container (for Simulation Quagga's OSPF, BGP, RIP)

### Run OpenDaylight (Hydrogen)

 * Run ODL

      `host> vagrant ssh routeflow`

      `cd /home/vagrant/opendaylight`

      `./run.sh`

 * ODL Web-UI

      Browser: `http://{Vagratn Host IP}:8080`

### Run RouteFlow Tutorial-2

  * Run RouteFlow

      `host> vagrant ssh routeflow`

      `cd /home/vagrant/RouteFlow-Test/RouteFlow/rftest/`

      `sudo ./rftest2`

  * RouteFlow Web-UI

      `cd /home/vagrant/RouteFlow-Test/RouteFlow/rfweb`

      `gunicorn -w 4 -b 0.0.0.0:8111 rfweb:application`

      Browser: `http://Vagrant Host IP}:8111/index.html`

### APPENDIX

  * Tutorial-1 (rftest1)
    * https://github.com/CPqD/RouteFlow/wiki/Tutorial-1:-rftest1
  * Tutorial-2 (rftest2)
    * https://github.com/CPqD/RouteFlow/wiki/Tutorial-2:-rftest2

![ScreenShot RF-Web](https://gitlab.com/call518/sdn-test/raw/master/README.md.files/rf_web.png)

### Run Mininet

  * Run Mininet (Virtual Infra)

      `host> vagrant ssh routeflow`

      `cd /home/vagrant/rf-topo-mininet/`

      `sudo ./run-routeflow-infra.sh`

# DevStack /w OpenDaylight

 * DevStack (Icehouse)
   * Controller/Network Node: 1 Host
   * Compute Node: 1 Host (Max: 3)
 * OpenDaylight (Helium)

### Start Vagrant

1. `host> vagrant up devstack-control`
2. `host> vagrant up devstack-compute-1`

### Run Control/Network Node

### Run Compute-1 Node

# VXLAN /w OVS

 * Configuration of VXLAN tunnel ports in OVS
 * Configuration of OpenFlow entries OVS
 * Logical separation of traffic between tenants

### Start Vagrant

(Note) *Order is important!*

1. `host> vagrant up vxlan-router`

2. `host> vagrant up vxlan-server1`

3. `host> vagrant up vxlan-server2`

### Underlay

![VXLAN Underlay](https://gitlab.com/call518/sdn-test/raw/master/README.md.files/sdn-test-vxlan-underlay.png)

### Overlay

![VXLAN Overlay](https://gitlab.com/call518/sdn-test/raw/master/README.md.files/sdn-test-vxlan-overlay.png)
