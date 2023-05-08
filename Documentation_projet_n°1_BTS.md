# Documentation projet n°1 BTS Romain CHANELIERE 2023

## I) Configuration et commandes du Pare-feu (firewall) FortiGate 61F

### Exécuter un ping
    execute ping <ip to ping>
    ou
    exe ping <ip to ping>

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

## II) Configuration et commandes du Routeur MikroTik RB3011UiAS
