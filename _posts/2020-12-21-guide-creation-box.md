---
layout: default
title: "Création de box - Guide du débutant"
date:   2021-01-05 03:00:00 +0200
published: true
categories:
  - Guide
---

*Cet article est la partie théorique pour la création d'une box boot2root. La suite de cet article qui concerne la partie pratique peut être trouvée [**ici**](https://kr0wz.github.io/fr/guide/2021/01/19/guide-creation-box-pratique.html)*

# Introduction :

Avant de continuer la lecture et potentiellement vous faire perdre votre temps, on va définir ce qu'est une **box** (*si jamais vous cherchiez autre chose genre une **WonderBox** lol*).
Dans le jargon de la sécurité informatique quand on parle de **box** on parle en fait d'une **machine vulnérable** conçue d'une manière à laisser la possibilité à des hackers d'en prendre le contrôle. 

Rassurez-vous, ces machines sont faîtes pour apprendre et donc ça reste dans un **cadre légal**.

J'en profite également pour vous (re)dire qu'il est interdit d'attaquer ou de tenter d'attaquer un système d'information qui ne vous appartient pas ou dont vous n'avez pas l'autorisation explicite et écrite de la personne qui possède ce système.

Pour en revenir à ces fameuses box aussi appelées **boot2root** ou bien **ctf** (*même si ce dernier terme n'est pas vraiment adapté*), il en existe en tout genre, de toutes difficultés, plus ou moins réalistes, de différentes distributions / OS, etc ...

Dernière chose avant d'attaquer le sujet, je tiens à préciser que cet article concerne **UNIQUEMENT** mon **expérience personnelle** ainsi que mes habitudes. Même si je pense que pas mal de personnes fonctionnent à peu près de la même manière. Je vais donc tenter de vous donner des **conseils** et **guidelines** à suivre si vous débutez dans la création de box.

En espérant qu'à la fin de cette article vous aurez les bases pour vous lancer et nous proposer de la nouveauté !

**Bonne lecture !**

* * *

## Ce que nous allons aborder dans cet article :

- [Préparation et réflexion à la création de la box](#préparation-et-réflexion-à-la-création-de-la-box) :
    - [Prise de notes](#prise-de-notes)
    - [Public visé](#public-visé)
    - [Flags](#flags)
    - [Scénario](#scénario)
    
- [Mise en place et configuration technique](#mise-en-place-et-configuration-technique) :
    - [Choisir son OS](#choisir-son-os)
    - [Choix du système de virtualisation](#choix-du-système-de-virtualisation)
    - [Sauvegardes](#sauvegardes)
    - [Configuration de base](#configuration-de-base)
    - [Diviser pour mieux régner](#diviser-pour-mieux-régner)
    - [Configuration globale](#configuration-globale)
    - [Essais globaux](#essais-globaux)
    - [Mise en ligne et diffusion](#mise-en-ligne-et-diffusion)

- [Erreurs à ne pas faire](#erreurs-à-ne-pas-faire)

- [Conclusion](#conclusion)

* * *

## Préparation et réflexion à la création de la box

Vous vous en doutez, avant de commencer à vouloir configurer votre box il faut déjà **réfléchir** à plusieurs choses essentielles. 
Pour une box orientée débutants il se peut qu'y aller directement sans trop réfléchir puisse fonctionner si vraiment vous savez déjà **précisément** ce que vous voulez mettre en place et que ce n'est pas très complexe. <br>

Mais croyez moi, quand il commence à y avoir plusieurs services qui tournent derrière avec des droits spécifiques pour plusieurs utilisateurs et des interactions dans tous les sens il est **important de poser à l'écrit** et de bien tout préparer. Si ça aussi vous servir pour plus tard pour **revenir en arrière** si jamais quelque chose n'est pas cohérent par exemple.

* * *

### Prise de notes 

On peut dire que c'est une **première esquisse** de ce qui va se rapprocher de votre box finale.
Ici l'idée n'est pas forcément de tout savoir ce qu'on va mettre en place mais plutôt de **noter un peu toutes sortes d'idées** qui nous viennent en tête. 
Ça peut être des vecteurs d'attaque, des escalations de privilèges, une idée d'un thème ou d'une histoire à développer et à intégrer dans la box, un scénario, etc ...

Vraiment noter le **plus de choses possibles** même les plus futiles. Cette étape est importante pour avoir le plus de matière possible pour ensuite manipuler et faire le tri sur ce qu'on veut garder ou pas lors de la suite pour la configuration de la box.

Imaginons que vous ayez plusieurs escalations de privilèges. Dans ce cas, le fait de les avoir noté va vous permettre de choisir celle qui **correspond le mieux** en fonction des autres choix.


Le fait de choisir quelque chose à mettre en place mais de ne pas savoir comment est également un **très bon exercice** pour apprendre de nouvelles techniques, technologies. Mettre les mains dans le cambouis a toujours été très formateur surtout en sécurité informatique.<br>

Alors si par exemple vous voulez mettre en place un **buffer overflow** mais que vous ne savez pas comment faire, allez vous renseigner sur comment on **exploit ces vulnérabilités**, comment elles fonctionnent.

Une fois que vous avez réuni suffisament d'idées vous pouvez passer à l'étape suivante qui consiste à **choisir le public** visé pour votre box.

Bien évidemment il est possible de revenir à cette étape si jamais on s'apperçoit que quelque chose n'est aps cohérente oub ein si d'autres idées nous parviennent.

**Très important** également : prenez des notes même pendant la phase de configuration et d'installation des services sur la machine. Cela va vous permettre de garder une trace de ce que vous avez fait si jamais un problème suirvient ou si plus tard vous êtes ammenés à reproduire quelque chose d'équivalent.

* * *

### Public visé

Cette étape va nous servir à définir le public qui va hacker la box. 
Il en va de soit qu'on ne va pas mettre des techniques de hacking ultra poussées si la box est destinée à des débutants et inversement.

On ne peut pas vraiment définir un tableau avec la difficulté en fonction des vulnérabilités mais on peut déjà définir un peu le type d'exploitations que l'on trouve en fonction de la difficulté.
Evidemment la difficulté est propre est chacun en fonction de son niveau et de son expérience dans le domaine. Donc là je vais parler en fonction de moi en sachant que j'ai pas un très bon niveau.

On pourrait donc donner un ordre d'idée qui correspondrait à ça :

- **Débutant** : web, code source, fuzzing, brute force, GTFOBins, très CTF like
- **Intermédiaire** : réflexion plus poussée pour faire les liens entre chaque étape, services plus "exotiques", custom scripts
- **Difficile** : souvent réaliste, custom exploits, souvent bas niveau / reverse / pwn (bref complexe), services créés sur mesure

Encore une fois c'est très grossier comme classement et évidemment qu'il peut y avoir n'importe quelle technique dans n'importe quelle difficulté mais c'est un peu pour donner une **base commune** et avoir une idée du type de vecteurs d'attaques de chaque catégorie.

La difficulté va également influer le temps de résolution de la box. Une box facile sera forcément **plus rapide** à résoudre qu'une box qui demande d'écrire un exploit à la main pour exploiter une vulnérabilité plutôt complexe.

On peut aller piocher et commencer à **faire le tri** au niveau des idées que nous avons eu au début pour qu'elles correspondent à la difficulté.

* * *

### Flags

Ce qu'on appelle **flag** ici correspond à une **chaîne de caractères** présente sur la machine qui permet de **prouver** qu'on a bien réussi un certain nombre d'étapes.<br>

Un flag peut être un **hash** aléatoire ou bien sous une forme spécifique comme : *my_flag{This_is_a_flag}*.

Il n'y a pas vraiment de règle concernant le format de ceux-ci. On doit seulement pouvoir les reconnaitre. S'ils ont un format spécial on pourra le préciser au départ par exemple.

La plupart du temps il n'y a que **2 flags** sur la machine. Un pour l'utilisateur et un pour le root.

Cependant, si vous voulez que l'utilisateur **cherche en profondeur** dans la box, vous pouvez toujours insérer des flags à des endroits spécifiques. Ça peut être des fichiers de configurations importants, le code source d'une page, etc ...


* * *

### Scénario

Quand je parle de scénario je parle du **chemin à suivre** pour "résoudre" la machine et la compromettre.

Plus précisément les **étapes nécessaires** à l'avancement du hacker dans la machine pour arriver à sa compromission totale. 
Ces étapes vont de l'énumération de base de la machine, en passant par l'exploitation pour avoir un premier shell, l'escalation de privilèges pour obtenir le root ou bien rajouter une étape intermédiaire pour passer sur un autre utilisateur qui lui aura peut-être les droits nécessaires, etc ...

On peut donner **quelques exemples** concernant ce qu'on veut que le hacker fasse et/ou ne fasse pas durant le hacking de notre box.
Si c'est une machine pour débutants, qu'on veut qu'il brute force quelque chose et qu'on veut laisser un indice alors il peut être judicieux de créer un répertoire caché à la racine du site (ou bien dans le code source de l'index par exemple) qui lui donnerait le nom d'une liste de mots de passe à utiliser ou bien carrément le fichier adéquat. Evidemment ce n'est qu'un exemple mais il est important de bien **définir au préalable où on veut diriger le hacker**. Ça peut aussi être un choix de ne rien donner à l'utilisateur et c'est à lui d'énumérer absolument tout se qui se situe devant lui pour trouver l'entrée ou des choses intéressantes.

Si on veut à l'inverse éviter que l'utilisateur brute force par exemple le ssh alors on va pouvoir mettre en place un **système pour éviter ça** (fail2ban pour ne citer que lui).

Il est également possible de faire une **box à thème**. Si par exemple vous êtes fan de la franchise Fallout alors vous pouvez mettre des clins d'oeil tout au long de la progression dans la machine. Ça peut aller au site web qui reprend un thème spécifique avec les couleurs, le texte, jusqu'aux utilisateurs dans la machine qui portent des noms particuliers. La seule limite est votre **imagination**.

Pour vous donner un exemple de machine à thème vous pouvez aller voir la machine [**Biohazard**](https://tryhackme.com/room/biohazard) sur TryHackMe. Il s'agit d'une box sur le thème de la franchise **Resident Evil**. La plupart de l'histoire se déroule via le web avec de petites énigmes qui vont nous permettre d'avancer et au fur et à mesure pour arriver à obtenir un shell.

On peut orienter le hacker vers de **fausses pistes** appelées (rabbit holes), ce qui va le pousser à revenir en arrière et continuer l'énumération pour rechercher une autre vulnérabilit potentielle. On pourrait imaginer plusieurs façons pour l'utilisateur d'arriver à un même point. Par exemple en intégrant plusieurs vulnérabilités dans la machine qui permettraient un choix au niveau de l'escalation de privilèges.

Le plus important reste d'avoir une **suite logique des étapes**. Encore une fois ce que je dis et très générique et bien sûr qu'il peut y avoir des exceptions partout. Vous pouvez très bien faire en sorte qu'une fois que l'utilisateur obtient un shell sur la machine ça déclenche une action qui va ouvrir un nouveau port sur la machine et donc le hacker va de nouveau devoir repartir à la phase énumération pour tenter de poursuivre son ascension.


Une fois que vous avez en tête une idée précise du chemin à emprunter, les vulnérabilités à mettre en place et le public visé vous pouvez commencer à vous diriger vers la **partie pratique** et **configuration** de la machine vulnérable.


* * *

## Mise en place et configuration technique

Dans cette partie on va voir de manière assez brève les **bonnes pratiques** pour mener à bien la **création de la box**.
Vous allez voir que la première chose ici correspond au choix du système d'exploitation ainsi que de la distribution que vous allez utiliser.

Encore une fois, il n'est **pas nécessaire de suivre l'ordre** dans lequel j'ai indiqué les étapes. Vous pouvez tout à fait choisir le système d'exploitation et ensuite réfléchir aux vulnérabilités associées à celui-ci. 

* * *

### Choisir son OS

Dans cette sous-partie pas grand chose à dire, si ce n'est que vous pouvez choisir votre système d'exploitation en fonction des vulnérabilités trouvées dans la partie "prise de notes" ou bien inversement et partir de l'OS pour rechercher les vulnérabilités liées.

A savoir aussi que le **choix de l'OS n'a pas vraiment d'impact pour certains services**. Par exemple avec le web, dans tous les cas vous pourrez mettre en place un site avec PHP, javascript ou même du Wordpress.

Le choix va surtout être déterminant pour **l'escalation de privilèges** ou bien si vous voulez mettre certains services qui ne sont disponibles que sur une plateforme en particulier.
Un buffer overflow ne va pas fonctionner de la même façon sur un Windows ou un Linux par exemple.

* * *

### Choix du système de virtualisation

Encore une fois ici c'est vraiment en fonction des **préférences** de chacun.

Il est vivement conseillé d'utiliser un **système de virtualisation** pour créer une machine vulnérable. **Pourquoi** ? Comme on le verra dans la partie suivante, les sauvegardes se font très facilement et rapidement, à la fin vous pourrez également exporter votre machine dans un format qui permettra de le partager de manière très aisée sur Internet.

Vous pouvez utiliser **VMWare** ou bien **VirtualBox** qui sont les plus répandus. Je vous conseillerais cependant de partir plutôt sur du **VirtualBox** qui est selon moi beaucoup plus facile de configuration pour débuter *(surtout si vous ne disposez pas de la version payante de VMWare)*. Je n'ai jamais eu de problèmes de réseau sur VirtualBox comparé à VMWare sur lequel j'ai jamais réussi à faire tourner une box provenant de VMWare suite à des problèmes de réseau qui faisaient qu'aucune adresse IP n'était attribuée à la machine.

Tout ça pour dire que le type de software utilisé pour la virtualisation de votre box n'est pas vraiment important, choisissez en fonction de vos habitudes (et de celui qui fonctionne le mieux pour vous).


* * *


### Sauvegardes

C'est l'une des **étapes les plus importantes** et vous me remercierez plus tard lorsque vous aurez fait une **backup** avant de casser votre VM.

Avant chaque grosse modification sur votre VM je vous conseille de faire une sauvegarde de votre machine en lui donnant un nom spécifique pour vous retrouver. En effet, si vous venez à devoir repartir d'une base fonctionnelle lors d'une erreur de configuration par exemple il est important de s'y **retrouver rapidement**.

Pensez donc à faire **régulièrement** des backups :)


* * *


### Configuration de base

Ce que j'appelle configuration de base c'est l'installation du **système d'exploitation** et des **services essentiels** au bon fonctionnement de votre machine (comme par exemple SSH).
Il s'agit donc de faire une **installation propre basique** (par exemple en prennant Debian), configurer l'utilisateur principal ainsi que le root. Au début vous pouvez donner des mots de passe simples pour vos utilisateurs, ce sera **plus facile** lors des configurations pour passer d'un utilisateur de la machine à un autre sans devoir vous taper des mots de passe illisibles toutes les 10 minutes. Attention cependant à la fin de votre configuration de **tout modifier** pour éviter de potentiels brute force *(sauf si encore une fois ça fait partie de votre scénario d'attaque)*.

Je vous recommande également d'installer un **serveur SSH** pour configurer **plus facilement** votre VM sans devoir être obligatoirement dessus via le shell de base (c'est toujours plus agréable). Même principe qu'avant, vous pouvez ensuite **supprimer le service** selon vos besoins.

Et pour reprendre ce qui a été dit avant, une fois votre installation de base effectuée pensez à **SAUVEGARDER**. Ça vous évitera d'attendre 30 minutes pour que votre OS se réinstalle en cas de problème.

* * *

### Diviser pour mieux régner

C'est un principe que j'applique **tout le temps** lorsque je crée des box et c'est **très souvent utilisé en informatique** en général (et notamment en **programmation**).

Pour vous donner une définition assez simple et pas du tout formelle, on pourrait dire qu'un **très gros problème est très compliqué à résoudre**. Parce qu'on ne sait pas forcément par où commencer, ce qu'il faut faire, il y a des liens logiques entre plusieurs parties de ce problème, etc ...

La **solution** à adopter est donc de **diviser** ce gros et très difficile problème en **plusieurs petits problèmes** beaucoup plus **simples** à résoudre.

On peut donner un exemple très simple et qui n'est pas en rapport avec l'informatique : construire une maison.
C'est un problème d'envergure parce que dit comme ça, ça parait **long et fastidieux** (et c'est le cas). Mais maintenant disons qu'on va **découper cette tâche** en plein de tâches plus petites / courtes et plus faciles à résoudre. La première tâche serait de construire les fondations, puis de poser les mûrs, etc ... Jusqu'à arriver finalement au **problème de base** qui était de construire une maison.

Ici c'est la même chose avec notre box. Le problème principal est : configurer une box. Mais c'est quand même très large. On va donc utiliser ce principe pour découper en **plus petits problèmes**.

Disons qu'on veut mettre en place un site web sur notre machine vulnérable. On peut encore découper ce problème en tâches plus petites : installer apache, puis installer PHP, puis créer les pages web à intégrer, puis configurer apache, potentiellement les logs, etc ... A la fin on a une **liste de petites choses** à faire qui n'ont pas l'air très compliquées mais qui, misent bout à bout, vont nous permettre de **reconstituer notre problème principal** et de le **résoudre**.

Maintenant que la grosse explication est passée, **concrètement** qu'est ce qu'il faut faire dans notre cas ?

Ce que j'ai l'habitude de faire c'est de faire **parties par parties**. Elle ne sont pas forcément à faire dans l'ordre. Par exemple je peux commencer par configurer les droits sur la machine pour préparer l'élevation de privilèges. En même temps que je fais cette configuration je **prends des notes** pour savoir ce que j'ai fait et donc pour pouvoir le refaire de manière rapide et propre à la fin.

Une fois que ça fonctionne je peux en faire une **sauvegarde** pour y avoir accès plus tard.

Il est également important de **faire des tests** à chaque fois que vous avez configuré une petite partie. En effet, ce serait dommage d'appliquer la méthode de diviser pour régner pour qu'au final aucun test ne soit effectué et que le **tout ne fonctionne pas**.


* * *

### Configuration globale

Cette partie permet de **réunir** toutes les petites parties qui ont été configurées pour n'en faire qu'un **gros bloc** dans une VM et qui correspondra à la machine finale. 

Si lors de cette étape vous vous rendez compte qu'il y a un **conflit** au niveau des idées de base, par exemple que telle vulnérabilités ne correspond pas à l'idée de la box alors vous pouvez toujours **revenir sur vos pas** pour modifier votre configuration.

Une fois que vous avez configuré chaque portion de ce que vous vouliez faire *(et qu'elle fonctionne)* il suffit de **tout reprendre et de tout assembler**. Encore une fois ici plusieurs stratégies sont possibles. Il peut être judicieux de reprendre la machine de base pour y **insérer** chaque portion. Mais on peut aussi reprendre n'importe quelle sauvegarde et y insérer les autres parties unes à unes.

C'est en fonction de vos **préférences** et au fur et à mesures que vous prendrez de **l'expérience** vous aurez vos propres habitudes.

A partir de cet instant n'oubliez pas de **modifier tous les mots de passe** des comptes et services sur votre machine que vous aviez configuré au préalable pour vous faciliter la tâche.

* * *

### Essais globaux

Il s'agit de nouveau d'une **étape importante** et je dirais même plus **primordiale** pour ne pas sortir une machine vulnérable **pleine de bugs** et impossible à utiliser.

Normalement ici vous avez plein de petites parties fonctionnelles mais il se peut que toutes ces parties rassemblées ne fonctionnent plus *(et malheureusement c'est probable que ça arrive)*.
Si tout fonctionne comme prévu c'est parfait et donc bravo à vous, vous avez réalisé votre première machine vulnérable ! Sinon il va falloir **revenir en arrière**.

Si jamais toutes vos parties fonctionnent de **manière séparées** mais qu'il y a un problème lorsque vous les regroupez, n'hésitez pas à le **faire par étapes**. Par exemple la partie 1 avec la partie 2, puis ajouter la partie 3. Si là ça ne fonctionne pas alors tenter d'ajouter la partie 2 avec la 3 et ainsi de suite pour définir d'où vient le problème.

C'est souvent le cas au niveau des **droits**. Si un service configuré d'un côté doit aller chercher des fichiers générés par un autre service qui lui aussi à été configuré de son côté alors il se peut que le premier n'ai pas les droits de lecture dessus et donc ça va poser problème. Ce sont souvent ce genre de petits problèmes qui apparaissent lors de la configuration de la machine.


* * *

### Mise en ligne et diffusion

Vous avez désormais une **machine vulnérable fonctionnelle** et prête à se faire hacker. Il ne vous reste plus qu'à la **partager** au monde entier.

Pour se faire il faut d'abord **exporter** votre VM dans un format qui pourra se **partager facilement** sur Internet. Pour VirtualBox on utilise les **fichiers .ova**. Il s'agit d'un simple fichier qui pèse en général 2 voir 3 GB et qui permet à n'importe qui d'importer votre machine dans son propre VirtualBox pour ensuite tenter de l'attaquer.

Vous pouvez la partager via **plein de moyens différents**, que ce soit via Google Drive, Mega.nz, Vulnhub, TryHackMe, etc ...

En sachant qu'évidemment le moyen de diffusion va permettre à plus ou moins de personnes d'y accéder. Il est normal qu'un upload sur Google Drive **apportera moins de personnes** qui feront votre box (puisqu'il faut que vous partagiez le lien) contrairement à la mise en ligne sur TryHackMe qui compte plusieurs centaines de milliers d'utilisateurs.


## Erreurs à ne pas faire

On va terminer cet article sur les **erreurs à ne pas faire** et qui risquent de **nuire à l'expérience** de celui qui tentera de faire votre box.
Pour ces erreurs je me base évidemment sur les problèmes auxquels j'ai **déjà été confrontés** lors des différentes box que j'ai faites d'autres personnes qui en avait créé.

La première chose est ce qu'on appelle le **guessing**. C'est le fait de devoir deviner quelque chose. Par exemple un mot de passe. Le but d'une box c'est soit d'avoir du **fun** en la faisant soit d'apprendre des choses et de **gagner en compétence**. Quand on arrive devant un mot de passe à deviner, qu'on a aucune idée de ce que ça peut être, qu'il n'y a aucun indice et qu'aucune wordlist ne peut cracker le mot de passe, eh bien ... ça fait pas avancer le schmilblic. Il y a plusieurs techniques et façons de faire pour **palier à ce problème**. Soit on donne un **indice** à l'utilisateur via un jeu de piste, soit on lui donne par exemple directement la wordlist, soit on le fait chercher ailleurs quelque chose qui lui permettrait de **contourner ce problème**.

L'autre chose qui peut être mal utilisée est le **brute force**. Pourquoi je dis ça ? En soit le brute force est une **technique comme une autre** qui est plutôt réaliste quand on voit la complexité désastreuse des mots de passe des utilisateurs dans la vraie vie. Cependant lorsqu'on attaque une machine vulnérable le but c'est **pas d'attendre 5 jours** que le brute force en utilisant rockyou se termine.
Donc oui le brute force peut faire partie d'un vecteur d'attaque dans une box mais il faut faire en sorte que l'utilisateur ne **perde pas trop de temps dessus**. Ensuire une fois pour contourner ce problème on peut par exemple mettre le mot de passe dans un texte puis faire utiliser l'outil ``CeWL`` pour générer une wordlist personnalisée, on peut également choisir un mot de passe dans une grosse wordlist mais qui n'est pas très éloignée. Par exemple prendre le 5000ième mot dans rockyou est largement suffisant pour faire comprendre à l'utilisateur comment se servir d'un outil de brute force *(pas la peine de mettre un mot placé à la ligne 9 412 014*).

Ce qui peut être embêtant dans une machine ne sont les **problèmes de droits**. Ils ont été un peu évoqués précédemment et si vous avez suivi ce petit guide. Normalement avec les **différents tests effectués** tout au long de la configuration de votre box ça ne devrait pas arriver. J'ai déjà vu dans une box le ``.bash_history`` d'un utilisateur qui n'était pas vide et qui était accessible à tout le monde. Du coup on pouvait voir les commandes entrées par le créateur de la box et du coup ça cassait un peu l'expérience.

Il se peut aussi que certains droits sur un fichier aient **été mal configurés** et du coup on peut effectuer des actions qui n'ont pas été pensées pour être faites et qui nous amène à **passer à côté du vrai chemin** qui a été pensé par le créateur.

Evidemment rien ne nous empêche de quand même faire la machine comme elle a été pensée mais ça ne renvoie pas forcément une bonne image de la machine (même si l'erreur est humaine).

D'où l'**importance de faire des tests** à chaque fois qu'on configure quelque chose sur la machine.


La dernière chose qui me vient en tête est une **machine virtuelle trop lourde**. Lorsque vous installez et configurez vos services, essayez de ne pas perdre de la place inutilement. Une machine légère peut faire moins de 1GB alors que les plus grosses peuvent aller jusqu'à 4GB **grand max**. Une petite pensée à ceux qui ont de petites connexions.

Pour **gagner de la place** vous pouvez par exemple dès l'installation de base configurer les partitions pour faire en sorte de ne prendre que l'espace nécessaire. Vous pouvez aussi **désinstaller** les outils vous vous avez utilisés sur la machine et qui ne sont pas indispensables à la résolution de celle-ci.

* * * 

## Conclusion

Il y a encore des **tonnes de choses** à dire sur la création d'une box boot2root mais je pense que le principal a été dit ici pour que vous ayez une **base plutôt solide** pour la création de celle-ci.

Vous pourrez retrouver très prochainement un **autre article** sur mon blog qui fera l'objet d'un **cas pratique de A à Z** pour la création et la mise en ligne d'une machine vulnérable.

Comme d'habitude n'hésitez pas à me **donner vos retours** sur [**Twitter**](https://twitter.com/ZworKrowZ) ou en rejoignant mon [**serveur Discord**](https://discord.gg/v6rhJay).

J'espère que vous aurez appris des choses et je vous dis bonne création !