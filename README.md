Switche řady HP1911, 1920, A5120
================================

**Obsah:**
  - [Přihlášení do příkazového řádku](#přihlášení-do-příkazového-řádku)
  - [Nastavení IP adresy](#nastavení-ip-adresy)
  - [Agregace linek](#agregace-linek)
  - [Šablona konfigurace](#Šablona-konfigurace)


## Přihlášení do příkazového řádku
**Switch HP1910:**
  ```txt
  _cmdline-mode on
  Y
  512900
  system-view
  ```
**Switch HP1920:**
  ```txt
  _cmdline-mode on    
  Y
  Jinhua1920unauthorized
  ```
Nastavení IP Adresy
-------------------
`ipsetup ip-address 10.28.14.8 255.255.224.0`

Agregace linek
--------------
**Vytvoření rozhranní Bridge-Aggregation 1**
  ```txt
  <B16/2p/A>system-view
  [B16/2p/A]interface Bridge-Aggregation 1
  [B16/2p/A-Bridge-Aggregation1]quit
  ```
**Přiřazení portů 23, 24 do agregační skupiny 1**
  ```txt
  [B16/2p/A]interface GigabitEthernet 1/0/23
  [B16/2p/A-GigabitEthernet1/0/23]port link-aggregation group 1
  [B16/2p/A-GigabitEthernet1/0/23]interface GigabitEthernet 1/0/24
  [B16/2p/A-GigabitEthernet1/0/24]port link-aggregation group 1
  [B16/2p/A-GigabitEthernet1/0/24]quit
  ```
**Zobrazení přehledu o agregačních skupinách**
  ```txt
  <B16-4P>display link-aggregation summary

  Aggregation Interface Type:
  BAGG -- Bridge-Aggregation, RAGG -- Route-Aggregation
  Aggregation Mode: S -- Static, D -- Dynamic
  Loadsharing Type: Shar -- Loadsharing, NonS -- Non-Loadsharing
  Actor System ID: 0x8000, d07e-289d-c027

  AGG         AGG       Partner ID               Select Unselect   Share
  Interface   Mode                               Ports  Ports      Type
  -------------------------------------------------------------------------------
  BAGG1       D         0x8000, d07e-281c-2a0a   2      0          Shar
  BAGG2       D         0x8000, 4431-92b1-ed6a   2      0          Shar
  ```

Šablona konfigurace
-------------------
  ```txt
  # Nastaveni uzivatelskeho pristupu

  sysname B16/ASEA/2p
  local-user admin
  password cipher iopknb
  service-type lan-access
  service-type ssh telnet terminal
  service-type ftp
  service-type web
  quit

  super password level 3 cipher heslo

  # Zapnuti SSH protokolu
  ssh server enable

  # Nastaveni RSTP protokolu
  stp mode rstp
  stp bpdu-protection
  stp priority 36864
  stp pathcost-standard dot1t
  Y
  stp enable

  loopback-detection enable
  loopback-detection multi-port-mode enable

  dhcp-snooping

  # Definovani vychozi cesty routovani (neni nutne)
  ip route-static 0.0.0.0 0.0.0.0 Vlan-interface1 10.28.1.4

  # Nastaveni NTP serveru - synchronizace casu
   ntp-service source-interface Vlan-interface1
   ntp-service unicast-server 10.28.10.121
   ntp-service unicast-server 195.113.144.201

  # Nastaveni SNMP
   snmp-agent community read public 
   snmp-agent sys-info contact IS
   snmp-agent sys-info location B16/ASEA/2p
   snmp-agent sys-info version v1 v2c
   snmp-agent target-host trap address udp-domain 10.28.10.75 params securityname public


  # Nastaveni jednotlivych portu
  interface GigabitEthernet1/0/1
   stp edged-port enable
   loopback-detection enable
  #

  interface GigabitEthernet1/0/2
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/3
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/4
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/5
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/6
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/7
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/8
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/9
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/10
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/11
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/12
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/13
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/14
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/15
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/16
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/17
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/18
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/19
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/20
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/21
   loopback-detection enable
   stp edged-port enable
  #
  interface GigabitEthernet1/0/22
   loopback-detection enable
   stp edged-port enable
  #

  interface GigabitEthernet1/0/23
   loopback-detection enable
   stp edged-port enable
  #

  interface GigabitEthernet1/0/24
   loopback-detection enable
   stp edged-port enable
  #

  interface GigabitEthernet1/0/25
   undo stp edged-port 
   stp loop-protection
  #
  interface GigabitEthernet1/0/26
   undo stp edged-port 
   stp loop-protection
  #
  interface GigabitEthernet1/0/27
   undo stp edged-port 
   stp loop-protection
   dhcp-snooping trust no-user-binding
  #

  interface GigabitEthernet1/0/28
   undo stp edged-port 
   stp loop-protection
   dhcp-snooping trust
   
  quit
  ```
