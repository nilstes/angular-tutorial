# Angular Tutorial
Nå skal vi lage en enkel Angular-applikasjon. En forutsetning er at *NodeJS* og *npm* er installert. Hvis ikke kan du se hvordan du gjør det i NodeJS-tutorial’en. 

# Angular Quickstart 
Dette er et starter-kit som gjør det enkelt å komme i gang med Angular. Med dette kan vi opprette et nytt prosjekt og få med alle komponentene vi trenger. Installer starter-kitet slik: 

```
npm install -g @angular/cli 
```

Se for øvrig denne nettsiden for mer utfyllende informasjon: https://angular.io/guide/quickstart. Sjekk også *ng-inspector plugin* for Chrome, Firefox og Safari. 
Nå kan vi opprette et nytt prosjekt. Følgende kommando vil opprette en ny mappe my-app med innhold:

```
ng new my-app cd my-app 
```

Nå har vi fått opprettet et prosjekt  med en del innhold:
* **node_modules**: nedlastede js-biblioteker
* **package.json**: npm-pakkedefinisjonen for prosjektet. Inneholder alle avhengigheter som Angular Quickstart har tatt med.
* **src**: template kildekode for default-applikasjonen. Disse skal vi endre mer på senere.
* **e2e**: mappe for tester 

La oss bygge prosjektet og starte serveren: 

```
ng serve --open 
```

Dette åpner også opp en nettleser på http://localhost:4200. La oss endre litt kode. Åpne `src/app/app.component.ts` i en teksteditor. 

Finn følgende kode: 

```javascript
export class AppComponent {
  title = 'app'; 
}
```

... og erstatt innholdet av app med f.eks navnet ditt. Nettsiden skal nå hilse deg personlig uten at du trenger å bygge på nytt… 

# Komponenter
Nå kan vi lage en ny komponent: 

```
$ ng generate component MyList
  create src/app/my-list/my-list.component.html (26 bytes)
  create src/app/my-list/my-list.component.spec.ts (629 bytes) 
  create src/app/my-list/my-list.component.ts (272 bytes)
  create src/app/my-list/my-list.component.css (0 bytes) 
  update src/app/app.module.ts (398 bytes) 
```

Vi legger inn html-kode i html-fila og css i css-fila. 

html:
```html
<table>
  <thead>
     <tr><th>Navn</th><th>Adresse</th><th>Alder</th></tr>
  </thead>
  <tbody>
    <tr *ngFor="let person of persons">
      <td>{{person.navn}}</td>
      <td>{{person.adresse}}</td>
      <td>{{person.alder}}</td>
    </tr>
  </tbody>
</table>
```

css:
```css
table {
    font-size:16px;
    font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
    border-collapse: collapse;
    border-spacing: 0;
    width: 100%;
}

td, th {
    border: 1px solid #ddd;
    text-align: left;
    padding: 8px;
}

tr:nth-child(even){background-color: #f2f2f2}

th {
    padding-top: 11px;
    padding-bottom: 11px;
    background-color: #4CAF50;
    color: white;
}
```

Vi venter litt med ts-fila til vi har laget oss en service for å hente data. Så endrer vi html-fila i `app.component.html` slik at hovedkomponenten også inkluderer vår nye listekomponent:

```html
<div style="text-align:center">
  <h1>
    Welcome to {{title}}!
  </h1>
  <app-my-list></app-my-list>
</div>
```

# Service

Nå kan vi lage oss en Service for å hente data. Vi kan generere en tom Service-klasse på denne måten:

```
ng generate service Person
```

La oss endre `person.service.ts` som ble generert for oss:

```javascript
import { Injectable } from '@angular/core';
import { Person } from './person';

@Injectable()
export class PersonService {

    constructor() {}
 
    getPersons(): Person[] {
        return [{navn:"Nils", adresse: "Skogen", alder: 25}];
    }
}
```

Da må vi også lage en Person-klasse med samme format, `person.ts`:

```javascript
export class Person {
  navn: string;
  adresse: string;
  alder: number;
}
```

Når vi så skal bruke servicen i my-list kan vi injisere denne inn ved help av «dependency injection». Da må vi legge til en provider i `app.module.ts`:

```javascript
providers: [PersonService],
```

I `my-list.component.ts` gjør vi følgende:

```javascript
export class MyListComponent {

    persons=[];
  
    constructor(private personService: PersonService) {
        this.persons=personService.getPersons();
    }
}
```

Her ser vi at konstruktøren tar imot en instans av *PersonService*. Nå vil Angular bruke «dependency injection» og injisere en instans av servicen for oss.

# Kalle en annen REST-server

For å hente data fra en database kan vi for eksempel bruke Node-serveren vi lagde tidligere. Da må vi endre Node-serveren slik at den tillater kall fra andre servere:

```javascript
    res.setHeader('Access-Control-Allow-Origin', '*');
```

Så kan vi legge til en ny metode i `person.service.ts`:

```javascript
    getPersons2(): Promise<Person[]> {
        console.log("Calling REST service"); 
        return fetch("http://localhost:8080").then((response) => {
            console.log(response); 
            if(!response.ok) {
                throw response.statusText;
            }
            console.log(response); 
            return response.json();
        });
    }
```

Da må vi også endre måten vi kaller servicen på siden den nå ikke returnerer umiddelbart lengre:

```javascript
    constructor(private personService: PersonService) {
        personService.getPersons2().then((result)=>{
            this.persons=result;
        }).catch((reason)=>{
            this.persons=[];
        });
    }
```

# Router

Nå inkluderer vi liste-komponenten inn i `app.component`, men vi kan la denne være dynamisk slik at vi enkelt kan endre skjermbilde ved å endre url’en. Bytt ut `app-my-list` med `router-outlet` i `app.component.html`:

```html
<router-outlet></router-outlet>
```

Da må vi også endre `app.module.ts`:

```javascript
  imports: [
    BrowserModule,
    RouterModule.forRoot([
      {
        path: 'list',
        component: MyListComponent
      },
      {
        path: '',
        component: AppComponent
      }
    ])
  ],
```
