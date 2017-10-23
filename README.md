# Angular Tutorial
Nå skal vi lage en enkel Angular-applikasjon. En forutsetning er at *NodeJS* og *npm* er installert. Hvis ikke kan du se hvordan du gjør det i NodeJS-tutorial’en. 

# Angular Quickstart 
Dette er et starter-kit som gjør det enkelt å komme i gang med Angular. Med dette kan vi opprette et nytt prosjekt og få med alle komponentene vi trenger. Installer starter-kitet slik: 

```
npm install -g @angular/cli 
```

Se for øvrig denne nettsiden for mer utfyllende informasjon: [https://angular.io/guide/quickstart] (https://angular.io/guide/quickstart). Sjekk også *ng-inspector plugin* for Chrome, Firefox og Safari. 
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
