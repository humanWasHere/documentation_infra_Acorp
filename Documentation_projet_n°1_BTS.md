# Documentation projet n°1 BTS Romain CHANELIERE 2023

## I) Configuration et commandes du Pare-feu (firewall) FortiGate 61F
Ne pas oublier la licence sur FortiCloud.

Ressources :
https://www.youtube.com/watch?v=nqJ7-4tB7jM   
https://www.youtube.com/watch?v=QIQ4HHFtAMw

### Table ARP
    get system arp

### Voir les IPs des ports du firewall
    get system interface

### Définition d'une IP sur le port X (ici au port 1)
    config system interface
    edit port1
    set mode static
    set allowaccess ping http https (ssh telnet)
    set ip 192.168.40.5 255.255.255.248
    end
(vérifier le ping)
### Configuration du routage pour accéder à internet sur le Forti
    config router static
    edit (savoir si il y en a un de configuré)
    edit 1
    (set dst 0.0.0.0/0) <!--attribution de l'IP de destination-->
    set device port1
    set gateway 192.168.40.2 (l'interface cloud cible)
    end
(vérifier l'accès à internet par ping)

<!--### Configuration du dns
pour l'accès à internet
    config system dns
    set primary 208.91.112.53
    set secondary 208.91.112.52
    end-->

### Configuration du NAT
Configuration du NAT par GUI (penser aux adresses configurées en subnet).

NAT table FortiGate :
    get system session list


<!--### bypass licence vidéo youtube https://www.youtube.com/watch?v=1CS5tD7ljdk
    config system ntp
    set ntpsync disable
    set type custom
    end
    exe reboot-->

### Renommer FortiGate
    config system global
    set hostname GSN3-FortiGate
    end
(Ici renommé : "GNS3-FortiGate")   

### Commandes générales
Reboot le FortiGate :

    exe reboot

Passer à la suite de la configuration (correspond à un ../) :

    next

Quitter une instance de configuration (correspond à un /) :

    end

Exécuter un ping

    exe ping <IP>


Exécuter un traceroute

    exe traceroute <IP>

## II) Configuration et commandes du Routeur MikroTik RB3011UiAS

### Reset la configuration du MikroTik
    system reset-configuration no-defaults=yes skip-backup=yes
### Ajout d'une adresse IP
On récup la connection sur ether1 :

    ip address add address=192.168.40.3/24 interface=ether1
<!--    ip address add address=10.22.0.1/23 interface=ether2
(on config le début du LAN sur ether2)-->

### Retirer une address IP
Enlever une IP configurée par port / dans le routing :

    ip address remove [find address 192.168.41.3/30 interface=ether1]

ou 

    ip address remove 2

### Configuration du routing NAT
On passe par ether1 pour accéder au réseau 192.168.40.0 :

    ip add address=192.168.40.5/29 interface=ether1

Ajout d'une passerelle :

    ip route add gateway=192.168.41.5

Ajout d'un DNS :

    ip dns set servers=8.8.8.8

### Fermer un port du MikroTik
    interface set ether3 disable=yes

### Renommer un port du MikroTik
    interface ethernet set ether1 name=LAN

### Entrer et quitter le safe mode
Ouverture ou fermeture du safe mode :

    Ctrl + X

### Commandes générales
Afficher les addresses IP configurées :

    ip address print

Afficher les routes :

    ip route print

Afficher les ports :

    interface print

On vérifie également que l'interface ping :

    ping 192.168.50.1

Rentre dans la configuration d'interface sur plusieurs ligne

    /interface

Revient à la configuration générique

    /

<!--
## Configuration DHCP relay MikroTik

### Configuration DHCP
Configure pool : 

    ip pool add name=DHCPgreLANpool ranges=10.22.0.10-10.22.1.254

Create DHCP server :

    ip dhcp-server add interface=ether3 relay=10.22.0.1 \ dns-server=192.168.50.1

Configure DHCP relay :

    ip dhcp-relay add name=DHCPrelay interface=ether2 \ dhcp-server=192.168.50.1 local-address=10.22.0.1 disabled=no

## III) Configuration DNS (Cisco)

### Configuration de l'interface fa0/0
    conf t
    interface fastEthernet 0/0
    ip address 192.168.50.1 255.255.255.0
    no shutdown
    end

### Afficher les interfaces configurées
    show ip int br

### Configuration DHCP et DNS
    conf t
    ip dhcp pool DHCPgreLANpool
    network 10.22.0.1 255.255.254.0
    default-router 192.168.50.1
    dns-server 192.168.50.1
    exit

    ip dhcp excluded-address 10.22.0.1 10.22.0.9
    end


### Voir binding et pool (interfaces machines avec DHCP)
    show ip dhcp binding
    show ip dhcp pool

### Commandes générales
Passer à la suite de la configuration (correspond à un ../) :

    exit

Quitter une instance de configuration (correspond à un /) :

    end
-->

<!--
## III) Configuration DHCP MikroTik

### Confiugration du DHCP
    ip dhcp-server
    ip pool add ranges=10.22.0.10-10.22.0.60 name=range1
    ip pool print

    ip dhcp-server/add address-pool=range1 lease-time=500 interface=ether3
    ip dhcp-server/network/add address=10.22.0.0/23 gateway=10.22.0.1 dns-server=8.8.8.8
-->

## IV) Configuration et commandes des VPCs sur GNS3
Ressources :
https://www.sysnettechsolutions.com/en/configure-vpcs-gns3/   
https://docs.gns3.com/docs/emulators/vpcs/

### Configuration des IPs
On défini l'IP du PC puis sa passerelle :

    ip 192.168.42.10/24 192.168.42.5

<!--### Configuration IP par DHCP
    ip dhcp-->

### Configuration DNS
    ip dns

### Commandes générales
Affiche les configurations IP :

    show ip

Retire les IPs configurées :

    clear ip

Sauvegarde la configuration :

    save

Éxécuter un ping :

    ping

Éxécuter un trace route :

    trace