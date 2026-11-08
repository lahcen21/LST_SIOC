# FLSM : Fixed-Length Subnet Mask

## 1. Présentation

Le FLSM (*Fixed-Length Subnet Mask*) est une technique de subnetting IPv4 qui divise un réseau en sous-réseaux de **même taille**. Tous les sous-réseaux utilisent donc le même masque et offrent le même nombre d'adresses.

[![Découpage FLSM d’un /24 en quatre sous-réseaux /26](images/flsm-subnetting.png)](images/flsm-subnetting.png)

Le FLSM est adapté lorsque les segments réseau ont des besoins comparables. Il est simple à documenter et à administrer, mais peut gaspiller des adresses si les besoins sont très différents.

## 2. Rappels IPv4

Une adresse IPv4 contient 32 bits, écrits en quatre octets : `192.168.1.10`. Le préfixe CIDR indique le nombre de bits réservés au réseau : `/24` signifie 24 bits réseau et 8 bits hôte.

| Élément | Rôle |
|---|---|
| Adresse réseau | Première adresse du sous-réseau, non attribuable à une machine |
| Adresses hôtes | Adresses attribuables aux interfaces |
| Broadcast | Dernière adresse, utilisée pour la diffusion |
| Masque | Sépare les bits réseau des bits hôte |

Pour un sous-réseau classique, le nombre d'hôtes utilisables est `2^h - 2`, où `h` est le nombre de bits hôte. On retire l'adresse réseau et l'adresse broadcast.

## 3. Principe du FLSM

On « emprunte » `n` bits à la partie hôte. Le nombre de sous-réseaux obtenus est `2^n`, et le nouveau préfixe vaut :

`nouveau préfixe = ancien préfixe + n`

Le nombre de bits hôte restants est `h = 32 - nouveau préfixe`.

| Besoin | Formule |
|---|---|
| Sous-réseaux | `2^n` |
| Hôtes utilisables par sous-réseau | `2^h - 2` |
| Adresses totales par sous-réseau | `2^h` |

## 4. Méthode générale de résolution

1. Identifier le réseau de départ et son préfixe.
2. Déterminer le nombre minimal de sous-réseaux requis.
3. Choisir `n` tel que `2^n` soit supérieur ou égal à ce besoin.
4. Ajouter `n` au préfixe initial.
5. Calculer le nombre d'hôtes disponibles.
6. Déterminer l'incrément : `256 - valeur du masque dans l'octet intéressant`.
7. Lister les adresses réseau, broadcast et plages hôtes.
8. Vérifier qu'il n'y a ni chevauchement ni adresse oubliée.

## 5. Exemple détaillé : 192.168.1.0/24 en quatre sous-réseaux

Besoin : quatre sous-réseaux égaux.

- `2^2 = 4` : on emprunte 2 bits.
- Nouveau préfixe : `/24 + 2 = /26`.
- Masque décimal : `255.255.255.192`.
- Bits hôte : `32 - 26 = 6`.
- Adresses par sous-réseau : `2^6 = 64`.
- Hôtes utilisables : `64 - 2 = 62`.
- Incrément : `256 - 192 = 64`.

| N° | Adresse réseau | Première adresse hôte | Dernière adresse hôte | Broadcast |
|---:|---|---|---|---|
| 1 | `192.168.1.0/26` | `192.168.1.1` | `192.168.1.62` | `192.168.1.63` |
| 2 | `192.168.1.64/26` | `192.168.1.65` | `192.168.1.126` | `192.168.1.127` |
| 3 | `192.168.1.128/26` | `192.168.1.129` | `192.168.1.190` | `192.168.1.191` |
| 4 | `192.168.1.192/26` | `192.168.1.193` | `192.168.1.254` | `192.168.1.255` |

## 6. Exemple avec une contrainte d'hôtes

Réseau `172.16.0.0/16`, besoin d'au moins 500 hôtes par sous-réseau.

Il faut `2^h - 2 >= 500`. Avec 9 bits : `510` hôtes utilisables. Le nouveau préfixe est donc `/23` (`32 - 9`). Chaque sous-réseau possède 512 adresses et l'incrément est 2 dans le troisième octet : `172.16.0.0/23`, `172.16.2.0/23`, `172.16.4.0/23`, etc.

## 7. Calcul binaire

Le masque `/26` s'écrit `11111111.11111111.11111111.11000000`, soit `255.255.255.192`. Les 26 premiers bits identifient le réseau et les 6 derniers identifient les hôtes. L'opération logique AND entre une adresse IP et son masque donne l'adresse réseau.

## 8. FLSM et routage

Les routeurs doivent connaître les préfixes de chaque sous-réseau. En routage statique, on ajoute une route vers chaque préfixe. Avec OSPF, RIP ou EIGRP, les préfixes sont annoncés par le protocole dynamique. La régularité du FLSM facilite la documentation et le résumé de routes lorsque les réseaux sont contigus.

## 9. Avantages et limites

| Avantages | Limites |
|---|---|
| Calcul et configuration simples | Taille identique imposée |
| Plan d'adressage prévisible | Gaspillage d'adresses possible |
| Documentation uniforme | Peu adapté aux organisations hétérogènes |
| Dépannage facilité | Moins efficace que le VLSM |

## 10. Comparaison FLSM / VLSM

Le FLSM utilise un seul masque pour tous les sous-réseaux. Le VLSM (*Variable-Length Subnet Mask*) autorise des masques différents et attribue à chaque segment une taille adaptée. Le FLSM est préférable pour sa simplicité pédagogique et opérationnelle ; le VLSM est préférable lorsque l'espace IPv4 est limité.

## 11. Erreurs fréquentes

- Confondre adresse réseau et première adresse hôte.
- Attribuer l'adresse broadcast à une machine.
- Utiliser `2^h` au lieu de `2^h - 2` pour les hôtes classiques.
- Oublier que le masque doit être identique en FLSM.
- Calculer un incrément dans le mauvais octet.
- Ne pas vérifier le chevauchement des plages.

## 12. Synthèse

Le FLSM consiste à diviser un réseau en blocs égaux. La démarche repose sur trois calculs : nombre de sous-réseaux `2^n`, nombre d'hôtes `2^h - 2` et incrément du masque. Une réponse correcte doit toujours fournir le préfixe, le masque décimal, les adresses réseau, les plages hôtes et les broadcasts.

## 13. Objectifs pédagogiques

À l'issue de ce chapitre, l'étudiant doit être capable de :

- expliquer la différence entre réseau, hôte et broadcast ;
- convertir un préfixe CIDR en masque décimal et binaire ;
- déterminer le nombre de bits à emprunter ;
- calculer le nombre de sous-réseaux et d'hôtes ;
- construire un tableau d'adressage sans chevauchement ;
- vérifier une adresse avec une opération AND ;
- justifier le choix du FLSM face au VLSM ;
- appliquer le plan obtenu dans un simulateur réseau.

## 14. Vocabulaire essentiel

| Terme | Définition |
|---|---|
| CIDR | Écriture d'une adresse suivie du nombre de bits réseau, par exemple `/26` |
| Préfixe | Nombre de bits à 1 dans le masque |
| Bit emprunté | Bit retiré à la partie hôte pour créer des sous-réseaux |
| Incrément | Écart entre deux adresses réseau consécutives |
| Supernetting | Regroupement de plusieurs réseaux en un préfixe plus large |
| Broadcast dirigé | Diffusion vers tous les hôtes d'un sous-réseau donné |

## 15. Table de correspondance des préfixes

| Préfixe | Masque | Hôtes utilisables |
|---:|---|---:|
| `/24` | `255.255.255.0` | 254 |
| `/25` | `255.255.255.128` | 126 |
| `/26` | `255.255.255.192` | 62 |
| `/27` | `255.255.255.224` | 30 |
| `/28` | `255.255.255.240` | 14 |
| `/29` | `255.255.255.248` | 6 |
| `/30` | `255.255.255.252` | 2 |

Cette table est utile pour résoudre rapidement un exercice. Pour une liaison point à point, `/30` fournit deux adresses utilisables ; dans des environnements modernes, `/31` peut être utilisé selon le support du matériel et du protocole.

## 16. Exemple de lecture binaire complet

Prenons `192.168.1.130/26` :

```text
Adresse : 11000000.10101000.00000001.10000010
Masque  : 11111111.11111111.11111111.11000000
AND     : 11000000.10101000.00000001.10000000
Réseau  : 192.168.1.128
```

Les six derniers bits sont réservés aux hôtes. Le réseau est donc `192.168.1.128/26`, le broadcast `192.168.1.191` et la plage utilisable `192.168.1.129–192.168.1.190`.

## 17. Exemple de conception pour une entreprise

Une entreprise possède `192.168.50.0/24` et souhaite quatre réseaux de même capacité : administration, enseignement, téléphonie et invités. Le choix `/26` offre 62 hôtes par service.

| Service | Réseau | Passerelle proposée | Broadcast |
|---|---|---|---|
| Administration | `192.168.50.0/26` | `192.168.50.1` | `192.168.50.63` |
| Enseignement | `192.168.50.64/26` | `192.168.50.65` | `192.168.50.127` |
| Téléphonie | `192.168.50.128/26` | `192.168.50.129` | `192.168.50.191` |
| Invités | `192.168.50.192/26` | `192.168.50.193` | `192.168.50.255` |

La passerelle est une convention d'administration : elle pourrait être la première ou la dernière adresse utilisable, à condition que le choix soit documenté et appliqué partout.

## 18. Vérification d'un plan d'adressage

Avant de déployer un plan FLSM, effectuer les contrôles suivants :

1. Le masque est identique pour tous les sous-réseaux.
2. Chaque adresse réseau tombe sur un multiple de l'incrément.
3. Le broadcast est l'adresse juste avant le réseau suivant.
4. Les plages hôtes excluent réseau et broadcast.
5. Les passerelles appartiennent à leur propre plage.
6. Les routes annoncées correspondent exactement aux préfixes utilisés.

Une vérification pratique peut utiliser `ping`, `tracert` ou `traceroute`, `ipconfig`/`ifconfig`, ainsi que les commandes Cisco `show ip interface brief` et `show ip route`.

## 19. Configuration Cisco indicative

Exemple pour le premier sous-réseau :

```text
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.192
Router(config-if)# no shutdown
Router(config-if)# exit
Router# show ip interface brief
```

Un poste du même réseau peut recevoir `192.168.1.10/26` avec la passerelle `192.168.1.1`. Un poste appartenant au deuxième sous-réseau doit utiliser une adresse comprise entre `192.168.1.65` et `192.168.1.126`, avec une passerelle du deuxième sous-réseau.

## 20. Limites et décision d'architecture

Le FLSM convient lorsque les services ont des tailles proches, lorsque la simplicité prime, ou dans un exercice d'apprentissage. Il devient inefficace lorsqu'un service nécessite 200 hôtes et un autre seulement 5 : imposer la même taille conduit à réserver beaucoup d'adresses inutilisées. Dans ce cas, on conserve le même raisonnement de base mais on choisit le VLSM, qui attribue un préfixe différent à chaque besoin.

## 21. Résumé opérationnel

Pour résoudre rapidement un problème FLSM :

```text
Besoin de sous-réseaux → choisir n avec 2^n
Nouveau préfixe       → préfixe initial + n
Bits hôte restants    → 32 - nouveau préfixe
Hôtes utilisables     → 2^(bits hôte) - 2
Incrément             → 256 - valeur du masque
Table finale          → réseau, hôtes, broadcast
```

Cette séquence doit être accompagnée d'une vérification logique et, si possible, d'un test dans Packet Tracer ou GNS3.

