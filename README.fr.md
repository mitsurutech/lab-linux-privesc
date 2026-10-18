# Lab d’élévation de privilèges sous Linux : un binaire setuid, une règle sudo et une tâche cron modifiable

[English](README.md) | [Français](README.fr.md) | [日本語](README.ja.md) | [Español](README.es.md)

[![check](https://github.com/mitsurutech/lab-linux-privesc/actions/workflows/check.yml/badge.svg)](https://github.com/mitsurutech/lab-linux-privesc/actions/workflows/check.yml)

Un **lab d’élévation de privilèges sous Linux** prêt à l’emploi pour la classe, issu de la [plateforme d’apprentissage MitsuruTech](https://mitsurutech.co.uk/fr/learning), où les écoles et les établissements donnent à chaque élève ses propres machines et son terminal dans le navigateur. Chaque élève reçoit sa propre VM Ubuntu, avec un second compte à faibles privilèges déjà présent, `intern`, et trois erreurs d’administrateur ordinaires semées à son intention : un programme utilitaire qui porte le bit setuid qu’il n’aurait jamais dû avoir, une règle sudo qui permet à `intern` de lancer un éditeur en tant que root sans mot de passe, et un script de maintenance que le cron de root exécute chaque minute et que tout le monde peut modifier. La mission est d’utiliser l’une des trois pour devenir root, de lire `/root/flag.txt` et de rédiger ce qui a été utilisé.

C’est un exercice de sécurité : la règle vient donc en premier. Attaquez uniquement votre propre VM. Elle n’a aucun accès à la machine d’un autre élève et aucun accès à internet en dehors des dépôts de paquets d’Ubuntu, de PyPI, de npm et de GitHub. N’appliquez jamais ce que vous apprenez ici à une machine qui n’est pas la vôtre : au Royaume-Uni, le Computer Misuse Act 1990 fait de cet acte, sans autorisation, une infraction pénale.

## Le lab

| Fichier | Ce que fait l’élève | Page d’aide |
| --- | --- | --- |
| [`linux-privesc.lab.yaml`](linux-privesc.lab.yaml) | Passer au compte à faibles privilèges `intern`, trouver l’une des trois faiblesses semées, s’en servir pour devenir root, et le prouver | [Lab d’élévation de privilèges sous Linux](https://mitsurutech.co.uk/fr/help/lab-linux-privesc) |

La configuration laisse dans le dossier personnel de l’élève un README qui énonce la règle, nomme le second compte et son mot de passe, et dit que root a laissé une porte d’entrée. Il ne donne aucune étape d’exploitation.

**Il n’a pas de script de notation, et c’est voulu.** Le propre compte de l’élève sur la VM a des droits d’administrateur : tout fichier qu’un script pourrait lire comme preuve, l’élève pourrait l’écrire en une ligne sans utiliser aucune des trois faiblesses. Nous l’avons essayé, et une vérification a réussi. Une note facile à falsifier est pire que pas de note, le travail est donc un court compte rendu écrit, quelle faiblesse, quelles commandes et ce que disait le flag, que l’enseignant lit. Corriger ensuite les faiblesses n’est pas demandé, et un enseignant peut le donner comme travail complémentaire.

## Ce que contient le fichier

`linux-privesc.lab.yaml` est une **définition de lab de VM** complète, exactement ce que la plateforme exécute :

| Clé | Ce que c’est |
| --- | --- |
| `cpus`, `memory`, `disk` | la taille de la VM de l’élève : 1 CPU, 1024 Mo et 6 Go |
| `open` | les ports qui s’ouvrent dans le navigateur : aucun, le lab est le terminal |
| `setup` | un script exécuté une fois, en tant que root, au premier démarrage de la VM : il crée le compte `intern`, écrit le flag, sème les trois faiblesses et écrit le README |

## L’utiliser sur la plateforme

**Pour le donner comme travail à une classe :**

1. Dans votre module, ajoutez une tâche et choisissez la carte **Lab de VM Ubuntu**.
2. Appuyez sur l’exemple pour partir de lui, ou collez le contenu de `linux-privesc.lab.yaml`. Utiliser un exemple remplace le texte, et Annuler rend le vôtre.
3. Publiez et essayez d’abord votre propre copie. Chaque élève obtient une VM à lui, conservée d’une session à l’autre.
4. Distribuez-la comme n’importe quelle tâche, et dans ses paramètres de rendu choisissez Documents pour que l’élève puisse rendre un compte rendu écrit. Lisez-le comme n’importe quel rendu : rien sur la machine ne le note.

Le [guide des labs de VM Ubuntu](https://mitsurutech.co.uk/fr/help/labs) du manuel et sa [page des labs d’exemple](https://mitsurutech.co.uk/fr/help/sample-labs) couvrent le reste. Pas encore de compte d’établissement ? [La première année est gratuite](https://mitsurutech.co.uk/fr/pricing) jusqu’à 50 élèves, et [l’inscription](https://mitsurutech.co.uk/fr/signup) ne demande qu’une adresse e-mail.

## Vérifier le fichier

À chaque push, `.github/workflows/check.yml` vérifie que le fichier est un YAML valide avec les clés ci-dessus et que `setup` tient dans la limite de 16 Ko de la plateforme. Il ne démarre pas de VM : cela se fait sur les propres machines de la plateforme.

## Ce qui ne fonctionne pas dans un lab

La VM atteint le dépôt de paquets d’Ubuntu, PyPI, npm et GitHub, et rien d’autre : il n’y a donc rien de l’autre côté de ce lab à attaquer, et pas de seconde machine depuis laquelle attaquer. C’est une seule VM avec un compte supplémentaire.

## À propos

Réalisé par [Mitsuru Technologies](https://mitsurutech.co.uk) pour la [plateforme d’apprentissage MitsuruTech](https://mitsurutech.co.uk/fr/learning) : Python, Java, Go, Node, SQL et Linux dans le navigateur pour les écoles et les établissements, avec des labs de conteneurs, des labs de VM, des applications web déployées depuis GitHub et des quiz qui se corrigent seuls. Essayez du code sans compte dans le [bac à sable](https://mitsurutech.co.uk/fr/playground).

D’autres labs du même éditeur : [lab-netlab](https://github.com/mitsurutech/lab-netlab), [lab-juice-shop](https://github.com/mitsurutech/lab-juice-shop), [lab-dvwa](https://github.com/mitsurutech/lab-dvwa), [lab-webgoat](https://github.com/mitsurutech/lab-webgoat), [lab-mysql-phpmyadmin](https://github.com/mitsurutech/lab-mysql-phpmyadmin) et [lab-sample](https://github.com/mitsurutech/lab-sample).

## Licence

Cette œuvre est sous licence [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). Vous pouvez la partager et l’adapter pour apprendre et enseigner, à condition de citer Mitsuru Technologies Ltd, de ne pas l’utiliser commercialement et de partager tout ce que vous en tirez sous la même licence. Le texte complet est dans le fichier `LICENSE`.

Tout ce que la configuration installe, `cron` et `vim`, provient du dépôt de paquets d’Ubuntu au démarrage de la VM et garde la licence de son propre projet ; rien de tout cela n’est distribué ici.
