# Protocoles de Synchronisation Temporelle : NTP et PTP

<img width="1248" height="832" alt="OIG1 (1)" src="https://github.com/user-attachments/assets/896cd6c9-3d67-4b09-b03b-3d8dc56597ba" />


**Auteur :** hackers_tchad  
**Date de publication :** 2026  
**Version :** 1.0 - Édition Avancée  
**Licence :** Documentation éducative et technique libre

---

## Table des matières

1. [Introduction générale](#introduction-générale)
2. [Historique et créateurs](#historique-et-créateurs)
3. [Concepts fondamentaux de synchronisation temporelle](#concepts-fondamentaux-de-synchronisation-temporelle)
4. [Architecture globale de la synchronisation](#architecture-globale-de-la-synchronisation)
5. [NTP - Network Time Protocol](#ntp---network-time-protocol)
   - 5.1. Définition et rôle
   - 5.2. Historique
   - 5.3. Architecture NTP
   - 5.4. Fonctionnement détaillé
   - 5.5. Strates et hiérarchie NTP
   - 5.6. Algorithmes et ajustement d'horloge
   - 5.7. Versions NTP
   - 5.8. Formats de paquets NTP
   - 5.9. Modes de fonctionnement NTP
   - 5.10. Commandes et utilitaires NTP
   - 5.11. Ports et sécurité
   - 5.12. Configuration NTP
   - 5.13. Vérification et tests sous Linux
   - 5.14. Vérification et tests sous Windows
   - 5.15. Configuration avancée et optimisations
   - 5.16. Dépannage NTP
6. [PTP - Precision Time Protocol](#ptp---precision-time-protocol)
   - 6.1. Définition et rôle
   - 6.2. Historique
   - 6.3. Architecture PTP
   - 6.4. Fonctionnement détaillé
   - 6.5. Domaines, horloges et messages PTP
   - 6.6. Types d'horloges PTP
   - 6.7. Délai et asymétrie
   - 6.8. Profils PTP
   - 6.9. Ports et sécurité
   - 6.10. Configuration PTP
   - 6.11. Vérification et tests sous Linux
   - 6.12. Vérification et tests sous Windows
   - 6.13. Configuration avancée et optimisations
   - 6.14. Dépannage PTP
7. [Comparatif NTP / PTP](#comparatif-ntp--ptp)
8. [Où et comment sont utilisés NTP et PTP](#où-et-comment-sont-utilisés-ntp-et-ptp)
9. [Applications et logiciels utilisant NTP et PTP](#applications-et-logiciels-utilisant-ntp-et-ptp)
10. [Configuration sur les principaux serveurs et équipements](#configuration-sur-les-principaux-serveurs-et-équipements)
11. [Sécurité de la synchronisation temporelle](#sécurité-de-la-synchronisation-temporelle)
12. [Commandes de vérification réseau](#commandes-de-vérification-réseau)
13. [Exercices pratiques](#exercices-pratiques)
14. [Ressources, livres et liens](#ressources-livres-et-liens)
15. [Glossaire](#glossaire)
16. [Conclusion](#conclusion)

---

## Introduction générale

La synchronisation temporelle est un élément fondamental des systèmes informatiques modernes. Elle garantit que les horloges des ordinateurs, des serveurs, des équipements réseau et des systèmes industriels restent coordonnées avec une référence de temps fiable. Sans une synchronisation précise, de nombreuses applications critiques deviendraient incohérentes ou inutilisables : transactions financières, journaux d'événements, authentification par certificats, communications réseau, télédistribution, systèmes de transport, télécommunications, finance à haute fréquence, production industrielle, et bien d'autres.

Les deux protocoles les plus importants pour la synchronisation temporelle sur réseau sont le **NTP (Network Time Protocol)** et le **PTP (Precision Time Protocol)**. NTP est conçu pour fournir une précision de l'ordre de la milliseconde, voire de la dizaine de microsecondes dans de bonnes conditions, sur des réseaux IP classiques. PTP, quant à lui, vise une précision bien plus élevée, de l'ordre de la microseconde, voire de la nanoseconde, et est utilisé dans des environnements nécessitant une synchronisation extrêmement fine, comme les réseaux télécoms, la finance, l'industrie 4.0, l'aérospatial, l'audiovisuel et le transport.

Ce guide explore en profondeur ces deux protocoles, leur histoire, leur architecture, leur configuration, les commandes de vérification sous Linux et Windows, ainsi que les bonnes pratiques de sécurité. Il s'adresse aux administrateurs système, aux ingénieurs réseau, aux professionnels de la cybersécurité, aux automaticiens, aux étudiants en informatique et à toute personne souhaitant maîtriser la synchronisation temporelle sur réseau.

---

## Historique et créateurs

### NTP

Le protocole NTP a été conçu par **David L. Mills**, professeur à l'Université du Delaware. Il a commencé à développer NTP au début des années 1980, alors qu'il travaillait sur les protocoles Internet et la synchronisation des horloges dans le cadre du projet ARPANET. La première spécification de NTP a été publiée dans la RFC 958 en 1985. Mills a poursuivi le développement et la normalisation de NTP pendant plusieurs décennies, contribuant à de nombreuses versions et à la création du réseau de serveurs de référence NTP.

Les dates clés de NTP sont :

- 1981 : Premières expérimentations sur la synchronisation d'horloges dans ARPANET.
- 1983 : Publication de la RFC 868 (Time Protocol), un protocole simple de synchronisation.
- 1985 : RFC 958, première spécification complète de NTP.
- 1988 : RFC 1059, NTP version 1.
- 1989 : RFC 1119, NTP version 2.
- 1992 : RFC 1305, NTP version 3, avec une architecture en strates et des algorithmes sophistiqués.
- 2010 : RFC 5905, NTP version 4, version actuelle la plus répandue.
- 2015-2024 : Travaux sur NTP version 5 et sur des extensions de sécurité comme NTS (Network Time Security).

David Mills est décédé en 2024, mais son héritage demeure dans l'ensemble de l'infrastructure temporelle d'Internet.

### PTP

Le protocole PTP a été développé au sein de l'**IEEE (Institute of Electrical and Electronics Engineers)**. Il a été standardisé sous la référence **IEEE 1588** en 2002. La révision **IEEE 1588-2008**, souvent appelée PTPv2, a apporté de nombreuses améliorations, notamment la prise en charge des réseaux multicast, des horloges transparentes et boundary, et une meilleure précision. La dernière révision majeure, **IEEE 1588-2019**, a enrichi le standard avec de nouveaux profils, des mécanismes de sécurité et une meilleure adaptabilité aux réseaux modernes.

Les principales dates de PTP sont :

- 2000-2002 : Développement du standard IEEE 1588 par un groupe de travail de l'IEEE, avec la contribution de nombreux industriels, dont ceux issus du secteur de l'automatisation et des télécommunications.
- 2002 : Publication de la norme IEEE 1588-2002 (PTPv1).
- 2008 : Publication de IEEE 1588-2008 (PTPv2), version la plus déployée.
- 2019 : Publication de IEEE 1588-2019, apportant des améliorations significatives.
- 2020-2026 : Adoption massive de PTP dans les réseaux 5G, l'industrie 4.0, la finance et les data centers.

Les contributeurs clés de PTP incluent des ingénieurs de **Agilent Technologies, National Instruments, Rockwell Automation, Siemens, Cisco, Intel** et de nombreux autres acteurs industriels.

---

## Concepts fondamentaux de synchronisation temporelle

### Temps universel coordonné (UTC)

L'**UTC (Coordinated Universal Time)** est la référence mondiale de temps civil. Il est basé sur le temps atomique international (TAI) auquel on ajoute ou retire des secondes intercalaires pour rester synchronisé avec la rotation de la Terre. Les horloges informatiques sont généralement synchronisées sur UTC, puis converties en fuseau horaire local pour l'affichage.

### Temps atomique international (TAI)

Le **TAI** est une échelle de temps basée sur des horloges atomiques. Il ne tient pas compte des secondes intercalaires. La différence entre UTC et TAI dépend du nombre de secondes intercalaires ajoutées depuis 1972.

### GPS et GNSS

Le **GPS (Global Positioning System)** et les autres systèmes de navigation par satellites (GNSS) fournissent une référence temporelle extrêmement précise. Chaque satellite embarque des horloges atomiques et diffuse des signaux temporels. Les récepteurs GPS peuvent ainsi fournir une référence UTC avec une précision de quelques dizaines de nanosecondes.

### Horloge matérielle (RTC)

L'**RTC (Real-Time Clock)** est l'horloge matérielle présente dans les ordinateurs. Elle fonctionne sur batterie et continue de tourner même lorsque la machine est éteinte. Au démarrage, le système d'exploitation lit l'heure de l'RTC, puis la synchronise avec NTP ou PTP.

### Dérive et dérive d'horloge

Les horloges des ordinateurs dérivent naturellement en raison de la tolérance des oscillateurs à quartz, des variations de température, de la tension d'alimentation et d'autres facteurs environnementaux. La dérive peut atteindre plusieurs secondes par jour si aucune synchronisation n'est effectuée.

### Latence et gigue

La **latence** est le temps de transmission d'un paquet entre deux points du réseau. La **gigue (jitter)** est la variation de cette latence. Ces deux facteurs influencent directement la précision de la synchronisation temporelle sur un réseau IP.

### Asymétrie de réseau

L'**asymétrie** désigne une différence de temps de trajet aller-retour entre deux équipements. Si le chemin aller est plus rapide que le chemin retour (ou inversement), les protocoles basés sur l'estimation du délai de propagation peuvent être biaisés. PTP est particulièrement sensible à l'asymétrie.

### Timestamp

Un **timestamp** est une valeur représentant un instant précis, généralement exprimée en secondes écoulées depuis une date de référence (epoch). Unix utilise le 1er janvier 1970 00:00:00 UTC comme epoch.

---

## Architecture globale de la synchronisation

L'architecture de synchronisation temporelle repose sur plusieurs composants interdépendants :

1. **Source de référence temporelle** : Horloge atomique, récepteur GPS/GNSS, signal radio (DCF77, MSF, WWVB), ou source nationale (UTC(LNE), UTC(PTB), etc.).
2. **Serveur de temps primaire (stratum 1)** : Serveur directement synchronisé sur une source de référence.
3. **Serveur de temps secondaire (stratum 2 et plus)** : Serveur synchronisé sur un serveur de stratum inférieur.
4. **Clients NTP/PTP** : Ordinateurs, serveurs, équipements réseau, systèmes industriels, etc.
5. **Réseau de distribution** : LAN, WAN, Internet, fibres dédiées, réseaux cellulaires.
6. **Boundary clocks et transparent clocks (PTP)** : Équipements réseau participant activement à la distribution PTP.

### Flux de synchronisation NTP

1. Une source de référence (GPS, horloge atomique) alimente un serveur stratum 1.
2. Le serveur stratum 1 diffuse l'heure vers des serveurs stratum 2.
3. Les serveurs stratum 2 desservent des clients stratum 3 ou d'autres serveurs.
4. Les clients ajustent leur horloge système en fonction des serveurs NTP.

### Flux de synchronisation PTP

1. Un grandmaster clock reçoit une référence temporelle (GPS, GNSS, ou autre).
2. Le grandmaster émet des messages PTP (Sync, Follow_Up, Announce) sur le réseau.
3. Les boundary clocks relaient et corrigent la distribution temporelle.
4. Les transparent clocks mesurent et compensent le temps de transit.
5. Les slaves ajustent leur horloge locale pour suivre le grandmaster.

---

## NTP - Network Time Protocol

### 5.1. Définition et rôle

NTP est un protocole de synchronisation temporelle conçu pour coordonner les horloges des ordinateurs sur un réseau IP. Il permet d'obtenir une précision typique de quelques millisecondes sur Internet et de quelques microsecondes sur un LAN bien configuré. NTP est un protocole de couche application utilisant UDP sur les ports 123 (client et serveur).

NTP est utilisé pour :

- Synchroniser les horloges des serveurs et postes de travail.
- Assurer la cohérence des journaux d'événements (logs).
- Garantir la validité des certificats et des mécanismes d'authentification (Kerberos, TLS).
- Orchestrer des transactions distribuées.
- Faciliter le diagnostic et la corrélation d'incidents.
- Synchroniser les équipements réseau (routeurs, commutateurs, pare-feu).
- Assurer la précision des systèmes industriels et embarqués.

### 5.2. Historique

L'histoire de NTP est étroitement liée au développement d'Internet et aux travaux de David Mills. Les versions majeures sont :

- **NTPv0 (1983)** : Première expérimentation documentée.
- **NTPv1 (1985, RFC 958)** : Première spécification formelle.
- **NTPv2 (1988-1989)** : Amélioration des algorithmes, support du broadcast.
- **NTPv3 (1992, RFC 1305)** : Introduction des strates, du mode client-serveur, du broadcast et manycast.
- **NTPv4 (2010, RFC 5905)** : Support d'IPv6, amélioration de la sécurité, support des réseaux mobiles.
- **NTPv5 (en développement)** : Nouvelle génération visant à améliorer la robustesse et la sécurité.

### 5.3. Architecture NTP

L'architecture NTP est hiérarchique et distribuée. Elle repose sur la notion de **stratum**, qui indique la distance par rapport à une source de référence temporelle.

- **Stratum 0** : Sources de référence (horloges atomiques, GPS, GNSS, signaux radio).
- **Stratum 1** : Serveurs NTP directement connectés à une source stratum 0.
- **Stratum 2** : Serveurs NTP synchronisés sur des serveurs stratum 1.
- **Stratum 3 à 15** : Serveurs et clients synchronisés sur des strates supérieures.
- **Stratum 16** : Non synchronisé.

NTP peut fonctionner selon plusieurs modes :

- **Mode client/serveur** : Le client interroge un ou plusieurs serveurs.
- **Mode symétrique** : Deux serveurs s'interrogent mutuellement pour améliorer la robustesse.
- **Mode broadcast/multicast** : Le serveur diffuse l'heure vers de nombreux clients.
- **Mode manycast** : Le client envoie une requête à un groupe multicast et sélectionne les meilleurs serveurs.

### 5.4. Fonctionnement détaillé

NTP utilise des échanges de paquets UDP pour estimer le décalage entre l'horloge locale et l'horloge de référence. Le principe de base est le suivant :

1. Le client envoie une requête NTP au serveur en notant l'heure locale d'envoi `t1`.
2. Le serveur reçoit la requête en notant l'heure locale de réception `t2`.
3. Le serveur envoie une réponse en notant l'heure locale d'envoi `t3`.
4. Le client reçoit la réponse en notant l'heure locale de réception `t4`.

Les formules fondamentales sont :

```
délai_aller_retour δ = (t4 - t1) - (t3 - t2)
décalage θ = ((t2 - t1) + (t3 - t4)) / 2
```

Le client utilise ces valeurs pour estimer le décalage de son horloge par rapport au serveur et pour ajuster progressivement l'heure système.

### 5.5. Strates et hiérarchie NTP

La hiérarchie des strates permet de limiter la charge sur les serveurs de référence et d'assurer une distribution scalable de l'heure. Les serveurs stratum 1 sont les plus précis mais aussi les plus rares. Les serveurs stratum 2 sont généralement accessibles publiquement et desservent les clients finaux.

Exemple de hiérarchie :

```
Stratum 0 : Horloge atomique / GPS
    |
Stratum 1 : serveur-ntp1.example.com
    |
Stratum 2 : serveur-ntp2.example.com
    |
Stratum 3 : postes de travail et serveurs clients
```

Un client NTP peut interroger plusieurs serveurs de strates différentes pour améliorer la fiabilité et la précision.

### 5.6. Algorithmes et ajustement d'horloge

NTP utilise plusieurs algorithmes pour sélectionner les meilleures sources et ajuster l'horloge locale :

- **Clock filter algorithm** : Filtre les mesures bruitées pour estimer le décalage et le délai.
- **Intersection algorithm** : Sélectionne les serveurs cohérents entre eux.
- **Clustering algorithm** : Classe les serveurs par qualité et réduit leur nombre.
- **Combine algorithm** : Combine les mesures des serveurs sélectionnés.
- **Loop filter et PLL/FLL** : Ajustent la fréquence de l'horloge locale pour réduire le décalage sans à-coups.

L'ajustement peut se faire de deux manières :

- **Step** : Modification brutale de l'heure (utilisée pour les gros écarts).
- **Slew** : Ajustement progressif de la fréquence de l'horloge (utilisé pour les petits écarts).

### 5.7. Versions NTP

- **NTPv3** : Encore rencontré sur de vieux systèmes. Utilise le format de paquet classique.
- **NTPv4** : Version actuelle. Supporte IPv6, améliore la précision et la sécurité.
- **SNTP (Simple Network Time Protocol)** : Version simplifiée de NTP, utilisée dans les équipements embarqués et les appareils IoT. Moins précis mais plus léger.
- **NTPv5** : En cours de spécification. Vise à améliorer la sécurité, la robustesse et la précision.

### 5.8. Formats de paquets NTP

Un paquet NTPv4 fait 48 octets minimum. Les champs principaux sont :

- **LI (Leap Indicator)** : Indicateur de seconde intercalaire (2 bits).
- **VN (Version Number)** : Version NTP (3 bits).
- **Mode** : Mode de fonctionnement (3 bits).
- **Stratum** : Strate du serveur (8 bits).
- **Poll** : Intervalle d'interrogation (8 bits).
- **Precision** : Précision de l'horloge locale (8 bits).
- **Root Delay** : Délai total vers la source de référence (32 bits).
- **Root Dispersion** : Dispersion totale vers la source de référence (32 bits).
- **Reference ID** : Identifiant de la source de référence (32 bits).
- **Reference Timestamp** : Heure de dernière mise à jour de l'horloge locale (64 bits).
- **Origin Timestamp** : Heure d'envoi de la requête par le client (64 bits).
- **Receive Timestamp** : Heure de réception de la requête par le serveur (64 bits).
- **Transmit Timestamp** : Heure d'envoi de la réponse par le serveur (64 bits).
- **Extension fields** : Champs optionnels pour NTS ou extensions.
- **MAC (Message Authentication Code)** : Authentification optionnelle.

### 5.9. Modes de fonctionnement NTP

- **Mode 1 (symmetric active)** : Association symétrique active.
- **Mode 2 (symmetric passive)** : Association symétrique passive.
- **Mode 3 (client)** : Requête client.
- **Mode 4 (server)** : Réponse serveur.
- **Mode 5 (broadcast)** : Diffusion périodique par un serveur.
- **Mode 6 (control message)** : Messages de contrôle (ntpq, ntpdc).
- **Mode 7 (private)** : Messages privés réservés.

### 5.10. Commandes et utilitaires NTP

Les principaux utilitaires NTP sont :

- **ntpd** : Démon NTP classique (NTPv4).
- **chronyd** : Démon NTP moderne, plus robuste dans les environnements instables.
- **ntpdate** : Outil ponctuel de synchronisation (obsolète, remplacé par ntpdate ou sntp).
- **sntp** : Client SNTP simple.
- **ntpq** : Outil de requête et de contrôle de ntpd.
- **ntpdc** : Outil de contrôle spécialisé de ntpd.
- **chronyc** : Outil de contrôle de chronyd.
- **timedatectl** : Outil systemd pour gérer l'heure et le fuseau horaire.
- **hwclock** : Outil de gestion de l'horloge matérielle.
- **ntptrace** : Outil de traçage de la hiérarchie NTP (obsolète).

### 5.11. Ports et sécurité

NTP utilise le port UDP **123** à la fois en source et en destination. Le port TCP 123 est rarement utilisé.

Bonnes pratiques de sécurité NTP :

- Limiter l'accès au serveur NTP aux clients autorisés (ACL).
- Désactiver les commandes de contrôle publiques.
- Utiliser NTS (Network Time Security) pour authentifier et chiffrer les échanges.
- Empêcher le spoofing et les attaques par amplification.
- Surveiller les logs et les requêtes anormales.
- Mettre à jour régulièrement le logiciel serveur.
- Éviter d'exposer un serveur NTP publiquement sans restriction.

### 5.12. Configuration NTP

#### Configuration d'un client NTP avec chrony

Fichier `/etc/chrony/chrony.conf` (Debian/Ubuntu) ou `/etc/chrony.conf` (RHEL/CentOS/Rocky) :

```ini
# Serveurs de référence
pool fr.pool.ntp.org iburst
pool 0.debian.pool.ntp.org iburst
pool 1.debian.pool.ntp.org iburst

# Autoriser les clients du réseau local (pour serveur)
allow 192.168.1.0/24

# Fichier de dérive de l'horloge
driftfile /var/lib/chrony/drift

# Journal des mesures
logdir /var/log/chrony
log measurements statistics tracking

# Seuil de correction brutale
makestep 1.0 3

# Correction maximale de la fréquence
maxupdateskew 100.0

# Utiliser le temps matériel en cas de coupure RTC
rtcsync
```

Redémarrage de chrony :

```bash
sudo systemctl restart chronyd
sudo systemctl enable chronyd
```

#### Configuration d'un client NTP avec ntpd

Fichier `/etc/ntp.conf` :

```ini
# Serveurs de référence
server 0.pool.ntp.org iburst
server 1.pool.ntp.org iburst
server 2.pool.ntp.org iburst
server 3.pool.ntp.org iburst

# Restrictions d'accès
restrict default kod nomodify notrap nopeer noquery
restrict 127.0.0.1
restrict ::1

# Fichier de dérive
driftfile /var/lib/ntp/drift

# Logs
statsdir /var/log/ntpstats/
```

Redémarrage de ntpd :

```bash
sudo systemctl restart ntpd
sudo systemctl enable ntpd
```

#### Configuration d'un serveur NTP stratum 2 avec chrony

```ini
server ntp1.example.com iburst
server ntp2.example.com iburst

# Servir le réseau local
allow 10.0.0.0/8
allow 172.16.0.0/12
allow 192.168.0.0/16

local stratum 10
```

### 5.13. Vérification et tests sous Linux

#### Vérifier le statut de synchronisation avec timedatectl

```bash
timedatectl status
```

Sortie typique :

```
               Local time: lun. 2026-01-12 14:30:45 CET
           Universal time: lun. 2026-01-12 13:30:45 UTC
                 RTC time: lun. 2026-01-12 13:30:44
                Time zone: Europe/Paris (CET, +0100)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

#### Vérifier les sources avec chronyc

```bash
chronyc sources
```

Sortie typique :

```
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
^+ ntp1.example.com              2   6   377    45   +120us[ +120us] +/-   15ms
^* ntp2.example.com              1   6   377    42   +45us[  +45us] +/-    8ms
^- ntp3.example.com              3   6   377    44   -230us[ -230us] +/-   32ms
```

#### Vérifier le tracking avec chronyc

```bash
chronyc tracking
```

Sortie typique :

```
Reference ID    : A1B2C3D4 (ntp2.example.com)
Stratum         : 2
Ref time (UTC)  : Mon Jan 12 13:30:30 2026
System time     : 0.000045 seconds slow of NTP time
Last offset     : +0.000012 seconds
RMS offset      : 0.000023 seconds
Frequency       : 5.123 ppm fast
Residual freq   : +0.002 ppm
Skew            : 0.156 ppm
Root delay      : 0.012345 seconds
Root dispersion : 0.000987 seconds
Update interval : 64.0 seconds
Leap status     : Normal
```

#### Interroger un serveur NTP avec ntpdate

```bash
sudo ntpdate -q pool.ntp.org
```

#### Interroger avec sntp

```bash
sntp pool.ntp.org
```

#### Tester un serveur NTP avec ntpq

```bash
ntpq -p
```

Sortie typique :

```
     remote           refid      st t when poll reach   delay   offset  jitter
======================================================================
*ntp1.example.com .GPS.            1 u   56   64  377    8.123    0.045   0.234
+ntp2.example.com .PPS.            1 u   52   64  377   12.456   -0.123   0.567
```

#### Vérifier les ports en écoute

```bash
sudo ss -ulnp | grep ntp
sudo ss -ulnp | grep chronyd
```

#### Vérifier les logs

```bash
sudo journalctl -u chronyd -f
sudo journalctl -u ntpd -f
sudo tail -f /var/log/chrony/measurements.log
```

#### Envoyer une requête NTP brute avec nmap

```bash
sudo nmap -sU -p 123 ntp.example.com
```

#### Vérifier la réponse NTP avec dig (pas applicable, utiliser ntpq ou chronyc)

NTP n'utilise pas DNS pour son protocole, mais on peut vérifier les enregistrements NTP si le domaine en publie :

```bash
dig ntp.example.com A
dig ntp.example.com AAAA
```

### 5.14. Vérification et tests sous Windows

#### Vérifier le statut NTP avec w32tm

```cmd
w32tm /query /status
```

Sortie typique :

```
Leap Indicator: 0(no warning)
Stratum: 3 (secondary reference - syncd by (S)NTP)
Precision: -6 (15.625ms per tick)
Root Delay: 0.0156249s
Root Dispersion: 0.0168706s
ReferenceId: 0xC0A80101 (source IP:  192.168.1.1)
Last Successful Sync Time: 1/12/2026 2:30:00 PM
Source: time.windows.com,0x9
Poll Interval: 10 (1024s)
```

#### Lister les sources NTP

```cmd
w32tm /query /source
w32tm /query /peers
```

#### Configurer manuellement un serveur NTP

```cmd
w32tm /config /manualpeerlist:"0.pool.ntp.org,1.pool.ntp.org,2.pool.ntp.org" /syncfromflags:manual /reliable:yes /update
net stop w32time && net start w32time
```

#### Forcer une resynchronisation

```cmd
w32tm /resync /force
```

#### Tester la connectivité UDP 123

```powershell
Test-NetConnection -ComputerName pool.ntp.org -Port 123 -Udp
```

#### Diagnostiquer avec w32tm

```cmd
w32tm /stripchart /computer:time.windows.com /samples:5 /dataonly
```

Sortie typique :

```
Tracking time.windows.com [13.86.101.172:123].
Collecting 5 samples.
The current time is 1/12/2026 2:35:00 PM.
14:35:00, +00.0004567s
14:35:01, +00.0003987s
14:35:02, +00.0004211s
14:35:03, +00.0003890s
14:35:04, +00.0004023s
```

#### Régler le fuseau horaire

```powershell
Set-TimeZone -Id "Romance Standard Time"
```

### 5.15. Configuration avancée et optimisations

#### Utilisation de NTS (Network Time Security)

NTS est une extension de sécurité pour NTP. Elle sécurise l'échange de clés avec TLS, puis authentifie les paquets NTP avec ces clés.

Configuration chrony avec NTS :

```ini
server time.cloudflare.com iburst nts
server nts.netnod.se iburst nts

ntsdumpdir /var/lib/chrony
```

Vérification :

```bash
chronyc -N authdata
chronyc -N sources
```

#### Optimisation pour un data center

- Utiliser des serveurs NTP locaux stratum 2.
- Activer le burst mode (`iburst`).
- Configurer plusieurs sources pour la redondance.
- Limiter la bande passante et la charge réseau.
- Utiliser des serveurs avec des liens symétriques.

#### Gestion des secondes intercalaires

NTP peut annoncer une seconde intercalaire avec le champ Leap Indicator. Les systèmes modernes utilisent généralement une approche "leap smear" (étalement progressif de la seconde) pour éviter les discontinuités.

Exemple avec chrony :

```ini
leapsectz right/UTC
```

### 5.16. Dépannage NTP

#### Problème : le service ne se synchronise pas

- Vérifier la connectivité réseau vers le serveur NTP.
- Vérifier que le port UDP 123 n'est pas bloqué par un pare-feu.
- Vérifier la validité des serveurs configurés.
- Consulter les logs pour détecter des erreurs de configuration.
- Vérifier que l'heure initiale n'est pas trop éloignée (utiliser `ntpd -gq` ou `chronyc makestep`).

#### Problème : décalage important persistant

- Vérifier la stabilité de l'horloge matérielle.
- Augmenter le nombre de sources NTP.
- Vérifier l'asymétrie réseau.
- Vérifier la charge CPU et les interruptions système.

#### Problème : attaque par spoofing

- Utiliser NTS ou des clés d'authentification partagées.
- Restreindre les sources autorisées.
- Surveiller les changements de source NTP.

---

## PTP - Precision Time Protocol

### 6.1. Définition et rôle

PTP est un protocole de synchronisation temporelle conçu pour atteindre une précision extrêmement élevée sur des réseaux locaux. Il peut fournir une synchronisation à la microseconde, voire à la nanoseconde, en utilisant des horodatages matériels dans les cartes réseau et les équipements réseau.

PTP est utilisé dans :

- Les réseaux de télécommunications 4G/5G.
- La finance à haute fréquence.
- L'industrie 4.0 et les systèmes d'automatisation.
- L'audiovisuel et la diffusion (SMPTE ST 2059, AES67).
- Le transport ferroviaire et maritime.
- Les data centers et le cloud.
- L'aérospatial et la défense.
- Les systèmes de mesure et d'instrumentation scientifique.

### 6.2. Historique

- 2000-2002 : Développement du standard IEEE 1588 par un groupe de travail de l'IEEE.
- 2002 : Publication de IEEE 1588-2002 (PTPv1).
- 2008 : Publication de IEEE 1588-2008 (PTPv2), avec de nombreuses améliorations.
- 2012-2015 : Développement de profils spécifiques (telecom, power, enterprise).
- 2019 : Publication de IEEE 1588-2019, apportant des améliorations de sécurité et de performance.
- 2020-2026 : Adoption massive dans les réseaux 5G, les smart grids, et l'industrie connectée.

### 6.3. Architecture PTP

L'architecture PTP repose sur plusieurs types d'horloges :

- **Grandmaster clock (GM)** : Horloge maîtresse de référence, généralement synchronisée sur GPS/GNSS.
- **Slave clock** : Horloge esclave synchronisée sur le grandmaster.
- **Boundary clock (BC)** : Horloge intermédiaire qui se synchronise en amont et sert de maître en aval.
- **Transparent clock (TC)** : Équipement réseau qui mesure le temps de transit des messages PTP et le compense.
- **Ordinary clock (OC)** : Horloge avec une seule interface PTP, soit maître, soit esclave.

Les messages PTP circulent généralement en multicast (adresse MAC 01:1B:19:00:00:00 et adresse IP 224.0.1.129) ou en unicast.

### 6.4. Fonctionnement détaillé

PTP utilise un mécanisme d'échange de messages pour mesurer le délai entre un maître et un esclave et synchroniser les horloges.

#### Synchronisation de fréquence et de phase

1. Le maître envoie périodiquement un message **Sync** en notant l'heure d'envoi `t1`.
2. L'esclave reçoit le Sync en notant l'heure de réception `t2`.
3. Si l'horodatage matériel n'est pas inclus dans le Sync, le maître envoie ensuite un message **Follow_Up** contenant `t1`.
4. L'esclave envoie un message **Delay_Req** en notant l'heure d'envoi `t3`.
5. Le maître reçoit le Delay_Req et note l'heure `t4`, puis envoie un **Delay_Resp** contenant `t4`.

Les formules sont :

```
délai_aller_retour = (t2 - t1) + (t4 - t3)
délai_moyen = délai_aller_retour / 2
décalage = t2 - t1 - délai_moyen
```

L'esclave utilise ces valeurs pour ajuster son horloge locale et se synchroniser sur le maître.

### 6.5. Domaines, horloges et messages PTP

#### Domaines PTP

Un **domaine PTP** est un ensemble d'horloges partageant le même identifiant de domaine. Plusieurs domaines peuvent coexister sur le même réseau physique. Les domaines sont identifiés par un nombre de 0 à 255.

Exemples :

- Domaine 0 : usage général.
- Domaine 1-3 : réservés pour des applications spécifiques.

#### Messages PTP

Les messages PTP se divisent en deux catégories :

**Messages d'événement (timestampés) :**

- **Sync** : Synchronisation de l'esclave sur le maître.
- **Delay_Req** : Demande de mesure du délai par l'esclave.
- **Pdelay_Req** : Demande de mesure du délai peer-to-peer.
- **Pdelay_Resp** : Réponse à Pdelay_Req.

**Messages généraux (non timestampés) :**

- **Follow_Up** : Transmet l'horodatage du Sync.
- **Delay_Resp** : Transmet l'horodatage du Delay_Req.
- **Pdelay_Resp_Follow_Up** : Transmet l'horodatage du Pdelay_Resp.
- **Announce** : Annonce les propriétés du maître et permet l'élection du meilleur maître (BMC).
- **Signaling** : Messages de signalisation pour la configuration.
- **Management** : Messages de gestion.

### 6.6. Types d'horloges PTP

#### Ordinary clock (OC)

Horloge PTP avec une seule interface. Elle peut être :

- **Master** : Fournit le temps aux esclaves.
- **Slave** : Reçoit le temps d'un maître.

#### Boundary clock (BC)

Horloge avec plusieurs interfaces. Elle se comporte comme esclave en amont et comme maître en aval. Les boundary clocks sont essentiels pour traverser les réseaux complexes et maintenir la précision.

#### Transparent clock (TC)

Équipement réseau (commutateur, routeur) qui mesure le temps de résidence des messages PTP et l'ajoute au champ correctionField. Cela permet de compenser les délais introduits par l'équipement.

#### Grandmaster clock (GM)

Horloge de référence ultime du domaine PTP. Elle est généralement synchronisée sur GPS/GNSS ou une horloge atomique.

### 6.7. Délai et asymétrie

La précision de PTP dépend fortement de la connaissance exacte du délai de propagation entre le maître et l'esclave. L'**asymétrie** (différence entre le délai aller et le délai retour) est la principale source d'erreur.

Les méthodes de mesure du délai sont :

- **End-to-end (E2E)** : Le délai est mesuré entre le maître et l'esclave via les messages Delay_Req/Delay_Resp.
- **Peer-to-peer (P2P)** : Chaque lien entre pairs mesure son propre délai via les messages Pdelay_Req/Pdelay_Resp.

La méthode P2P permet une correction plus fine dans les réseaux avec des transparent clocks.

### 6.8. Profils PTP

L'IEEE 1588 définit un cadre général, mais de nombreux profils d'application existent pour des secteurs spécifiques :

- **Default (IEEE 1588-2008)** : Profil par défaut, multicast ou unicast.
- **Telecom (ITU-T G.8265.1, G.8275.1, G.8275.2)** : Profils pour les réseaux de télécommunications.
- **Power (IEEE C37.238)** : Profil pour les smart grids et les systèmes électriques.
- **Enterprise (IEEE 802.1AS / gPTP)** : Profil pour les réseaux Ethernet industriels et l'audiovisuel.
- **SMPTE ST 2059-2** : Profil pour l'industrie du cinéma et de la diffusion.
- **AES67** : Profil pour l'audio professionnel sur IP.
- **Automotive (IEEE 802.1AS)** : Utilisé dans les véhicules connectés.

### 6.9. Ports et sécurité

PTP utilise principalement le protocole Ethernet de couche 2 (Ethertype 0x88F7) ou UDP sur les ports **319** et **320**.

- **Port 319** : Messages d'événement (Sync, Delay_Req, Pdelay_Req, Pdelay_Resp).
- **Port 320** : Messages généraux (Follow_Up, Delay_Resp, Announce, Management, Signaling).

Bonnes pratiques de sécurité PTP :

- Isoler le trafic PTP sur un VLAN dédié.
- Activer l'authentification des messages PTP si le matériel le supporte.
- Surveiller les annonces de grands maîtres.
- Utiliser des boundary clocks de confiance.
- Protéger la source GPS/GNSS contre le brouillage et le spoofing.
- Mettre à jour les firmwares des équipements PTP.
- Utiliser des mécanismes de redondance (PRTC, alternate grandmaster).

### 6.10. Configuration PTP

#### Installation de linuxptp

Sous Debian/Ubuntu :

```bash
sudo apt update
sudo apt install linuxptp
```

Sous RHEL/CentOS/Rocky :

```bash
sudo dnf install linuxptp
```

#### Configuration d'un grandmaster PTP (ptp4l)

Fichier `/etc/linuxptp/ptp4l.conf` :

```ini
[global]
priority1 128
priority2 128
domainNumber 0
twoStepFlag 1
slaveOnly 0
network_transport L2
```

Lancement du grandmaster :

```bash
sudo ptp4l -f /etc/linuxptp/ptp4l.conf -i eth0 -m
```

#### Configuration d'un esclave PTP

```ini
[global]
priority1 255
priority2 255
domainNumber 0
slaveOnly 1
network_transport L2
```

Lancement de l'esclave :

```bash
sudo ptp4l -f /etc/linuxptp/ptp4l.conf -i eth0 -m -s
```

#### Synchronisation de l'horloge système (phc2sys)

L'outil `phc2sys` synchronise l'horloge physique (PHC) de la carte réseau avec l'horloge système.

```bash
sudo phc2sys -s eth0 -c CLOCK_REALTIME -m
```

Pour synchroniser un slave sur le maître via la PHC :

```bash
sudo phc2sys -s /dev/ptp0 -c CLOCK_REALTIME -m
```

### 6.11. Vérification et tests sous Linux

#### Vérifier les interfaces PTP supportées

```bash
ethtool -T eth0
```

Sortie typique :

```
Time stamping parameters for eth0:
Capabilities:
  hardware-transmit     (SOF_TIMESTAMPING_TX_HARDWARE)
  software-transmit     (SOF_TIMESTAMPING_TX_SOFTWARE)
  hardware-receive      (SOF_TIMESTAMPING_RX_HARDWARE)
  software-receive      (SOF_TIMESTAMPING_RX_SOFTWARE)
  software-system-clock (SOF_TIMESTAMPING_SOFTWARE)
  hardware-raw-clock    (SOF_TIMESTAMPING_RAW_HARDWARE)
PTP Hardware Clock: 0
Hardware Transmit Timestamp Modes:
  off                   (HWTSTAMP_TX_OFF)
  on                    (HWTSTAMP_TX_ON)
Hardware Receive Filter Modes:
  none                  (HWTSTAMP_FILTER_NONE)
  all                   (HWTSTAMP_FILTER_ALL)
```

#### Lancer ptp4l en mode test

```bash
sudo ptp4l -i eth0 -m
```

Sortie typique en mode esclave :

```
ptp4l[1234567.123]: selected /dev/ptp0 as PTP clock
ptp4l[1234567.124]: port 1: INITIALIZING to LISTENING on INIT_COMPLETE
ptp4l[1234567.125]: port 1: new foreign master 00:11:22:33:44:55-1
ptp4l[1234567.126]: selected best master clock 00:11:22:33:44:55
ptp4l[1234567.127]: port 1: LISTENING to UNCALIBRATED on RS_SLAVE
ptp4l[1234567.128]: master offset          123 s0 freq      +5123 path delay       234
ptp4l[1234567.129]: master offset           45 s2 freq      +5234 path delay       234
ptp4l[1234567.130]: port 1: UNCALIBRATED to SLAVE on MASTER_CLOCK_SELECTED
```

#### Vérifier la synchronisation avec phc2sys

```bash
sudo phc2sys -s eth0 -c CLOCK_REALTIME -m
```

Sortie typique :

```
phc2sys[1234567.123]: CLOCK_REALTIME phc offset      1234 s0 freq      +12345 delay      1234
phc2sys[1234567.124]: CLOCK_REALTIME phc offset       567 s2 freq      +12456 delay      1234
```

#### Lister les horloges PTP disponibles

```bash
ls /dev/ptp*
```

#### Utiliser pmc (PTP Management Client)

```bash
sudo pmc -u -b 0 'GET CURRENT_DATA_SET'
sudo pmc -u -b 0 'GET PARENT_DATA_SET'
sudo pmc -u -b 0 'GET TIME_STATUS_NP'
```

#### Vérifier les ports en écoute

```bash
sudo ss -ulnp | grep -E ':319|:320'
```

### 6.12. Vérification et tests sous Windows

Sous Windows, PTP est moins natif que NTP. Les solutions incluent :

- Utiliser des logiciels tiers (TimeKeeper, ptpd, Meinberg PTP).
- Utiliser Windows Server 2019/2022 avec des fonctionnalités PTP limitées.
- Utiliser des cartes réseau supportant PTP avec leurs propres outils.

#### Tester avec Meinberg PTP

Télécharger et installer Meinberg PTP Monitor, puis exécuter :

```cmd
ptp_monitor.exe -i "Ethernet" -m
```

#### Vérifier la connectivité multicast PTP

```powershell
Test-NetConnection -ComputerName 224.0.1.129 -Port 319 -Udp
```

#### Utiliser Wireshark pour capturer les paquets PTP

Capturer sur le port UDP 319/320 ou sur l'Ethernet type 0x88F7.

### 6.13. Configuration avancée et optimisations

#### Utilisation de boundary clocks

Dans un réseau complexe, placer des boundary clocks entre les segments permet de :

- Réduire la charge du grandmaster.
- Isoler les domaines PTP.
- Améliorer la scalabilité.
- Réduire l'impact des variations de délai.

#### Utilisation de transparent clocks

Les commutateurs transparents doivent être configurés pour :

- Activer le PTP sur tous les ports.
- Mesurer et corriger le temps de résidence.
- Préserver la priorité des messages PTP.

#### Optimisation des horloges matérielles

- Utiliser des cartes réseau avec PHC (PTP Hardware Clock).
- Activer le timestamping matériel dans le BIOS/OS.
- Désactiver l'économie d'énergie des interfaces réseau.
- Utiliser des câbles et des commutateurs de qualité constante.

### 6.14. Dépannage PTP

#### Problème : l'esclave ne trouve pas de maître

- Vérifier que le multicast est bien acheminé.
- Vérifier les VLAN et les ACL.
- Vérifier que les interfaces supportent le timestamping matériel.
- Vérifier la configuration du domaine PTP.
- Vérifier que le maître est bien en ligne et émet des messages Announce.

#### Problème : décalage important ou instable

- Vérifier l'asymétrie des liens.
- Vérifier la charge CPU et les interruptions.
- Vérifier que les transparent clocks corrigent correctement le temps de résidence.
- Vérifier la qualité du câblage et des connecteurs.
- Vérifier la stabilité de la source GPS/GNSS.

#### Problème : perte de synchronisation fréquente

- Augmenter la priorité du grandmaster.
- Vérifier les conflits de BMC (Best Master Clock).
- Surveiller les annonces et les changements de maître.
- Utiliser une redondance de grands maîtres.

---

## Comparatif NTP / PTP

| Critère | NTP | PTP |
|---------|-----|-----|
| Précision typique | Milliseconde à dizaine de microsecondes | Microseconde à nanoseconde |
| Protocole de transport | UDP/IP | Ethernet L2 ou UDP/IP |
| Ports | UDP 123 | UDP 319/320, Ethertype 0x88F7 |
| Infrastructure | Serveurs stratum | Horloges, boundary clocks, transparent clocks |
| Complexité | Modérée | Élevée |
| Coût | Faible à modéré | Élevé (matériel spécifique) |
| Portée | Internet, LAN, WAN | LAN principalement |
| Utilisation typique | Serveurs, postes, réseau IT | Télécoms, finance, industrie, audiovisuel |
| Sécurité | NTS, clés partagées | Authentification, VLAN, isolation |
| Robustesse | Très robuste, large écosystème | Nécessite une infrastructure dédiée |

---

## Où et comment sont utilisés NTP et PTP

### NTP

NTP est omniprésent dans les systèmes informatiques :

- **Data centers** : Synchronisation des serveurs physiques et virtuels.
- **Cloud computing** : Synchronisation des instances et des services distribués.
- **Réseaux d'entreprise** : Synchronisation des routeurs, commutateurs, pare-feu, contrôleurs.
- **Systèmes d'exploitation** : Windows, Linux, macOS, BSD intègrent tous un client NTP.
- **IoT et embarqué** : Caméras, capteurs, objets connectés utilisent SNTP.
- **Finance** : Horodatage des transactions (moins exigeant que PTP dans certains cas).
- **Télécommunications** : Synchronisation des éléments de gestion et des OSS/BSS.
- **Cybersécurité** : Corrélation des logs, validité des certificats, authentification Kerberos.
- **Sciences et recherche** : Synchronisation d'instruments et de grappes de calcul.

### PTP

PTP est utilisé dans des contextes nécessitant une très haute précision :

- **Réseaux 5G** : Synchronisation des stations de base (gNB), du RAN et du cœur de réseau.
- **Finance à haute fréquence** : Horodatage précis des ordres et des transactions.
- **Industrie 4.0** : Synchronisation des robots, capteurs, actionneurs et systèmes de contrôle.
- **Smart grids** : Synchronisation des disjoncteurs, transformateurs et compteurs intelligents.
- **Audiovisuel** : Synchronisation lip-sync, diffusion multi-caméras, production IP.
- **Transport** : Synchronisation des systèmes de signalisation ferroviaire, maritime et aérien.
- **Data centers** : Synchronisation des bases de données distribuées et des applications temps réel.
- **Défense et aérospatial** : Synchronisation des systèmes critiques.
- **Métrologie et physique** : Expériences nécessitant une coordination temporelle extrême.

---

## Applications et logiciels utilisant NTP et PTP

### Logiciels et démons NTP

- **chrony** : Démon NTP moderne et performant.
- **ntpd** : Démon NTP classique du projet NTP.
- **OpenNTPD** : Démon NTP minimaliste d'OpenBSD.
- **systemd-timesyncd** : Client NTP léger intégré à systemd.
- **NTPsec** : Fork sécurisé de ntpd.
- **sntp** : Client SNTP simple.
- **ntpdate** : Outil ponctuel de synchronisation.
- **ntpq / ntpdc / chronyc** : Outils de contrôle.
- **Meinberg NTP** : Solutions NTP pour Windows et Linux.
- **TimeKeeper** : Logiciel de synchronisation NTP/PTP.

### Logiciels et démons PTP

- **linuxptp (ptp4l, phc2sys, pmc)** : Suite PTP pour Linux.
- **ptpd** : Implémentation PTP portable.
- **TimeKeeper** : Solution PTP commerciale.
- **Meinberg PTP** : Logiciels et cartes PTP.
- **IEEE 1588 PTP Stack** : Implémentations propriétaires des constructeurs.
- **PTP Track Hound** : Outil d'analyse PTP.
- **Wireshark** : Analyse des paquets PTP.

### Équipements matériels

- **Serveurs NTP/PTP** : Meinberg, Microsemi (Microchip), Orolia (Safran), Spectracom, EndRun.
- **Cartes réseau PTP** : Intel, Mellanox/NVIDIA, Broadcom, Solarflare.
- **Commutateurs PTP** : Cisco, Juniper, Arista, HPE, Huawei.
- **Récepteurs GPS/GNSS** : u-blox, Trimble, Septentrio, Furuno.
- **Horloges atomiques** : Oscillateurs à césium, rubidium, et horloges optiques.

### Outils de test et d'analyse

- **chronyc / ntpq** : Diagnostic NTP.
- **ptp4l / phc2sys / pmc** : Diagnostic PTP.
- **Wireshark** : Capture et analyse NTP/PTP.
- **tcpdump** : Capture réseau.
- **ethtool -T** : Vérification du timestamping matériel.
- **nmap** : Scan de ports NTP/PTP.
- **mtr / traceroute** : Diagnostic réseau.
- **iperf3** : Mesure de latence et gigue.
- **Oscilloscopes et analyseurs de temps** : Validation de la précision.

---

## Configuration sur les principaux serveurs et équipements

### Configuration chrony avancée

```ini
# Sources
pool time.google.com       iburst minpoll 6 maxpoll 10
pool time.cloudflare.com   iburst nts
pool fr.pool.ntp.org       iburst

# Serveur local
local stratum 10
allow 192.168.0.0/16

# Sécurité
ntsdumpdir /var/lib/chrony

# Performance
hwclockfile /etc/adjtime
makestep 1.0 3
maxdrift 500
maxupdateskew 100.0

# Logs
log tracking measurements statistics
logdir /var/log/chrony
```

### Configuration d'un routeur Cisco pour NTP

```cisco
ntp server 192.168.1.1
ntp server 192.168.1.2
ntp authenticate
ntp authentication-key 1 md5 SECRET_KEY
ntp trusted-key 1
ntp source-interface GigabitEthernet0/0
```

### Configuration d'un routeur Cisco pour PTP

```cisco
ptp mode boundary
ptp domain 0
ptp source ip 192.168.1.10
interface GigabitEthernet0/1
 ptp enable
 ptp transport ipv4
 ptp delay-request interval 0
```

### Configuration d'un commutateur Arista pour PTP

```eos
ptp mode boundary
ptp domain 0
ptp priority1 128
ptp priority2 128
interface Ethernet1
   ptp enable
```

### Configuration d'un serveur Windows comme client NTP

```cmd
w32tm /config /manualpeerlist:"0.pool.ntp.org 1.pool.ntp.org 2.pool.ntp.org" /syncfromflags:manual /update
net stop w32time && net start w32time
w32tm /resync
```

### Configuration d'un serveur NTP Meinberg

L'interface graphique de Meinberg permet de :

- Sélectionner les sources de référence.
- Configurer les restrictions d'accès.
- Activer le monitoring.
- Gérer les logs et les statistiques.
- Configurer des options avancées comme le leap smear.

---

## Sécurité de la synchronisation temporelle

### Menaces courantes

- **Spoofing NTP/PTP** : Un attaquant usurpe un serveur de temps légitime.
- **Attaque par déni de service** : Saturation du serveur de temps.
- **Attaque par amplification NTP** : Exploitation du mode monlist pour amplifier du trafic.
- **Brouillage GPS/GNSS** : Perturbation de la source de référence.
- **Spoofing GPS** : Falsification du signal GPS pour tromper le grandmaster.
- **Attaque man-in-the-middle** : Interception et modification des paquets de temps.
- **Retour dans le temps** : Forcer un système à accepter une heure passée, ce qui peut invalider des certificats.

### Bonnes pratiques

- Utiliser NTS pour sécuriser NTP.
- Restreindre l'accès aux serveurs de temps.
- Désactiver les commandes de contrôle publiques.
- Surveiller les changements de source et les écarts anormaux.
- Protéger les sources GNSS.
- Utiliser des horloges locales de secours (holdover).
- Valider les signatures et les certificats.
- Segmenter le réseau PTP sur un VLAN dédié.
- Mettre à jour régulièrement les logiciels et firmwares.
- Auditer régulièrement la configuration et les logs.

### NTS (Network Time Security)

NTS sécurise NTP en deux phases :

1. **NTS-KE (Key Exchange)** : Établissement d'une clé via TLS.
2. **NTV (NTPv4 with NTS Extension Fields)** : Authentification des paquets NTP avec cette clé.

Exemple de serveur NTS public :

- `time.cloudflare.com`
- `nts.netnod.se`
- `ptbtime1.ptb.de`

---

## Commandes de vérification réseau

### Linux

#### Vérifier la résolution DNS

```bash
nslookup pool.ntp.org
dig pool.ntp.org A
dig pool.ntp.org AAAA
```

#### Vérifier la connectivité UDP NTP

```bash
nc -u -vz pool.ntp.org 123
sudo nmap -sU -p 123 pool.ntp.org
```

#### Vérifier le timestamping matériel

```bash
ethtool -T eth0
```

#### Capturer des paquets NTP

```bash
sudo tcpdump -i eth0 -n port 123
```

#### Capturer des paquets PTP

```bash
sudo tcpdump -i eth0 -n ether proto 0x88f7
sudo tcpdump -i eth0 -n udp port 319 or udp port 320
```

#### Scanner les ports

```bash
sudo nmap -sU -p 123,319,320 time.example.com
sudo nmap -p 123,319,320 time.example.com
```

### Windows

#### Vérifier la connectivité NTP

```powershell
Test-NetConnection -ComputerName pool.ntp.org -Port 123 -Udp
```

#### Diagnostiquer w32tm

```cmd
w32tm /query /status
w32tm /query /peers
w32tm /stripchart /computer:time.windows.com /samples:5 /dataonly
w32tm /resync /force
```

#### Capturer avec Wireshark

Installer Wireshark et filtrer avec :

```
ntp
ptp
udp.port == 319 or udp.port == 320
eth.type == 0x88f7
```

---

## Exercices pratiques

### Exercice 1 : Configurer un client NTP avec chrony

Objectif : Synchroniser une machine Linux sur des serveurs NTP publics.

Étapes :
1. Installer chrony.
2. Configurer les serveurs `pool.ntp.org`.
3. Redémarrer le service.
4. Vérifier avec `chronyc sources` et `chronyc tracking`.

### Exercice 2 : Interroger un serveur NTP manuellement

Objectif : Utiliser `sntp` ou `ntpdate -q` pour obtenir l'heure d'un serveur.

```bash
sntp pool.ntp.org
ntpdig pool.ntp.org
```

### Exercice 3 : Configurer un serveur NTP local

Objectif : Transformer une machine Linux en serveur NTP pour le réseau local.

Étapes :
1. Configurer chrony avec `allow 192.168.1.0/24`.
2. Activer `local stratum 10`.
3. Redémarrer chrony.
4. Tester depuis un autre client du réseau.

### Exercice 4 : Configurer PTP sur deux machines Linux

Objectif : Synchroniser deux machines avec linuxptp.

Étapes :
1. Installer linuxptp sur les deux machines.
2. Configurer une machine en grandmaster (`slaveOnly 0`).
3. Configurer l'autre en esclave (`slaveOnly 1`).
4. Lancer `ptp4l` sur les deux machines.
5. Observer la synchronisation avec `phc2sys`.

### Exercice 5 : Analyser le trafic NTP avec Wireshark

Objectif : Capturer et analyser des paquets NTP.

Étapes :
1. Lancer Wireshark.
2. Filtrer le trafic NTP (`port 123`).
3. Observer les champs Leap Indicator, Stratum, Precision, Reference ID, timestamps.

### Exercice 6 : Analyser le trafic PTP avec Wireshark

Objectif : Capturer et analyser des paquets PTP.

Étapes :
1. Lancer Wireshark sur un réseau PTP.
2. Filtrer avec `ptp` ou `eth.type == 0x88f7`.
3. Observer les messages Sync, Follow_Up, Delay_Req, Delay_Resp, Announce.

### Exercice 7 : Sécuriser NTP avec NTS

Objectif : Configurer chrony pour utiliser des serveurs NTS.

Étapes :
1. Configurer `server time.cloudflare.com iburst nts`.
2. Vérifier l'authentification avec `chronyc -N authdata`.
3. Capturer le trafic et vérifier l'utilisation de TLS pour le key exchange.

---

## Ressources, livres et liens

### RFCs et standards essentiels

- RFC 868 - Time Protocol (1983)
- RFC 958 - Network Time Protocol (NTP) (1985)
- RFC 1059 - Network Time Protocol (Version 1) (1988)
- RFC 1119 - Network Time Protocol (Version 2) (1989)
- RFC 1305 - Network Time Protocol (Version 3) (1992)
- RFC 2030 - Simple Network Time Protocol (SNTP) Version 4 (1996)
- RFC 5905 - Network Time Protocol Version 4: Protocol and Algorithms Specification (2010)
- RFC 5906 - Autokey Specification and Cryptography (2010)
- RFC 5907 - Definitions of Managed Objects for NTP (2010)
- RFC 7384 - Security Requirements of Time Protocols in Packet Switched Networks (2014)
- RFC 8573 - Message Authentication Code for NTP (2019)
- RFC 8915 - Network Time Security for NTP (2020)
- IEEE 1588-2002 - IEEE Standard for a Precision Clock Synchronization Protocol for Networked Measurement and Control Systems
- IEEE 1588-2008 - IEEE Standard for a Precision Clock Synchronization Protocol for Networked Measurement and Control Systems
- IEEE 1588-2019 - IEEE Standard for a Precision Clock Synchronization Protocol for Networked Measurement and Control Systems
- ITU-T G.8261 - Timing and Synchronization Aspects in Packet Networks
- ITU-T G.8265.1 - Precision Time Protocol Telecom Profile for Frequency Synchronization
- ITU-T G.8275.1 - Precision Time Protocol Telecom Profile for Phase/Time Synchronization with Full Timing Support
- ITU-T G.8275.2 - Precision Time Protocol Telecom Profile for Phase/Time Synchronization with Partial Timing Support
- SMPTE ST 2059-1 - Generation and Alignment of Interface Signals for the SMPTE Epoch
- SMPTE ST 2059-2 - SMPTE Profile for Use of IEEE 1588 Precision Time Protocol
- IEEE 802.1AS - Timing and Synchronization for Time-Sensitive Applications (gPTP)
- IEC 61850-9-3 - Utility Communication Architecture - Precision Time Protocol Profile
- IEEE C37.238-2011 / 2017 - Power System Relaying Standard Profile for PTP

### Livres recommandés

- "Computer Network Time Synchronization: The Network Time Protocol on Earth and in Space" par David L. Mills
- "The NTP FAQ and HOWTO" par Various Authors
- "Precision Clock Synchronization Protocol for Networked Measurement and Control Systems (IEEE 1588)" par John C. Eidson
- "Measurement, Control, and Communication Using IEEE 1588" par John C. Eidson
- "Time Frequency Measurement and Control" par J. A. Barnes
- "GPS and GNSS Technology in Geosciences" par Various Authors
- "Network Time Protocol (NTP) Project Reference Library" par Harlan Stenn
- "Understanding Linux Network Internals" par Christian Benvenuti
- "TCP/IP Illustrated" par W. Richard Stevens
- "Computer Networking: A Top-Down Approach" par James F. Kurose et Keith W. Ross

### Liens utiles

- https://www.ntp.org/
- https://chrony.tuxfamily.org/
- https://linuxptp.sourceforge.net/
- https://www.ieee802.org/1/pages/802.1as.html
- https://www.itu.int/rec/T-REC-G/en
- https://www.ptb.de/cms/en/ptb/fachabteilungen/abt4/fb-44/ag-441.html
- https://time.cloudflare.com/
- https://www.pool.ntp.org/
- https://support.ntp.org/
- https://datatracker.ietf.org/
- https://www.meinbergglobal.com/
- https://www.microsemi.com/
- https://www.orolia.com/
- https://www.endruntechnologies.com/
- https://www.spectracomcorp.com/
- https://wireshark.org/
- https://www.nist.gov/pml/time-and-frequency-division
- https://www.bipm.org/en/time-ftp

### Applications et outils

- chrony
- ntpd / NTPsec
- systemd-timesyncd
- linuxptp
- ptpd
- Meinberg NTP / PTP
- TimeKeeper
- PTP Track Hound
- Wireshark
- tcpdump
- ethtool
- nmap
- iperf3
- w32tm (Windows)
- Date and Time Settings (Windows)
- GNSS receivers (u-blox, Trimble, Septentrio)
- Oscilloscopes et compteurs d'intervalles

---

## Glossaire

- **BMC (Best Master Clock)** : Algorithme d'élection du meilleur maître PTP.
- **Boundary Clock (BC)** : Horloge PTP relaistant la synchronisation entre segments.
- **Chrony** : Démon NTP moderne pour Linux.
- **Drift** : Dérive de l'horloge locale.
- **Epoch** : Date de référence pour les timestamps (1er janvier 1970 pour Unix).
- **Follow_Up** : Message PTP transmettant l'horodatage d'un Sync.
- **GNSS** : Global Navigation Satellite System (GPS, Galileo, GLONASS, BeiDou).
- **Grandmaster Clock** : Horloge maître de référence PTP.
- **Gigue (Jitter)** : Variation de la latence réseau.
- **Leap Second** : Seconde intercalaire ajoutée pour synchroniser UTC avec la rotation terrestre.
- **NTP** : Network Time Protocol.
- **NTS** : Network Time Security.
- **Ordinary Clock** : Horloge PTP avec une seule interface.
- **PHC (PTP Hardware Clock)** : Horloge matérielle dédiée dans une carte réseau.
- **PTP** : Precision Time Protocol.
- **RTC** : Real-Time Clock, horloge matérielle.
- **Second intercalaire** : Ajustement de UTC pour tenir compte de la rotation terrestre.
- **Slave Clock** : Horloge esclave PTP synchronisée sur un maître.
- **SNTP** : Simple Network Time Protocol.
- **Stratum** : Niveau dans la hiérarchie NTP.
- **Sync** : Message PTP de synchronisation.
- **TAI** : Temps Atomique International.
- **Timestamp** : Valeur représentant un instant précis.
- **Transparent Clock (TC)** : Équipement réseau compensant le temps de transit PTP.
- **UTC** : Temps Universel Coordonné.

---

## Conclusion

Les protocoles NTP et PTP sont indispensables à la synchronisation temporelle des systèmes modernes. NTP assure une synchronisation fiable et précise à l'échelle d'Internet, des data centers et des réseaux d'entreprise. PTP répond aux besoins les plus exigeants en matière de précision, notamment dans les télécommunications, la finance, l'industrie et l'audiovisuel.

La maîtrise de ces protocoles nécessite une compréhension approfondie de leurs architectures, de leurs modes de fonctionnement, de leurs commandes de vérification et de leurs bonnes pratiques de sécurité. Avec l'évolution des réseaux 5G, de l'industrie 4.0, de l'IoT et des systèmes critiques, la synchronisation temporelle reste un enjeu majeur de fiabilité, de performance et de sécurité.

Ce document, rédigé par **hackers_tchad** en 2026, constitue une référence avancée pour comprendre, configurer, tester et sécuriser NTP et PTP. Il couvre les aspects théoriques, pratiques et opérationnels, avec des commandes pour Linux et Windows, des exemples de configuration, des exercices et de nombreuses ressources complémentaires.

Nous encourageons le lecteur à expérimenter dans un environnement de test, à consulter les RFCs et standards officiels, et à rester informé des évolutions technologiques et des menaces. La précision temporelle est un pilier silencieux mais essentiel de notre société numérique.

---

**Fin du document.**
