# Ionic

## Introduction
Présentation des applications de type WebView.  
Comparaison avec Cordova.  

Une application web resposera sur Html/css/JS et un binding JS-SDKNatif. Ce sont des vues natives contenant un navigateur web épuré.  

<br>

Apache Cordova a justement vocation à développer ce type d'applications composées d'une webapp compilée par un moteur de rendu et assisté par des plugins permettant d'accéder aux fonctions du smartphone. Communauté très dynamique dans le début des années 2010.  
Capacitor prendra le relai de Cordova tout en resant compatible avec Cordova.  

----
## Installation
Installer node.js : https://nodejs.org/en/download/

``` bash
npm config set proxy http://proxy29.ad.campus-eni.fr:8080
npm config set https-proxy http://proxy29.ad.campus-eni.fr:8080
npm install -g @ionic/cli
npm install -g npm@8.19.1
```
----
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
- Pas nécessaire de créer un compte ionic (fonctionnalités premium)

L'application fonctionnera en single page application.  

Préparer les déploiements sur Mobile.  
```
ionic capacitor add android
ionic capacitor add ios
```

## Angular
### Présentation
----
FW JS MVC fonctionnant sous forme de Single page Application
Basé sur TypeScript, mais devra être transpilé.  
Enjeu de définition sur la granularité des composants.  
Par convention, chaque composant se trouve dans son propre répertoire ou se trouveront les fichiers suivants :
- Controlleur : ts
- Template : html
- Style : scss
- Test Unitaire : spec.ts

L'usage d'annotation @Component permet de rajouter des métadonnées au fichier (*e.g.*  selector)
Le composant sera ensuite inséré en balise dans le html

### Creation de composant
----
```
ionic generate
```
- select : component
- enter : title
- Add component in home.module.ts to NgModule declarations : [**Homepage, COMPONENT-TO-ADD**]

``` javascript
@NgModule({
  imports: [
    CommonModule,
    FormsModule,
    IonicModule,
    HomePageRoutingModule
  ],
  declarations: [HomePage, TitleComponent]
})
```

### Lier html et TS : Interpolation
----
Insérer une variable dans le html {{ hello }}
Lier la variable dans **home.page.ts**
``` javascript
export class HomePage {
  hello: string = 'Hello World !!!';

  constructor() {}
}
```
#### Liaison par événement : Event Binding
``` html
 <button (click)="clickMe()">Cliquez ici</button>
```
``` javascript
export class HomePage {
  hello: string = 'Hello World !!!';

  constructor() {}

  clickMe(){
    this.hello = "Phrase hello modifiée par le binding";
  }
}
```

#### Property Binding
```javascript
//Declare variable
  isHidden: boolean = true;
//use it in function
clickMe(){
    this.hello = "Phrase hello modifiée par le binding";
    this.isHidden = !this.isHidden;
  }
```
``` html
  <div [hidden]="isHidden">Suis-je caché</div>
```

#### Liaison à double sens
```javascript
  name: string = "Anonyme";
```
``` html
<Label>Entrez votre nom</Label>
<input type="text" placeholder="nom" [(ngModel)]="name">
<p>Bonjour {{ name }}</p>
```