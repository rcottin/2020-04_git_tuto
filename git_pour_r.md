---
title: "Introduction à Git avec R"
author: "Raphaël Cottin"
date: "21/04/2021"
output: 
  html_document:
    keep_md: true
    number_section: true
    toc: true
  
---



Tutoriel adapté de [Happy Git with R](https://happygitwithr.com/rstudio-git-github.html) par Jenny Brian (béni soit son nom). 

# Installation et paramétrage

## Installation 

Les étapes pour faire marcher le système:

1. création d'un compte GitHub
2. Installation de Git pour Windows
- paramètres d'installation: 
  + certificat SSD de base
  + autorisation de third party apps
3. paramétrage du compte github
* via Rstudio et le package `usethis` (cf. *infra*)
* via l'invite de commande: 

```
git config --global user.name '@MATRICULE'
git config --global user.email 'MAIL BPI'
git config --global --list
```




## paramétrage de Git dans R


- appel au bash shell dans les options
- installation du package `usethis`
```
install.packages("usethis")
```
- parametrer Git avec son compte github (username et l'adresse mail utilisées pour s'inscrire sur GitHub)

```r
library(usethis)
```

```
## Warning: package 'usethis' was built under R version 4.0.5
```

```r
use_git_config(user.name = "rcottin",
               user.email = "raph.cottin@gmail.com")
```

## Un premier exemple

1. création d'un Repo sur Github

par exemple: 

* nom: `myrepo`
* ajouter une description quelconque
* public
* initialiser avec un fichier README

cloner l'adresse HTTPS avec le gros bouton vert: `https://github.com/rcottin/myrepo.git`

2. cloner le repo sur l'ordi

`pwd` pour voir dans quel dossier on se trouve. Puis: 
```
$ git clone https://github.com/rcottin/myrepo.git

```
(à taper à la main...)

faire de ce repo notre répertoire de travail, puis obtenir infos:
```
$ cd myrepo
$ ls
$ head README.md
$ git remote show origin
```

3. faire un changement local, commit, push

```
$ echo "une ligne écrite sur mon ordi local"  >> README.md
$ git status
```
puis l'ajouter à son Repo. Donner mot de passe et login si demandé:

```
$ git add -A
$ git commit -m "un commit depuis mon ordi local"
$ git push
```

4. confirmer que le changement est bien pris en compte

aller sur la page du repo ([lien](https://github.com/rcottin/myrepo.git))
vérifier que le changement est bien pris en compte


5. effacer le dossier, en local et sur github
```
$ cd..
$ rm -rf myrepo§
```

## Connecter Rstudio à Git et Github

1. créer un repo sur github

(même paramètres que plus haut)

2. Cloner le nouveau repo via Rstudio

* *nouveau projet > version control > Git*
* coller l'url du Repo qu'on vient de créer
  + faire attention à l'emplacement local de sauvegarde
  + ouvrir une nouvelle session

télécharge automatiquement le fichier `README.md`

3. Faire changement locaux, sauvegarder, commit

* ajouter une ligne au fichier `README.md` depuis l'IDE Rstudio
* dans l'onglet *Git*, choisir le README, chocher "staged", cliquer "commit". Ajouter un message de commit. 

4. Push les changement vers Github et confirmer

* toujours dans l'onglet git, cliquer "push"
* aller en ligne sur le repo et vérifier que les changements ont été pris en compte (après rafraîchissement)

5. nettoyer
* effacer le dossier en local
* effacer le dossier sur github


# Workflow

## Nouveau projet, GitHub en premier

1. faire un repo sur Github (initialiser avec README)
1. créer un nouveau projet sur R :
  + *New Project > Version Control > Git*
  + coller l'url du Repo Github
3. faire changement locaux, save, commit: chaque fois qu'une étape importante a été atteinte
4. *Push* les changement vers Github
  -  faire un "pull" *depuis* GitHub en premier!
  
## Projet existant, GitHub en premier

1. créer un repo sur GitHub
1. Nouveau projet Rstudio
1. amener son projet dans dans le dossier créé
1. stage et commit
1. push vers GitHub

## Projet existant, Github en dernier
(pas recommandé au début, plus d'opportunités de plantage. mais parfois nécessaire)

1. lancer le projet RStudio
1. créer un repo Git
  -  *tools > Project Options > Git/SVN*, selectionner Git, confirmer new repository
3. stage & commit
4. créer et connecter un repo GitHub
  - attention : ne PAS initialiser avec README
  - connecter le repo GitHub à Git dans Rstudio : 
      + cliquer sur l'icône "deux carrés violets"
      + "add remote" et coller l'URL. 
      + Choisir un nom: `origin`, puis "add
      + nommer la branche : `master` et **créer branche**, choisir "overwrite"
      


# Utiliser Git via l'invite de commande

voici la liste des principales commandes:

## Navigation

* `pwd` : Print Working Directory (montre dans quel dossier Git se trouve)
* `ls` : list files 
* `cd` : Change Directory. Pour changer le répertoire de travail.  
	+ Attention, CTRL + V ne marche pas dans le shell GIT
	+ mais on peut faire un drag & drop d'un fichier ou dossier dans la fenêtre Git
* navigation: 
	+ aller à sous répertoire `SOUSREP` : `cd SOUSREP`
	+ aller au répertoire parent : `cd ..`
	+ retourner maison : `cd ~`
	
## paramétrer le remote

* `git remote` pour changer lister les remotes (vérifier que l'adresse est correcte)
* `git remote add origin URL` ajoute la remote `URL` avec le nickname `origin`
* `git remote set-url origin URL` change l'adresse de la remote `origin` pour `URL`
* `git remote --verbose` et `git remote show origin` pour checker

## utiliser git

* `git log` voir l'historique des commit (espace pour scroller, q pour sortir)
* `git status` pour donner les infos sur la branche en cours: changement, fichier non trackés, etc. 
* `git add NOM-FICHIER-OU-DOSSIER` : "stage" des fichiers ou dossiers ("ajoute" à git)
	+ ajouter tous les fichiers: `git add -A`
	+ ajouter les fichiers modifiés (pas ajoutés) : `git add -u`
	+ ajouter les nouveaux fichiers: `git add .`
* voir les nouveaux fichiers qui ont été "stagés": `git diff --name-only --cached`
  + version longue: `git diff`
* commit les changements (avec message de commit): `git commit --message "un message"`
* push/pull les changements vers le remote: 
  + `git push`  version courte
  + `git push origin master`. Ici `origin`est le nom du remote et `master` celui de la branche
  + `git pull`
* liste de tous les fichiers trackés par git : `git ls-files` ou  `git ls-tree --full-tree -r --name-only HEAD`


## gitignore

plutôt que d'ajouter à la main tous les fichiers qu'on veut stager, on signale à git ceux qu'on veut ignorer via le fichier .gitignore. 
Recommandé pour les fichiers de données, surtout les données intermédiaires (philosophie *only the source is real*).

Ajout à la main : 

* ajouter le nom du fichier avec extension au gitignore et enregitrer
* ajouter tout un dossier et son contenu: `NOM-DOSSIER/**`
	+ sauf un fichier particulier `!fichier.txt`
* ignorer tous les fichiers de type csv (p.ex): `*.csv`
* ignorer tous les fichiers commençant par "test" : `test*`

Via l'invite de commandes: 

* ajouter FICHIER à gitignore : `echo 'FICHIER' >> .gitignore` (attention au double `>`!)

**attention** attention une fois qu'un fichier est "tracké" par Git,  cela ne servira à rien de l'ajouter au gitignore. Il faut d'abord "unstager" le fichier avec la commande suivante: 
`git rm --cached <file-name>` ou git `rm -r --cached <folder-name>`

## Quand tout va mal
* unstage les fichiers depuis le dernier commit : `git reset HEAD`


## Trucs & astuces

* **PAS D'ACCENTS, ESPACES, CARACTERES SPECIAUX DANS LES NOMS DE FICHIERS!**


# Utilisation de Git

## Conflit de merge: 

* dans un projet existant, modifier le README.md en ligne (sur Github/Gitlab). Commit en ligne
* modifier le fichier README local. 
* stage, commit, pull depuis remote => erreur `merge conflict`
  + vérifier l'origine de l'erreur : 
```
$ git status
```
* réconcilier le README "à la main": 
  + effacer/garder lignes désirées
  + effacer les symboles spéciaux de Git
* stage, commit, pull, pus

NB: si cette configuration arrive en travail collaboratif, la personne en bout de chaîne a le contrôle de quelles modifications garder. 

NB2: d'un autre côté, il est toujours possible de revenir à une version précédente grâce à l'historique. 

## travailler avec les branches

On utilise les branches pour experimenter une idée, corriger un bug, etc... sans modifier le fichier d'origine. 

créer une nouvelle branche sur son repo local et s'y localiser:
```
$ git checkout -b NOM-DE-LA-BRANCHE
```
pousser la nouvelle branche vers GitLab

```
$ git push origin NOM-DE-LA-BRANCHE
```
lister toutes les branches en local: 
```
$ git branch
```
retourner sur la branche principale:
```
$ git checkout master
```

effacer une branche: 
```
$ git branch -d NOM-DE-BRANCHE-A-EFFACER
$ git push origin :NOM-DE-BRANCHE-A-EFFACER
```

fusionner des branches en local: 
```
$ git checkout master         # switcher vers la branche maître
$ git merge NOM-BRANCHE       # intégrer les changement de l'autre branche
$ git branch -d NOM-BRANCHE   # (optionnel) effacer la branche
```

après avoir fusionné les branches:
```
git remote prune origin
```
pour "élaguer" les branches restantes sur le remote
 
## remonter le temps
```
git reflog
```
Montre tout ce qui a été fait, sur toutes les branches. Chaque commit comporte un `HEAD@{index}
```
git reset HEAD@{index}
```
Machine à remonter le temps!



# Ressources en ligne: 

* [Happy Git with R](https://happygitwithr.com/rstudio-git-github.html), superbe guide de Jenny Brian, pour l'intégration de Git/Github/R
* [les slides](https://github.com/uo-ec607/lectures/blob/master/02-git/02-Git.pdf) de mon gars sûr Grant McDermott
* [L'aide en ligne](https://git-scm.com/book/fr/v2/D%C3%A9marrage-rapide-%C3%80-propos-de-la-gestion-de-version) de Git, en français
* ["Oh Shit, Git!?!"](https://ohshitgit.com/) (sérieusement)

