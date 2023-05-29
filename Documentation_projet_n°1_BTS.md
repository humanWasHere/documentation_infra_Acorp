# Documentation projet n°1 BTS Romain CHANELIERE 2023

## I) Configuration et commandes du Pare-feu (firewall) FortiGate 61F

### Exécuter un ping
    execute ping <IP to ping>
ou
    exe ping <IP to ping>

### table ARP
    get system arp

### voir les IPs des ports du firewall
    get system interface

### définir une IP sur le port X (ici au port 1)
    config system interface
    edit port1
    set mode static
    set allowaccess ping http https (ssh telnet)
    set ip <ip/netmask> (192.168.40.5) <!--(10.40.0.5/23)-->
    end
(vérifier le ping)
### configuration du routage pour accéder à internet sur le Forti
    config router static
    edit (savoir si il y en a un de configuré)
    edit 1
    (set dst 0.0.0.0/0) <!--attribution de l'IP de destination-->
    set device port1
    set gateway 192.168.40.2 (l'interface cloud cible)
    end
(vérifier l'accès à internet par ping)

### configuration du dns <!--pour l'accès à internet-->
    config system dns
    set primary 208.91.112.53
    set secondary 208.91.112.52
    end-->

<!--### bypass licence vidéo youtube https://www.youtube.com/watch?v=1CS5tD7ljdk
    config system ntp
    set ntpsync disable
    set type custom
    end
    exe reboot-->

### commandes générales
    end
(quitter une instance de config)

## II) Configuration et commandes du Routeur MikroTik RB3011UiAS

### reset MikroTik config
    system reset-configuration no-defaults=yes skip-backup=yes
### ajout adresse IP au Mikro
    ip address add address=192.168.40.3/24 interface=ether1
(on récup la connection sur ether1)
    ip address add address=192.168.50.1/23 interface=ether2
(on config le début du LAN sur ether2)
    ip address print
(on vérifie la création)
    ping 192.168.50.1
(on vérifie également que l'interface ping)

### commandes générales
    ip address remove [find address 192.168.41.3/30 interface=ether1]
(enlève une IP configuré par port / dans le routing)
    Ctrl + X
(ouvre ou ferme le safe mode)