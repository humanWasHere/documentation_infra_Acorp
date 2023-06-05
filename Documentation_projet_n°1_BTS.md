# Documentation projet n°1 BTS Romain CHANELIERE 2023

## I) Configuration et commandes du Pare-feu (firewall) FortiGate 61F
Don't forget to have a licence in FortiCloud

ressources :
https://www.youtube.com/watch?v=nqJ7-4tB7jM   
https://www.youtube.com/watch?v=QIQ4HHFtAMw

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
    set ip 192.168.40.5 255.255.255.248
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

### configuration du dns
<!--pour l'accès à internet
    config system dns
    set primary 208.91.112.53
    set secondary 208.91.112.52
    end-->

### NAT table FortiGate
    get system session list

### conf NAT GUI
penser aux adresses configurées en subnet (pas en range)


<!--### bypass licence vidéo youtube https://www.youtube.com/watch?v=1CS5tD7ljdk
    config system ntp
    set ntpsync disable
    set type custom
    end
    exe reboot-->

### rename FortiGate
    config system global
    set hostname GSN3-FortiGate
(renommé ici "GNS3-FortiGate)   
    end

### commandes générales
    exe reboot
(reboot le forti)   
    next
(passe à la suite de la conf)   
    end
(quitter une instance de config)

## II) Configuration et commandes du Routeur MikroTik RB3011UiAS

### reset MikroTik config
    system reset-configuration no-defaults=yes skip-backup=yes
### ajout adresse IP au Mikro
    ip address add address=192.168.40.3/24 interface=ether1
(on récup la connection sur ether1)
<!--    ip address add address=10.22.0.1/23 interface=ether2
(on config le début du LAN sur ether2)-->

### Configuration du routing NAT au Mikro
    ip add address=192.168.40.5/29 interface=ether1
(on passe par ether1 pour reach le network 192.168.40.0)   
    ip route add gateway=192.168.41.5
(ajout de gateway)
<!--    ip dns set servers=8.8.8.8
(ajout d'un DNS)-->

### retirer une address IP au Mikro
    ip address remove [find address 192.168.41.3/30 interface=ether1]
(enlève une IP configuré par port / dans le routing)

### fermer un port du Mikro
    interface set ether3 disable=yes

### renommer port Mikro
    interface ethernet set ether1 name=LAN

### Entrer et quitter le safe mode
    Ctrl + X
(ouvre ou ferme le safe mode)

### commandes générales
    ip address print
(on vérifie la création)   
    ping 192.168.50.1
(on vérifie également que l'interface ping)   
    /interface
(rentre )

## II) Configuration et commandes des VPCs sur GNS3
ressources :
https://www.sysnettechsolutions.com/en/configure-vpcs-gns3/   
https://docs.gns3.com/docs/emulators/vpcs/

### Configuration des IPs
    ip 192.168.42.10/24 192.168.42.5

### Configuration IP par DHCP
    ip dhcp

### commandes générales
    show ip
(affiche les configs IP)   
    clear ip
(removes IP configs)   
    save
(to save config)   
    ping
(classique)   
    trace
(tracert)