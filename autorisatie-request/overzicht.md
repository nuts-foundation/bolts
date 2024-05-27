# Functionele beschrijving autorisatieverzoek

![](../.gitbook/assets/by-sa.png)

_Dit werk valt onder een_ [_Creative Commons Naamsvermelding-GelijkDelen 4.0 Internationaal-licentie_](https://creativecommons.org/licenses/by-sa/4.0/)_._

## 1. Inleiding

Een veel voorkomende vraag bij het uitwerken van een Bolt is of het ook mogelijk is om een patiënt toestemming te laten verlenen op de plek en het moment wanneer dat nodig is. De patiënt zit bijvoorbeeld bij de huisarts en op dat moment is er behoefte om toegang te krijgen tot de gegevens van bijvoorbeeld het ziekenhuis. In het proces moet de patiënt toestemming geven en moet het ziekenhuis een aanvraag tot gegevensdeling beoordelen en, indien er geen bezwaren zijn, inwilligen.

Deze Bolt heeft als doel dat te beschrijven. Dit kan als losstaande use-case gebruikt worden, maar zal meestal onderdeel zijn van een andere Bolt. Deze beschrijving heeft dan ook als doel om als standaard te dienen zodat dit stuk in ieder geval niet voor elke Bolt uitgedacht hoeft te worden. Deze Bolt is alleen bruikbaar voor de gevallen waar expliciete toestemming van de patiënt nodig is.

Dit document is primair bedoeld voor softwareleveranciers die gezamenlijk een Bolt ontwikkelen, en ten behoeve daarvan een implementatietraject gaan starten. Voor hen biedt dit document de noodzakelijke inzichten om tot een succesvolle koppeling te komen. Daarnaast beoogt dit document leesbaar te zijn voor betrokken zorginstellingen, zodat helder wordt of het proces van toestemming vragen zoals dat hier beschreven staat voldoet aan hun wensen.

Dit document is niet volledig. Het is op dit moment een levend document om tussen leveranciers tot werkende afspraken en technische oplossingen te komen. Er zal dus nog het één en ander in gaan veranderen in de komende maanden. Daarnaast is dit document ook vooral een index: op veel plekken zal het refereren naar bestaande standaarden en andere documentatie. Zo houden we dit document beknopt en leesbaar, en kunnen we de documentatie waar we naar verwijzen ook weer hergebruiken voor andere Bolts.

### 1.1 Verklarende woordenlijst

* Nuts — Een samenwerkingsverband van partijen in de zorg om tot een breed gedragen, open, decentrale infrastructuur te komen voor de uitwisseling van gegevens in de zorg en het medische domein. Zie [www.nuts.nl](http://www.nuts.nl).
* Bolt — Een praktische toepassing van het Nuts gedachtegoed en open technologie om een tastbare usecase in de zorg mogelijk te maken.
* Bronhouder — De zorginstelling die gegevens over de patiënt heeft en de aanvraag voor gegevensdeling moet controleren.
* Aanvrager — De zorginstelling die gegevens over de patiënt wil inzien en een autorisatie verzoek indient.
* Grondslag — Een wettelijke basis voor het mogen doorbreken van het beroepsgeheim van de bronhouder.

## 2. Procesbeschrijving

Met deze specificatie ondersteunen we het proces dat een zorgverlener in naam van een zorgorganisatie een andere zorgorganisatie kan verzoeken om gegevens van een bepaalde patiënt te delen. De patiënt heeft hiertoe toestemming gegeven aan de aanvrager. De bronhouder dient deze aanvraag te beoordelen en kan daarna besluiten om over te gaan tot gegevensdeling.

### 2.1 Het geven van toestemming

Een aanvrager kan niet een verzoek indienen zonder daarvoor de goedkeuring van de patiënt te hebben gekregen. De manier waarop een patiënt toestemming kan geven is vastgelegd in de WGBO. De aanvrager zal de wijze van toestemming moeten vastleggen in het dossier van de patiënt. Dit kan bijvoorbeeld een document zijn dat door de patiënt is ondertekend, de welbekende natte handtekening. De vorm is hierbij ondergeschikt aan de inhoud. Het is belangrijk dat de reden voor de aanvraag van toestemming en beschrijving van hoe er toestemming is verleend opgeslagen wordt in het dossier. De aanvrager moet bij het vragen van toestemming ook duidelijk aangeven wat de scope van de toestemming is. Deze moet proportioneel zijn en aansluiten bij een of meerdere specifieke Bolts.

### 2.2 Indienen autorisatieverzoek

Wanneer de zorgverlener de toestemming heeft verkregen is het de taak van het systeem van de aanvrager om deze toestemming om te zetten naar een technisch verzoek aan het systeem van de bronhouder. De [technische specificatie](specification.md) beschrijft hoe de systemen dit moeten implementeren. Een verzoek wordt altijd in de context van één bepaalde Bolt gedaan.

### 2.3 Beoordelen

De bronhouder zal elk autorisatieverzoek moeten beoordelen. Dit kan automatisch gedaan worden o.b.v. bepaalde regels, maar het zou ook kunnen dat een zorgverlener er naar moet kijken. Dit is van veel factoren afhankelijk. Wanneer een Bolt gebruik wil maken van deze specificatie, dan zal het een apart hoofdstuk moeten opnemen over onder welke condities automatisch of handmatig een verzoek goedgekeurd kan worden. Enkele voorbeelden ter illustratie:

* een patiënt is tussen de 12 en 16 jaar, dit vereist toestemming van zowel patiënt als ouder/voogd.
* er is een mentorschap toegewezen door de rechtbank.
* het dossier bevat gegevens die schadelijk kunnen zijn voor derden.
* de patiënt is niet wilsbekwaam voor het geven van toestemming.

Hoe het systeem van de bronhouder omgaat met deze condities is aan de zorgorganisatie en zijn leverancier. Een mogelijke oplossing is dat het systeem van de bronhouder altijd automatisch een verzoek goedkeurt tenzij een zorgverlener dit uitgeschakeld heeft.

### 2.4 Afronden

Nadat het verzoek is goedgekeurd zal het systeem van de bronhouder een autorisatie aanmaken binnen de scope die gevraagd is. De benodigde technische gegevens worden automatisch verstuurd naar de aanvrager via het Nuts netwerk. 

## 3. Intrekken

Het kan natuurlijk voorkomen dat de patiënt zijn toestemming intrekt. Wanneer de patiënt de aanvrager verzoekt om de toestemming in te trekken, zal deze een intrekking versturen over het Nuts netwerk waarin de oorspronkelijke toestemming (verwijzing naar) ingetrokken wordt. Het systeem van de bronhouder wordt op de hoogte gesteld van deze intrekking door het Nuts netwerk. De uitgegeven autorisatie zal dan ook ingetrokken worden door de bronhouder. Omdat de toestemming via een papieren proces is afgehandeld bij de aanvrager, vereist dit wel medewerking van de aanvrager. Er blijft natuurlijk ook altijd de mogelijkheid voor de patiënt om rechtstreeks bij de bronhouder aan te geven gegevens niet meer te willen delen.
