---
layout: pages
title: Docker
category: Trainings
---

This Docker training is **only offered in french** for the moment.

[Drop me a mail for a quote](mailto:geoffrey.bachelet@gmail.com).

<!-- {% include see_also_the_docker_101_book.html %} -->

---

## Objectifs

* Comprendre le fonctionnement de Docker
* Comprendre ses avantages et ses inconvénients
* Savoir quand l'utiliser et quand ne pas l'utiliser
* Être autonome avec Docker

## Pre-requis

* Familiarité avec la ligne de commande UNIX
* Laptop avec VirtualBox ou VMWare ou Docker

## Parcours pédagogique

Au cours de la formation, les stagiaires construiront un container ré-utilisable pour un projet de leur choix (ou projet imposé, un seul projet par session), tout d'abord dans un container "tout en un" qui comprendra l'ensemble des services nécessaires au fonctionnement de l'application, puis dans une architecture multi-container où chaque service s'exécute dans son propre container.

## Plan de formation

### Jour 1

* L'infrastructure d'hier à aujourd'hui
  * Historique de la gestion d'une infrastructure
  * Problèmes
* Qu'est-ce que Docker, comment ça marche ?
  * Le concept de container
  * Pourquoi des containers ?
    * Comparaison avec la virtualisation
    * Avantages / Inconvénients
  * Les containers LXC / libcontainer
  * Les différents execution drivers
  * Les différents storage drivers
  * boot2docker
* Premiers pas
  * Création d'un container
  * Commiter un container
  * Re-utilisation d'une image
  * Gestion des containers et images
* Le Dockerfile
  * Introduction au Dockerfile
  * Syntaxe et explication du DSL
  * Dockerfile et contexte de build
  * Surcharge d'image

### Jour 2

* Les volumes
  * Partage de fichiers entre l'hôte et un container
  * Le design pattern Data Container
  * Partage de volumes entre plusieurs containers
* Gestion des logs
  * Avec le syslog de l'hôte
  * Via un partage de volume
  * Autres solutions (Remote API, logspout, ...)
* Pets vs Cattle
  * Présentation du problème
  * Les containers "self-contained" multi-services avec supervisord
  * Les architectures multi-containers
* Le networking
  * Présentation du networking avec Docker
  * Relier des containers entre eux
  * Le design pattern Ambassador
  * Service Discovery
    * Built-in
    * Avec des outils third-party (etcd, consul, ...)
* Orchestration
  * Qu'est-ce que l'orchestration ?
  * Présentation d'outils third-party : Gaudi, Fig, Vagrant, ...
  * Aller plus loin avec Fig

### Jour 3

* La Remote API
  * Présentation de la Remote API
  * Comment utiliser la Remote API
  * Exemples d'implémentation
  * Clients third-party
* Docker Index
  * L'index public
  * Les "trusted builds"
  * Configurer et utiliser un registre privé
* Bonus
  * CoreOS / Project Atomic
  * PaaS maison avec Flynn, Mesos / Marathon
  * Reverse proxying avec nginx ou hipache
  * 0 downtime deployment
