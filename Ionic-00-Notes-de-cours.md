# Ionic

## Introduction
Présentation des applications de type WebView.  
Comparaison avec Cordova.  

Une application web resposera sur Html/css/JS et un binding JS-SDKNatif. Ce sont des vues natives contenant un navigateur web épuré.  

<br>

Apache Cordova a justement vocation à développer ce type d'applications composées d'une webapp compilée par un moteur de rendu et assisté par des plugins permettant d'accéder aux fonctions du smartphone. Communauté très dynamique dans le début des années 2010.  
Capacitor prendra le relai de Cordova tout en resant compatible avec Cordova.

## Installation
Installer node.js : https://nodejs.org/en/download/

``` bash
npm config set proxy http://proxy29.ad.campus-eni.fr:8080
npm config set https-proxy http://proxy29.ad.campus-eni.fr:8080
npm install -g @ionic/cli
npm install -g npm@8.19.1
```

## Présentation de Ionic
Composé de 3 parties principales : 
- CLI
- Ionic Native (tous les modules)
- Composants (appelà un framework js)

A chaque fois ces parties surchargeront Cordova/Capacitor.

Les plus utiles : 
``` bash
ionic start
ionic generate
ionic serve
```
``` bash
ionic doctor check
ionic build
ionic cordova
ionic capacitor
```

Paramétrer proxy ionic :
``` bash
ionic config set -g proxy http://proxy29.ad.campus-eni.fr:8080
```

Création premier projet :
- ionic start
- Ne pas utiliser le wizard
- Nom du projet
- Framework Angular
- Blank projet
