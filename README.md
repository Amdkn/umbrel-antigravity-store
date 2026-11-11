# Magasin Umbrel — Antigravity (Amdkn)

Magasin communautaire Umbrel avec une seule appli : **Antigravity**.

## L'appli

L'agent Antigravity (`agy`, image officielle Google `docker.io/sbx/antigravity:1.0.0`,
épinglée par digest) tourne en contrôle à distance sur l'Umbrel. On le pilote depuis
[antigravity.google.com](https://antigravity.google.com/) en choisissant la machine
nommée `umbrel`. Pas besoin de laisser le PC allumé.

L'agent n'a pas d'interface web locale : la tuile Umbrel ouvre une page d'état qui
explique où le piloter.

## Installer

1. Dans Umbrel : **App Store → ⋯ → Community App Stores → Add**.
2. Coller l'adresse du magasin :
   `https://github.com/Amdkn/umbrel-antigravity-store`
3. Ouvrir le magasin **Amdkn** et installer **Antigravity**.

## Première utilisation (autorisation Google)

Au premier démarrage, l'agent doit être autorisé avec un compte Google. Le lien
d'autorisation s'affiche dans les journaux de l'appli (tableau de bord Umbrel →
Antigravity → journaux). L'autorisation est ensuite conservée dans les données de
l'appli et survit aux redémarrages.

## Détails techniques

- Image : `docker.io/sbx/antigravity:1.0.0` (digest épinglé dans `docker-compose.yml`).
- Commande : `agy remote-control start --name umbrel`.
- Pas de `--dangerously-skip-permissions` : les approbations d'outils restent sous
  ton contrôle depuis la console web Antigravity.
- Données persistantes : `.config`, `.local/share`, `.local/state` et `workspace` sous `/home/agent` (autorisation, configuration, espace de travail). Le dossier `/home/agent` lui-même n'est JAMAIS monté : le binaire `agy` vit dans l'image à `/home/agent/.local/bin/agy`, et un montage par-dessus le cacherait (premier install échoué : « agy: executable file not found in $PATH »).
- Page d'état : petit serveur statique sur le port 8080 (tuile Umbrel).

## Vérifications faites à l'empaquetage

- Manifeste copié sur les champs exacts d'une appli officielle (`code-server`).
- Image vérifiée au registre Docker Hub (tags `1.0.0` / `latest`, digest de la liste
  de manifestes, config de l'image lue : utilisateur `agent`, entrée `agy`,
  aucun port exposé).
- CLI `agy` 1.5.5 installé et `remote-control start --help` vérifié sur la machine
  de construction.
- Non vérifié par un lancement réel dans un conteneur (pas de Docker sur la machine
  de construction) : le premier démarrage sur l'Umbrel reste le test réel.
