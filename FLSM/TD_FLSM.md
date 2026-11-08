# Travaux dirigés — FLSM

## Consignes générales

Pour chaque exercice, présenter les calculs et un tableau contenant le numéro du sous-réseau, l'adresse réseau, la première adresse hôte, la dernière adresse hôte et le broadcast. Sauf indication contraire, on exclut réseau et broadcast du nombre d'hôtes utilisables.

## Exercices

1. Découper `192.168.10.0/24` en 4 sous-réseaux FLSM. Donner préfixe, masque, incrément et hôtes utilisables.
2. Découper `192.168.20.0/24` en 8 sous-réseaux et dresser le tableau complet.
3. À partir de `172.16.0.0/16`, créer des sous-réseaux pouvant accueillir au moins 50 hôtes. Donner le nombre maximal de sous-réseaux.
4. À partir de `172.20.0.0/16`, créer des sous-réseaux d'au moins 500 hôtes et donner les quatre premiers.
5. Pour `192.168.5.77/26`, calculer réseau, broadcast et plage hôte. Vérifier en binaire.
6. Diviser `150.10.0.0/16` en 32 sous-réseaux identiques et calculer les hôtes par sous-réseau.
7. Découper `10.1.4.0/24` en 16 sous-réseaux et écrire tous les réseaux et broadcasts.
8. Pour `172.31.19.200/20`, déterminer le sous-réseau, le broadcast et la plage d'hôtes.
9. Diviser `10.10.0.0/16` en 16 départements universitaires et proposer les passerelles des quatre premiers.
10. Diviser `192.168.100.0/24` entre Administration, Production, Invités et Téléphonie.
11. Vérifier et corriger les réseaux `/27` proposés : `192.168.1.0`, `.32`, `.60`, `.96`.
12. Proposer les routes statiques entre deux routeurs desservant quatre sous-réseaux `/26` de `192.168.10.0/24`.
13. Réaliser dans Packet Tracer quatre LAN `/26` issus de `192.168.50.0/24`, puis vérifier la connectivité.
14. Quatre services ont besoin de 8, 15, 40 et 60 hôtes. Calculer le gaspillage avec quatre `/26` et juger le choix FLSM.
15. Découper `172.16.0.0/20` en 8 sous-réseaux, puis comparer avec une solution VLSM pour 1000, 400, 100 et 30 hôtes.

Le corrigé expliqué se trouve dans [`TD_FLSM_Corrige.md`](TD_FLSM_Corrige.md).

