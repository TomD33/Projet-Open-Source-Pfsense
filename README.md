**Nom du Groupe :** 

- Pfsense


**Le Projet comporte 4 personnes Master 1 Informatique**

- Thomas Maurin
- Thomas Duval
- Clément Cloux
- Arsene Bascop

Outils  pour notre projet :
-  Terraform avec google plateforme pour lancement des machines 
- 2 VMs avec 2 isos Pfsense configurer avec Terraform
-- Ansible déploiment des confs des machines
- Monitoring pour un visionnage  des machines



- **Objectif pour la partie Réseau** :


- **Principe de fonctionnement**  
 
pfSense communique sur les réseaux LAN & WAN avec ses adresses IP virtuelles ; il n'utilise jamais l'adresse IP assignée à son interface.  
En cas de défaillance de pfSenseA (pfSense primaire), pfSenseB (pfSense secondaire) prend le relais **sans aucune interruption de service**. La bascule de pfSenseA vers pfSenseB est totalement transparente. 
Monotoring pour obtenir une gestion de terraform  log et ansible puis les pfsenses.
 
Afin d'assurer la réplication du serveur pfSenseA vers le serveur pfSenseB, 3 éléments doivent être configurés : CARP, pfsync et XML-RPC.  
 
 
- **CARP**  
 
CARP (_Common Address Redundancy Protocol_) est un protocole permettant à plusieurs hôtes présents sur un même réseau de partager une adresse IP.  
 
Ici, nous utilisons CARP afin de partager une adresse IP WAN et une adresse IP LAN sur nos serveurs pfSense.  
C'est cette adresse IP _virtuelle_ que pfSense va utiliser pour sa communication sur le réseau. Ainsi, en cas de défaillance du pfSense primaire (pfSenseA), le pfSense secondaire (pfSenseB) prendra le relais de manière **transparente au niveau réseau** (reprise de l'adresse IP virtuelle).  
 
 
- **pfsync**  
 
pfsync est un protocole permettant de synchroniser entre deux serveurs pfSense l'état des connexions en cours (et de manière plus large entre deux serveurs exécutant le firewall Packet Filter). Ainsi, en cas de défaillance du serveur primaire, l'état des connexions en cours est maintenu sur le serveur secondaire. Il n'y a donc pas de coupure liée à la bascule des services du pfSenseA vers le pfSenseB.  
 
Il est recommandé d'effectuer cette synchronisation sur un lien dédié entre les deux serveurs pfSense. À défaut, le lien LAN peut être utilisé.  
La réplication peut se faire d'un serveur primaire vers un ou plusieurs autres serveur(s).  
 
 
- **XML-RPC**  
 
XML-RPC est un protocole permettant la réplication de données d'un serveur vers un autre. Il est utilisé dans pfSense afin de répliquer la configuration du serveur primaire vers le serveur secondaire.  
Pour garantir son bon fonctionnement, il est important qu'il utilise la même interface que celle utilisée par le protocole pfsync.



- **SCHEMA**  
![](https://i.imgur.com/doabRM1.png)
