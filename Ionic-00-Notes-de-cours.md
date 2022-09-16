# Ionic

## Introduction

Présentation des applications de type WebView.  
Comparaison avec Cordova.

Une application web resposera sur Html/css/JS et un binding JS-SDKNatif. Ce sont des vues natives contenant un navigateur web épuré.

<br>

Apache Cordova a justement vocation à développer ce type d'applications composées d'une webapp compilée par un moteur de rendu et assisté par des plugins permettant d'accéder aux fonctions du smartphone. Communauté très dynamique dans le début des années 2010.  
Capacitor prendra le relai de Cordova tout en resant compatible avec Cordova.

---

## Installation

Installer node.js : https://nodejs.org/en/download/

```bash
npm config set proxy http://proxy29.ad.campus-eni.fr:8080
npm config set https-proxy http://proxy29.ad.campus-eni.fr:8080
npm install -g @ionic/cli
npm install -g npm@8.19.1
```

---

## Présentation de Ionic

Composé de 3 parties principales :

- CLI
- Ionic Native (tous les modules)
- Composants (appelà un framework js)

A chaque fois ces parties surchargeront Cordova/Capacitor.

Les plus utiles :

```bash
ionic start
ionic generate
ionic serve
```

```bash
ionic doctor check
ionic build
ionic cordova
ionic capacitor
```

Paramétrer proxy ionic :

```bash
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

---

FW JS MVC fonctionnant sous forme de Single page Application
Basé sur TypeScript, mais devra être transpilé.  
Enjeu de définition sur la granularité des composants.  
Par convention, chaque composant se trouve dans son propre répertoire ou se trouveront les fichiers suivants :

- Controlleur : ts
- Template : html
- Style : scss
- Test Unitaire : spec.ts

L'usage d'annotation @Component permet de rajouter des métadonnées au fichier (_e.g._ selector).  
Le composant sera ensuite inséré en balise dans le html

### Creation de composant

---

```
ionic generate
```

- select : component
- enter : title
- Add component in home.module.ts to NgModule declarations : [**Homepage, COMPONENT-TO-ADD**]

```javascript
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

---

Insérer une variable dans le html {{ hello }}
Lier la variable dans **home.page.ts**

```javascript
export class HomePage {
  hello: string = "Hello World !!!";

  constructor() {}
}
```

#### Liaison par événement : Event Binding

```html
<button (click)="clickMe()">Cliquez ici</button>
```

```javascript
export class HomePage {
  hello: string = "Hello World !!!";

  constructor() {}

  clickMe() {
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

```html
<div [hidden]="isHidden">Suis-je caché</div>
```

#### Liaison à double sens

```javascript
name: string = "Anonyme";
```

```html
<label>Entrez votre nom</label>
<input type="text" placeholder="nom" [(ngModel)]="name" />
<p>Bonjour {{ name }}</p>
```

#### Conditionnelles et directive structurelles

```javascript
  fruits: string[] = ["pomme", "banane", "kiwi", "pomme", "fraise"];
```

On insère notre js directement dans les balises

```html
<ul>
  <li *ngFor="let fruit of fruits">{{ fruit }}</li>
</ul>
```

On retrouvera ainsi les directives :

- ngFor

```html
<!-- Foreach depusi une liste déclarée côté js -->
<ion-select interface="popover">
  <ion-select-option *ngFor="let niveau of niveaux" value="{{niveau}}"
    >{{niveau}}</ion-select-option
  >
</ion-select>
```

- ngIf

```html
<div *ngIf="condition; else elseBlock">
  Content to render when condition is true.
</div>
<ng-template #elseBlock>Content to render when condition is false.</ng-template>
```

- ngStyle

```html
<p [ngStyle]="{'color' : colorText}">Texte</p>
```

- ngClass : applique une classe css si une condition est remplie

```html
<!-- Ici isHidden est un Boolean -->
<p [ngClass]="{'green': isHidden, 'orange': !isHidden}">Paragraphe</p>
```

#### Utilisation d'une alerte

```javascript
export class HomePage {
//Ajouter l'alertController au constructeur de la page
constructor(
    private alertController: AlertController,
  ) {}
//Fonction de click asynchrone
async button_commencer_click() {
    if (condition) {
      //Créer une alerte grace au controller et l'hydrater
      const alert = await this.alertController.create({
        header: 'Erreur',
        message: this.error_length,
        buttons: ['OK'],
      });
      //Envoyer l'alerte
      await alert.present();
}}}
```

#### Utilisation d'un toast

```javascript
export class HomePage {
//Ajouter le toast au constructeur de la page
constructor(
    private toastController: ToastController
  ) {}
//Fonction de click asynchrone
 async button_reponse_click(
  //La fonction de click doit se voir passer 2 paramètres
  //- la position du toast
  //- la réponse
    position: 'top' | 'middle' | 'bottom',
    reponse: string
  ) {
    const toast = await this.toastController.create({
      //On affichera un message enrichi de la "réponse" issue du bouton
      message: 'Votre réponse est ' + reponse,
      duration: 1500,
      position: position,
    });
    await toast.present();
}}
```

## Services

---

Les services seront mutualisés entre les contolleurs.  
Ils sont concus en singleton.  
Classes TS contenant des méthodes stateless (autonomes, ne dépendant pas de la résolution d'un autre événement).  
Un service diposera d'un décorator

```javascript
@Injectable()
```

Il faut le rajouer aux tableaux des providers en angular vanilla.  
Procédure :

```bash
ionic generate
service
```

Idéalement on regroupera les services dans un répertoire dédié.  
On injecte ensuite une instance du service dans le constructeur

```javascript
constructor(private rngService: RngService){}
```

Les services ne seront **jamais** instanciés directement par les controlleurs.  
Permet d'optimiser la mémoire, Angular gerera les appels et instanciations.

## Programmation asynchrone

Un navigateur ne dispose que d'un seul thread pour tout exécuter.  
Une méthode longue paralysera ainsi les actions du navigateur.  
Pour palier ce problème, la programmation asynchronea été introduite.

### Promise, .then, .catch...

Côté service :

```javascript
getRandomNumber() {
    return Math.floor(Math.random() * 100);
  }

  getRandomNumberPromise(): Promise<number> {
    return new Promise((resolve, reject) => {
      resolve(Math.floor(Math.random() * 100));
      reject(-1);
    })
  }
```

Côté controlleur TS :

```javascript
// Récupération d'un nombre aléatoire du service généré de manière synchrone
  getNumber() {
    this.nb = this.dataSrv.getRandomNumber();
  }

  // Récupération asynchrone d'un nombre aléatoire à travers une promise
  // qui est ensuite traitée avec les mots-clés then et catch.
  getNumberThen() {
    this.dataSrv.getRandomNumberPromise().then((result: number) => {
      this.nb = result;
    }).catch(error => {
      console.log(error);
    }).finally(() => {
      console.log("Fin !");
    });
  }

  // Récupération asynchrone d'un nombre aléatoire à travers une promise
  // grâce aux mots-clés async et await
  async getNumberAsync() {
    try {
      this.nb = await this.dataSrv.getRandomNumberPromise();
    } catch (error) {
      console.log(error);
    }
  }
```

### Async Await

Permettent de simplifier la gestion des promesses.  
Traitement synchrone d'une méthode asynchrone.  
<br>

## Interrogagtion d'API Rest

---

Ajouter à src/app/app.module.ts dans NgModule :

```javascript
import { HttpClientModule } from "@angular/common/http";

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    IonicModule.forRoot(),
    AppRoutingModule,
    HttpClientModule,
  ],
  providers: [{ provide: RouteReuseStrategy, useClass: IonicRouteStrategy }],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

On injectera HttpClient dans notre service

```javascript
 constructor(private http: HttpClient) {}
```

## Observables

---

https://www.learnrxjs.io/

## Navigation

---

Utilisation de App.routing-module.ts  
Il contiendra un tableau de routes.

```bash
ionic generate
page
```

Injecter le service Router dans un controlleur TS pour naviguer entre les pages.

### Transférer des données entre les pages

Dans app routing module
Préciser si elle pourra prendre des variables

```javascript
path: "/about/:name";
```

Avec [routerLink] naviger via le html

Dans le controller pour naviguer par le js :
fonction navigate :

Toutefois, préciser qu'une route prend un paramètre oblige à le passer.  
Si on veut maintenir la route précédente, il faudra la dupliquer.

Service ActivatedRoute dans la page cible permet de récupérer les parametres passés.

```javascript
export class AboutPage implements OnInit {
  constructor(private activatedRoute: ActivatedRoute) { }
  ngOnInit() {
this.activatedRoute.snapshot.params.name

  }
}
```

Peut également etre utilisé de deux manieres
A partir d'un observable
ou directement via .snapshot.params.el

## Genetation d'apk

---

```bash
ionic cap add android
ionic cap add ios
ionic cap sync
ionic cap open
```

Puis build apk et run.

## Sur mac
Il faut installer cocoapods qui ne permettait pas la synchornisation.
```bash
brew install cocoapods
```

## Interraction avec les modules natifs et surcouche ionic native

---

Les plugins cordova sont en js pure. Ionic propose donc uen surcouche ionic native utilisable en Angular/TS.  
On trouvera plusieurs centaines de plugin

- Installer le plugin capacitor
- Importer le module Angular correspondant
- Enregistrer le module
- Injecter le service
  Ici exemple avec le module Preference dans un service injectable

```javascript
import { Injectable } from "@angular/core";
import { Preferences } from "@capacitor/preferences";
import { Infos } from "../Models/infos";

@Injectable({
  providedIn: "root",
})
export class PreferencesService {
  constructor() {}

  setInfos = async (infos: Infos) => {
    await Preferences.set({
      key: "infos",
      value: JSON.stringify(infos),
    });
  };
  getInfos = async () => {
    const { value } = await Preferences.get({ key: "infos" });
    console.log(`Hello ${value}!`);
    return value;
  };
}
```
