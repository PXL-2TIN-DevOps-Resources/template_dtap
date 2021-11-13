# Assignment 5 Environments (DTAP)

Voor deze opdracht maak je opnieuw gebruik van 2 virtuele machines:
*   **Testserver:** De VM met jenkins uit de vorige lessen
*   **Productionserver:** Een nieuwe Ubuntu VM met NodeJS & npm. 

Het is belangrijk dat beide servers met elkaar kunnen communiceren. Het kan makkelijker zijn als je de productieserver een statisch IP adres geeft. Werk je liever met docker containers? Dat mag van ons ook, maar dan is de configuratie van onderstaande aan jou & wordt het geheel wel complexer om te configureren.

In de repository kan je 2 jenkinsfiles vinden. Je gebruikt beide jenkinsfiles om ze te linken aan elk hun eigen pipeline in je jenkins omgeving. Je voorziet in deze opdracht dus 2 pipelines op de Testserver.

# test.jenkinsfile

Je krijgt reeds een bestaande pipeline met enkele stages in. Voorzie volgende extra stages in deze pipeline:

*   Stage **"Install dependencies**: Gebruik de global tool configuratie van nodejs met als naam "testenvnode". Deze configuratie gebruik je in deze stage om het `npm install` commando uit te voeren.
*   Stage **“Create artifact”**: In deze stage wordt er een artifact gemaakt van de bestaande code van onze NodeJS applicatie.
*   Stage **“deploy test”**: In deze stage zorg je ervoor dat je de bestanden van je artifact uitpakt in de `/opt` folder van je VM. Hierna start je, vanuit de pipeline, de NodeJS applicatie. (#TODO: checken nodejs command in pipeline blijft draaien, runnen op background?) Als je vervolgens naar [http://localhost:3000](http://localhost:3000) surft in de vm, zal je de calculator app kunnen gebruiken.

    _Tip: Het is niet toegelaten om het sudo commando te gebruiken. Heb je geen rechten in bepaalde folders? Dan zorg je ervoor dat de gebruiker ‘jenkins’ de juiste rechten krijgt. Dit is iets wat je handmatig, buiten de pipeline, kan instellen._
    
*   Denk aan de cleanup stappen! De pipeline moet meerdere keren na elkaar kunnen draaien.

![alt_text](https://i.imgur.com/9leib3p.png "image_tooltip") _Heb je aanpassingen gedaan om jenkins rechten te geven tot bepaalde mappen, dan documenteer je dit in oplossing.md onder punt (a)._

# production.jenkinsfile

Je krijg een lege pipeline. Ook deze pipeline integreer je op de jenkins server die op de test server staat. Het doel van deze pipeline is het voorzien van volgende stages:

*   Stage **“deploy prod”**: Zoek een manier waarop je artifact uit de test.jenkinsfile pipeline kan gebruiken om te deployen naar je 2de Virtuele machine (dus via een remote connectie). Ook hier moeten de bestanden opnieuw in de folder `/opt` komen te staan zonder het gebruik van sudo. Vervolgens start je ook hier weer de NodeJS applicatie zodat deze bereikbaar is.
     _Tip: Het gebruiken van een artifact uit een andere pipeline kan op 2 manieren: Je kan kijken waar & op welke manier Jenkins artifacts opslaat op het bestandssysteem of je kan gebruik maken van de plugin `copyartifact`_
     
    _Tip: Denk hierbij aan de commando’s gezien bij onder andere systems advanced. Het kopiëren van bestanden van één server naar een andere kan met het `scp` commando. Maak verder gebruik van de jenkings plugin `sshagent` die werkt met SSH keys als authenticatie_

*   Stage **“test prod”**: Doe een check om te kijken of de applicatie werkt. Voorlopig kan je dit testen met het curl commando om te controleren of je een statuscode 200 krijgt als je het IP adres van de 2de server bezoekt.
*   Denk aan de cleanup stappen! De pipeline moet meerdere keren na elkaar kunnen draaien.

# Configuration management

Voorlopig is er geen onderscheid tussen test en productie. Beide omgevingen gebruiken dezelfde artifact met dezelfde configuratie. Vanuit het concept “build once, deploy anywhere” is het ook belangrijk dat we die zelfde artifact gebruiken voor beide omgevingen. Voor de oefening voorzien we de verschillen in configuratie aan de hand van 2 CSS files:

*   Test.style.css: de body heeft een rode achtergrond
*   Prod.style.css: de body heeft een witte achtergrond

Je maakt deze 2 css files zelf aan. Maak een nieuwe, persoonlijke, publieke repository met als naam <groep>-configuratie-dtap waarin je deze configuraties in opslaat. Belangrijk is dat je voor de opdracht de test en productie pipeline uitbreid met een extra stage **“Config env”** die de juiste css file naar de juiste locatie kopieert. Als je daarna de test- of productieomgeving bezoekt, zien we de verschillende achtergrondkleuren.

Er zijn verschillende manieren waarop je bovenstaande kan configureren. Zie dit stuk gedeeltelijk als een onderzoeksopdracht. Leg hier de focus op het gebruik van een single source of truth voor je configuratie & op automatisatie. 

![alt_text](https://i.imgur.com/9leib3p.png "image_tooltip") _Geef een korte omschrijving van de aanpak in oplossing.md onder punt (b)._

![alt_text](https://i.imgur.com/9leib3p.png "image_tooltip") _Zorg ervoor dat alle **werkende** stappen aanwezig zijn in de Jenkinsfiles van je repository. Indien deze hier niet in staan, krijg je voor deze opdracht ook geen punten._

# Deliverable
Een repository met:
- Een opgevulde test.jenkinsfile met bovenstaande beschreven stages
    - Geen gevulde Jenkinsfile = 0 op deze deelopdracht
    - Een niet werkende (=syntax errors in het pipeline script) Jenkinsfile = -30% op het eindresultaat
- Een opgevulde production.jenkinsfile
    - Geen gevulde Jenkinsfile = 0 op deze deelopdracht
    - Een niet werkende (=syntax errors in het pipeline script) Jenkinsfile = -30% op het eindresultaat
- Een uitbreiding in de test.jenkinsfile met daarin de nodige stappen voor de configuratiemanagement
- Een opgevulde oplossing.md file met antwoorden op bovenstaande vragen inclusief screenshots
