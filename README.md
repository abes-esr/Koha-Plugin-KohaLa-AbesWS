# Plugin KohaLa Abes WS - Version 2021

**Abes WS** est un plugin Koha qui permet d'exploiter depuis Koha des services
web de l'ABES. L'intégration à Koha de services web de l'ABES vise deux
objectifs distincts et complémentaires :

- **Contrôles rétrospectif** — Des listes d'anomalies de catalogage sont
  affichées. À partir de ces listes, des opérations de correction peuvent être
  lancées.

- **Enrichissement de l'affichage** — L'affichage des notices dans Koha est
  enrichies de données récupérées en temps réel à l'Abes.

Ce plugin a été conçu et développé lors d'un atelier _Services web de
l'[Abes](https://abes.fr)_ qui s'est tenu lors du Hackathon 2021 de
l'association [KohaLa](http://koha-fr.org) des utilisateurs français de Koha.

![Abes](https://raw.githubusercontent.com/fredericd/Koha-Plugin-KohaLa-AbesWS/master/Koha/Plugin/KohaLa/AbesWS/img/logo-abes.svg)
![KohaLa](https://raw.githubusercontent.com/fredericd/Koha-Plugin-KohaLa-AbesWS/master/Koha/Plugin/KohaLa/AbesWS/img/logo-kohala.png)

## Installation

**Activation des plugins** — Si ce n'est pas déjà fait, dans Koha, activez les
plugins. Demandez à votre prestataire Koha de le faire, ou bien vérifiez les
points suivants :

- Dans `koha-conf.xml`, activez les plugins.
- Dans le fichier de configuration d'Apache, définissez l'alias `/plugins`.
  Faites en sorte que le répertoire pointé ait les droits nécessaires.

**📁 TÉLÉCHARGEMENT** — Récupérez sur le site [Tamil](https://www.tamil.fr)
l'archive de l'Extension **[KohaLa Abes
WS](https://www.tamil.fr/download/koha-plugin-kohala-abesws-1.0.2.kpz)**.

Dans l'interface pro de Koha, allez dans `Outils > Outils de Plugins`. Cliquez
sur Télécharger un plugin. Choisissez l'archive **téléchargée** à l'étape
précédente. Cliquez sur Télécharger.

## Utilisation du plugin

### Configuration

Dans les Outils de plugins, vous voyez l'Extension *KohaLa Abes WS*. Cliquez sur
Actions > Configurer.

Quatre sections pilotent le fonctionnement du plugin :

- **Accès aux WS** — Paramètres d'accès aux services web. Il n'est pas
  nécessaire de modifier les paramètres par défaut.

- **Établissement** — L'ILN et les RCR de l'ILN. Les services web ne seront
  interrogés que pour cet ILN et ces RCR. Pour les RCR, il faut entrer la liste
  de ses RCR, suivi pour chacun du nom en clair de la bibliothèque. Par exemple :

  ```text
  341722102 BIU Montpellier - Droit, Science po, Eco et Gestion
  341725201 ABES - Centre de doc
  ```

  permettra d'interroger les infos relatives à deux RCR, le RCR 341722102
  correspond à la _BIU Montpellier - Droit, Science po, Eco et Gestion_ et le RCR 341725201 pour _ABES - Centre de doc_.

- **bibliocontrol** — Le service web _bibliocontrol_ renvoie les anomalies de
  catalogage d'un ou de plusieurs RCR choisis dans la liste définie dans la
  section _Etablissement_. Ces anomalies sont, pour le moment, au nombre de trois :

  - Présence d'une zone 225 esseulée
  - Code fonction 000 en 700, 701 ou 702
  - Présence simultanée d'une zone 181 et d'une sous-zone 200$b

  On choisit ici les anomalies à afficher.

- **Page détail** — Dans la page détail d'une notice bibliographique affichée
  dans l'interface pro de Koha, on peut récupérer et afficher des informations
  complémentaires obtenues au moyen des services web de l'ABES. Pour le moment, on dispose des options suivantes :

  - **Activer** — pour activer l'affichage d'infos provenant du Sudoc sur la
    page de détail des notices biblio
  - **Localisation** — pour afficher les localisations de la notice dans les
    établissements déployés dans le Sudoc.
  - **Sélecteur PPN** — Sélecteur jQuery permettant de retrouver le PPN dans la
    page de détail. C'est la feuille de style XSL de la page de détail de
    l'interface pro qui affiche et rend accessible le PPN. Par exemple,
    `#ppn_value`.

### Bibliocontrol

La page **bibliocontrol** lance l'appel au service web bibliocontrol de l'ABES,
puis affiche le résultat dans un tableau. On choisit au préalable le RCR dont on
veut contrôler les notices. Le tableau contient deux colonnes permettant
d'identifier les notices : PPN et Titre.

La colonne Titre contient le titre de la notice si le plugin peut le retrouver
dans Koha à partir du PPN. Pour que cela fonctionne, il faut avoir établi un
lien dans `Administration > Liens Koha => MARC` entre le champ Unimarc contenant
le PPN et le champ MySQL `biblioitems.lccn`.

La colonne PPN contient une icône permettant de copier d'un clic le PPN dans le
presse-papier. De là, on peut passer dans _WinIBW_ pour retrouver une notice et
la modifier.

### AlgoLiens

**AlgoLiens** est un service web de l'ABES qui, pour un ou plusieurs RCR,
identifie les notices présentant des zones pour lesquelles il manque les
sous-zones de liens. C'est par exemple une zone comme celle-ci :

```text
702  1 $a Arbus $b Sanson $4 610
```

qui n'a pas de sous-zone `$3` établissant un lien avec une autorité Auteur.

Sur la page de démarrage, on sélectionne le/les RCR ainsi que les types de
notice que l'on souhaite contrôler. On distingue les notices bibliographiques
des notices d'autorité. Pour chaque notice, on peut choisir des types de
document ou des types d'autorité.

Un tableau présente le résultat obtenu au moyen de l'appel du service web
AlgoLiens.

### Page de détail

Si on a activé l'affichage d'infos Sudoc sur la page de détail des notices
bibliographiques, le service web _multiwhere_ de l'ABES est appelé pour chaque
notice qui dispose d'un PPN. Un onglet **Sudoc** est ajouté au tableau des
exemplaires qui est affiché sous la notice bibliographique. Dans cet onglet, les
localisation de la notice dans les établissements Sudoc sont affichées. Chaque
établissement est un lien vers la page Sudoc du RCR : nom de établissement,
adresse, téléphone, etc.

## VERSIONS

* **1.0.3** / avril 2021 — Version initiale

## LICENCE

This software is copyright (c) 2021 by Tamil s.a.r.l..

This is free software; you can redistribute it and/or modify it under the same
terms as the Perl 5 programming language system itself.

