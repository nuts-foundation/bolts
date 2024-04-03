---
description: 'Let op: Deze specificatie is vervangen door een nieuwe versie v2024.1]'
---

# Avond-, nacht- en weekendzorg (ANW) v2022

## 1. Inleiding

Een moer is waardeloos zonder een bout, en evenzo is Nuts waardeloos zonder een Bolt. “Bolt” is onze term voor een concrete toepassing van het Nuts gedachtegoed en open technologie om een tastbare use-case in de zorg mogelijk te maken. De tastbare use-case die hieronder wordt uiteengezet, betreft het veilig verschaffen van de toegang tot de juiste dossierinformatie voor zorgprofessionals in de avond-, nacht- en weekendzorg (ANW), waarbij de ANW-medewerker wel in zijn eigen dossier (ECD) blijft werken. Deze leveranciersspecificatie van de ‘ANW-bolt’ is tot stand gekomen met een viertal samenwerkende softwareleveranciers in het Nuts initiatief. Dit document specificeert eenduidig hoe zorgprofessionals in de ANW-uren toegang kunnen krijgen tot de benodigde dossierinformatie. Ook, of misschien wel vooral, als het een cliënt van een andere organisatie betreft. Het proces wordt uitgewerkt, alsmede de technische componenten en de informatiebehoefte. Bij de informatiebehoefte wordt waar mogelijk aangesloten op de bestaande informatiestandaarden of zorginformatiebouwstenen uit een standaard.

Dit document is primair bedoeld voor softwareleveranciers die willen bijdragen aan het oplossen van deze regionale ANW-problematiek. ANW-zorg wordt overal geleverd en vaak regionaal opgepakt. Dit maakt het (AVG-)vraagstuk omtrent het tijdig toegang krijgen tot de juiste informatie (en alleen die informatie die nodig is) voor elke zorgorganisatie en leverancier relevant. Op basis van deze specificatie kunnen leveranciers de implementatie starten en krijgen zorginstellingen een goed beeld van de problematiek die opgelost wordt.

Mocht dit document vragen bij u oproepen dan houden we ons aanbevolen [om deze te beantwoorden](https://nuts.nl/contact/).

### 1.1. Verklarende woordenlijst

* Nuts — Een samenwerkingsverband van partijen in de zorg om tot een breed gedragen, open, decentrale infrastructuur te komen ten behoeve van de uitwisseling van gegevens in de zorg en het medische domein. Zie [www.nuts.nl](https://nuts.nl).
* Bolt — Een praktische toepassing van het Nuts gedachtegoed en open technologie om een tastbare usecase in de zorg mogelijk te maken.
* ANW – Avond-, nacht- en weekendzorg. Het betreft hier de zorg die buiten kantoortijden geleverd wordt. Het gaat hier dan om zowel geplande als ongeplande zorg.
* Bronhouder — De zorginstelling waar de cliënt in zorg is en de organisatie die verantwoordelijk is voor het dossier van de cliënt. Vanuit het perspectief van de ANW-zorg is dit dus de organisatie waar de cliënt in zorg is en die ANW-zorg nodig heeft.
* Bronsysteem — Het softwaresysteem dat de gegevens van de bronhouder beheert.
* Regisseur – De zorginstelling of entiteit die verantwoordelijk is voor het plannen van de ANW-zorgprofessionals
* Opvragende/bevragende/ontvangende partij — De zorginstelling die de patiënt ontvangt, en dus een informatiebehoefte heeft van gegevens van de bronhouder
* Doelsysteem — Het softwaresysteem dat de gegevens van de opvragende partij beheert

Voor de nadere uitwerking van de bolt, wordt het interoperabiliteitsmodel van Nictiz gehanteerd.

![](https://lh5.googleusercontent.com/vDqh0tKTmaFczY\_K2lsW5YRlirHfaMHYJZ7uLyaOwL5LfbTZMxt3Ha34\_yllz3jySFCHo\_C\_biPJbOcP1sylimZHqpOOYFJO\_cKgiJQEfsMzzg8QKXVernL7Rg5ff0wI1wPFUjP85M1sUkhw\_Q)

## 2. Organisatiebeleid

De use-case zoals hieronder wordt uitgewerkt, betreft de ANW-zorg; een use-case die voor alle zorgorganisaties actueel is maar organisatorisch op verschillende manieren wordt opgelost. Een aantal voorbeelden van hoe ANW-zorg in Nederland opgepakt wordt:

* Een externe partij is verantwoordelijk voor het aannemen van telefoontjes en alarmering in de ANW-uren en zorgt er vervolgens voor dat de ANW-medewerkers in de regio geïnformeerd worden en op tijd bij hun cliënten komen.
* Zorgorganisaties in de regio hebben zelf de handen ineengeslagen om de ANW-zorg te verzorgen. De wijze waarop zij dit doen kan nog variëren: één organisatie is verantwoordelijk voor het aannemen van de telefoontjes of dit werkt met een roulerend schema.
* Een zorgorganisatie levert zelf de ANW-zorg en doet dit niet in samenwerking met andere partijen.

Uit een uitvraag bij een kleine twintig regio’s in Nederland blijkt dat het merendeel van de regio’s de ANW-zorg in samenwerking oppakt (eerste twee voorbeelden) en het niet loont om dit nog individueel te doen. Hoe het organisatorisch ook vormgegeven is, concreet betekent dit dat in het geval van een samenwerking, medewerker X toegang moet krijgen tot dossierinformatie van zorgorganisatie Y. Daar kan bovenop komen dat het inplannen van medewerker X door weer een aparte entiteit wordt gedaan, organisatie Z.

De kern van onderstaande ANW-bolt is dat deze breed toepasbaar moet zijn voor alle organisatorische scenario’s die er in Nederland denkbaar zijn (behalve het ‘individuele’ scenario, daar is toegang tot de dossiers geen issue). Het is een bolt die inzetbaar is, niet alleen voor ANW-zorg maar ook voor allerlei vormen van netwerkzorg: inzet casemanagers dementie, palliatieve netwerkzorg, etc. Voor al dit soort voorbeelden geldt dat medewerkers van organisatie X toegang moeten hebben tot informatie van cliënten van organisatie Y. Voor de herkenbaarheid van de huidige use-case, wordt (vooralsnog) gesproken over de ANW-bolt.

## 3. Zorgproces

Met deze specificatie ondersteunen we het proces van de ANW-zorg. Hierbij wordt een cliënt ingepland op de route van de wijkverpleegkundige en moet deze toegang krijgen tot de actuele gegevens van de cliënt op het juiste moment. Deze zijn nodig om goed zijn/haar taken uit te kunnen voeren. Het kan hier gaan om zowel geplande als ongeplande zorg. Onderstaande proces vindt haar basis in een regionale use-case maar is dusdanig vormgegeven dat deze breder en landelijk toepasbaar is.

We onderscheiden in dit proces de volgende stadia:

1. Een cliënt die zorg nodig heeft in de ANW-uren presenteert zich bij de zorgorganisatie van dienst (regisseur), gepland danwel ongepland
2. De regisseur beoordeelt het verzoek (triage) en zet de zorgvraag door/zoekt een passende zorgprofessional die de benodigde zorg kan leveren
3. De betrokken zorgprofessional krijgt toegang tot de juiste dossierinformatie en kan de zorg leveren en het dossier verrijken met nieuwe informatie

Een belangrijke basis voor het ANW-proces is dat het gaat om leveren van zorg over organisaties heen. Dat wil zeggen dat de zorgmedewerker niet in dienst is/hoeft te zijn van de organisatie waar de cliënt zorg ontvangt. Ook de beoordeling van de zorgvraag of de planning van de zorgmedewerkers kan door een andere organisatie verzorgd worden.

Belangrijke uitgangspunten bij de uitwerking van het proces, zijn:

* De ANW-medewerker blijft in het eigen ECD werken. Dit betekent dat hij/zij de informatie getoond krijgt vanuit het bronsysteem van de cliënt.
* De ANW-medewerker krijgt alleen toegang tot de informatie die noodzakelijk is om in de ANW-uren de benodigde zorg te kunnen leveren.
* Informatie is direct toegankelijk voor de ANW-medewerker. Hij/zij ziet realtime dat er een cliënt in de route is toegevoegd en kan zich ook direct ‘inlezen’ in de vraag en achtergrond van de cliënt.

### 3.1 De cliënt presenteert zich: verzoek tot zorg

De manier waarop een cliënt zich meldt bij een zorgorganisatie in de ANW-uren kent verschillende vormen. De cliënt kan bijvoorbeeld een indicatie hebben voor geplande zorg in de ANW-uren, maar veelal zal er sprake zijn van ongeplande zorg, dat wil zeggen zorg die niet uitgesteld kan worden.

In het geval van geplande zorg, wordt de cliënt ingepland bij een medewerker van het ANW-team. Hoe dit ingepland wordt, is aan de organisatie zelf. Vaak zal het hier gaan om ‘blokken’ van geplande zorg, waarbij tussen deze blokken door ruimte is om ongeplande zorg te leveren.

Als er sprake is van ongeplande zorg, dan is de veelheid aan varianten hoe dit gebeurt een stuk groter. Een regio kan bijvoorbeeld een extern bureau inzetten waar de meldingen binnenkomen of dit met de organisaties in de regio georganiseerd hebben. We noemen dit gemakshalve een regisseursfunctie: los van hoe meldingen binnenkomen, er moet regie gevoerd worden op de wijze van binnenkomst en de vervolgstappen.

De manier waarop meldingen binnenkomen is ook verschillend, dit kan op basis van alarmering door de cliënt zelf of bijvoorbeeld een telefoonsignaal van cliënt, mantelzorger, huisarts of externe meldbank. Een andere optie is dat bijvoorbeeld de huisarts direct een uitvoeringsverzoek opstelt en dit mailt aan de betreffende organisatie/extern bureau.

### 3.2 Beoordelen van het verzoek

De volgende stap in het proces is dat de regisseur (b.v. externe partij of eigen organisatie) de zorgvraag beoordeelt: de regisseur voert een triage uit. De manier waarop de triage wordt uitgevoerd, zal per regio of samenwerkingsverband verschillen. Dit zullen inhoudelijke vragen zijn (meestal in de vorm van een beslisboom) om te achterhalen óf en welke zorg een cliënt nodig heeft.

De uitkomst van deze triage kan als gevolg hebben dat er een interventie nodig is, of niet. Indien een interventie nodig is, kan dit nog door bijvoorbeeld de mantelzorger gebeuren of er wordt, in ernstige gevallen, melding gemaakt richting 112. Een derde vorm van interventie is de inzet van ANW-zorg. Zodra er zorg nodig is door een ANW-medewerker, moet dit signaal terechtkomen bij de juiste ANW-medewerker.

Een logische vervolgstap is dat de regisseur telefonisch contact heeft met de ANW-medewerker van dienst/het ANW-team om te melden dat een cliënt zorg nodig heeft en of dit geleverd kan worden. Een andere manier kan zijn dat de regisseur de cliënt direct inplant op de route van de ANW-medewerker. De ANW-medewerker krijgt hiervan een (push-)bericht en ziet real-time dat er een cliënt is toegevoegd op de route.

De regisseur kan al dan niet al informatie toevoegen aan het dossier van de desbetreffende cliënt.

### 3.3 Verkrijgen van de juiste toegang en dossierinformatie

Zoals bovenstaande beschrijving aangeeft, ligt het startpunt van het ANW-proces bij de regisseur (die tevens triagist kan zijn). De verantwoordelijkheid voor het verzorgen van de juiste toegang tot de meest actuele (dossier)informatie voor de ANW-medewerker ligt bij de regisseur.

Gegeven de driehoek van bronhouder, opvrager en regisseur is het cruciaal dat de toestemmingen juridisch en technisch goed ondervangen zijn (zie hoofdstuk 6). Bij zowel de geplande als ongeplande ANW-zorg moet duidelijk zijn dat de triage, evt. planning en zorglevering geleverd kan worden door een (zorg)professional/entiteit buiten de ‘eigen organisatie’ om (bronhouder).

De regisseur draagt zorg dat de cliënt die zorg nodig heeft op de route terecht komt van de juiste ANW-medewerker. De ANW-medewerker ziet om welke cliënt het gaat en wat de bronhouder is van de informatie van deze cliënt. De ANW-medewerker heeft in zijn eigen dossier een zogenaamde ANW-view (met relevante (dossier)informatie) zodat hij daar de informatie getoond krijgt vanuit het bronsysteem van de bronhouder (daar waar de cliënt in zorg is). Dit gebeurt ‘onderwater’ op basis van de NUTS-principes. De ANW-medewerker kan de cliëntinformatie inzien, de benodigde zorg leveren en ook op onderdelen de informatie weer verrijken (rapportages, metingen, etc.). Deze verrijkte informatie komt vervolgens weer terecht in het bronsysteem van de bronhouder; dit gaat middels het push-principe (post via FHIR resource, zodat voldaan wordt aan de bolt). Na het afsluiten van de dienst, is de cliëntinformatie niet meer toegankelijk voor de ANW-medewerker. Hiervoor wordt in de NUTS autorisatie een geldigheid meegegeven (van bijvoorbeeld 24 uur).

### 3.4 Fasering

Zoals gezegd, ligt het startproces bij de regisseur (veelal triagist). In de meest ideale situatie werkt de regisseur met een informatiesysteem dat ook een NUTS-bolt kent. Van daaruit kan dan de autorisatie opgestart worden, dat wil zeggen dat de regisseur de gewenste autorisatie verstrekt voor de ANW-medewerker van dienst (zie ook paragraaf 6.1). In de praktijk kan het zo zijn dat met een informatiesysteem gewerkt wordt om de triage te verzorgen dat (nog) geen bolt kent (b.v. Salesforce). De regisseur kan dan vanuit één van de voor hem beschikbare ECD’s de autorisatie opstarten. Er zijn dus mogelijkheden om de ANW-bolt gefaseerd in gebruik te nemen waarbij in de ideale situatie alle betrokken informatiesystemen de ANW-bolt ingebouwd hebben.

## 4. Informatie

Voor de ANW-zorg is geïnventariseerd wat de informatiebehoefte is voor de medewerker om goed zijn/haar werk te kunnen doen. Hierbij wordt zoveel mogelijk de aansluiting gezocht bij reeds bestaande zorginformatiebouwstenen ([ZIB’s](https://zibs.nl/wiki/ZIB\_Publicatie\_2020\(NL\))). Voor een aantal onderdelen is nog geen ZIB beschikbaar, hierover zullen nadere afspraken gemaakt moeten worden. Er is een onderscheid tussen informatie die de ANW-medewerker alleen hoeft in te zien (om een goed beeld te vormen van de problematiek) en die de medewerker moet verrijken/invullen. Deze verrijkte of ingevulde informatie moet dus weer teruggeschreven worden in het bronsysteem van de bronhouder.

De informatie die de ANW-medewerker wil inzien, betreft:&#x20;

| Informatie                                                               | Zorginformatiebouwsteen / toelichting                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Informatiestandaard (BGZ of eOverdracht)                                                                                                                                                                          |
| ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Zorginzage (conform Bolt Zorginzage)                                     | <ol><li>Rapportages vervangen door <a href="https://zibs.nl/wiki/SOEPVerslag-v1.0(2020NL)">SOEP verslag</a> (rapportages is nu nog vrije tekst)</li><li><a href="https://zibs.nl/wiki/Patient-v3.2(2020NL)">Patiënt</a></li><li><a href="https://zibs.nl/wiki/Bloeddruk-v3.2.1(2020NL)">Bloeddruk</a></li><li><a href="https://zibs.nl/wiki/LaboratoriumUitslag-v4.6(2020NL)">Laboratoriumuitslag</a></li><li><a href="https://zibs.nl/wiki/Lichaamsgewicht-v3.2(2020NL)">Lichaamsgewicht</a></li><li><a href="https://zibs.nl/wiki/Lichaamslengte-v3.1.1(2020NL)">Lichaamslengte</a></li><li><a href="https://zibs.nl/wiki/Lichaamstemperatuur-v3.1.2(2020NL)">Lichaamstemperatuur</a></li><li><a href="about:blank">O2 saturatie</a></li><li><a href="https://zibs.nl/wiki/Polsfrequentie-v3.3(2020NL)">Polsfrequentie</a></li></ol> | <ol><li>N.v.t.</li><li>BGZ, eOverdracht</li><li>BGZ, eOverdracht</li><li>BGZ, eOverdracht</li><li>BGZ, eOverdracht</li><li>BGZ, eOverdracht</li><li>eOverdracht</li><li>eOverdracht</li><li>eOverdracht</li></ol> |
| Medische voorgeschiedenis/gegevens (diverse ZIB’s uit patiënten context) | <ol><li><a href="https://zibs.nl/wiki/ZorgEpisode-v1.0(2020NL)">Zorgepisode</a></li><li><a href="https://zibs.nl/wiki/Familieanamnese-v3.1(2020NL)">Familieanamnese</a></li><li><a href="https://zibs.nl/wiki/AlcoholGebruik-v3.2(2020NL)">Alcoholgebruik</a></li><li><a href="https://zibs.nl/wiki/DrugsGebruik-v3.3(2020NL)">Drugsgebruik</a></li><li><a href="https://zibs.nl/wiki/Probleem-v4.4(2020NL)">Probleem</a></li></ol>                                                                                                                                                                                                                                                                                                                                                                                                    | <ol><li>N.v.t.</li><li>eOverdracht</li><li>BGZ, eOverdracht</li><li>BGZ, eOverdracht</li><li>BGZ, eOverdracht</li></ol>                                                                                           |
| Medisch beleid                                                           | <ol><li><a href="https://zibs.nl/wiki/AllergieIntolerantie-v3.3(2020NL)">Allergie intolerantie</a></li><li><a href="https://zibs.nl/wiki/Alert-v4.1(2020NL)">Alerts</a></li><li><a href="https://zibs.nl/wiki/BehandelAanwijzing2-v1.0(2020NL)">Behandelaanwijzing</a></li><li><a href="https://zibs.nl/wiki/VrijheidsbeperkendeInterventie-v1.0(2020NL)">Vrijheidsbeperkende interventie</a></li><li><a href="https://zibs.nl/wiki/ZorgAfspraak-v1.0(2020NL)">Zorgafspraak</a></li><li><a href="https://zibs.nl/wiki/Wilsverklaring-v3.1.1(2020NL)">Wilsverklaring</a></li><li><a href="https://zibs.nl/wiki/Vaccinatie-v4.0(2020NL)">Vaccinatie</a></li></ol>                                                                                                                                                                        | <ol><li>BGZ, eOverdracht</li><li>BGZ, eOverdracht</li><li>BGZ, eOverdracht</li><li>Nieuw, nu nog maatregel</li><li>Nieuw</li><li>BGZ, eOverdracht</li><li>BGZ, eOverdracht</li></ol>                              |
| Zorgplan                                                                 | <ol><li><a href="https://zibs.nl/wiki/VerpleegkundigeInterventie-v3.2(2020NL)">Verpleegkundige Interventie</a></li><li><a href="https://zibs.nl/wiki/Behandeldoel-v3.2(2020NL)">Behandeldoel</a></li><li><a href="https://zibs.nl/wiki/MedischHulpmiddel-v3.3.1(2020NL)">Medisch hulpmiddel</a></li><li><a href="https://zibs.nl/wiki/UitkomstVanZorg-v3.2(2020NL)">Uitkomst van zorg</a></li><li><a href="https://zibs.nl/wiki/Verrichting-v5.2(2020NL)">Verrichting</a></li></ol>                                                                                                                                                                                                                                                                                                                                                    | <ol><li>eOverdracht</li><li>eOverdracht</li><li>BGZ, eOverdracht</li><li>eOverdracht</li><li>BGZ, eOverdracht</li></ol>                                                                                           |
| Actuele medicatie                                                        | <ol><li><a href="https://zibs.nl/wiki/MedicatieGebruik2-v1.1.1(2020NL)">Medicatie gebruik</a></li><li><a href="https://zibs.nl/wiki/MedicatieContraIndicatie-v1.0(2020NL)">Medicatie Contraindicatie</a></li><li><a href="https://zibs.nl/wiki/MedicatieToediening2-v1.1.1(2020NL)">Medicatietoediening</a></li><li><a href="https://zibs.nl/wiki/Medicatieafspraak-v1.2(2020NL)">Medicatie afspraak</a></li><li><a href="https://zibs.nl/wiki/Toedieningsafspraak-v1.0.3(2020NL)">Toedieningsafspraak</a></li></ol>                                                                                                                                                                                                                                                                                                                   | <ol><li>BGZ, eOverdracht</li><li></li><li>eOverdracht</li><li>BGZ, eOverdracht</li><li>BGZ, eOverdracht</li></ol>                                                                                                 |
| Contactpersonen                                                          | <ol><li><a href="https://zibs.nl/wiki/Contactpersoon-v3.4(2020NL)">Contactpersoon</a></li></ol>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | <ol><li>BGZ, eOverdracht</li></ol>                                                                                                                                                                                |
| Informatie m.b.t. de toegang tot de woning                               | <ol><li>Veiligheid rondom toegang</li><li>Fysieke toegang (code, sleutelkastje, etc.)</li></ol>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                                                                                                                                                                                                   |

De informatie die de ANW-medewerker wil muteren/terugschrijven, betreft:

| Informatie        | Zorginformatiebouwstaan / toelichting                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Informatiestandaard (BGZ of eOverdracht)                                                                                                                                                                                    |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Metingen          | <ol><li><a href="https://zibs.nl/wiki/Bloeddruk-v3.2.1(2020NL)">Bloeddruk</a></li><li><a href="https://zibs.nl/wiki/Lichaamsgewicht-v3.2(2020NL)">Lichaamsgewicht</a></li><li><a href="https://zibs.nl/wiki/Lichaamslengte-v3.1.1(2020NL)">Lichaamslengte</a> </li><li><a href="https://zibs.nl/wiki/Lichaamstemperatuur-v3.1.2(2020NL)">Lichaamstemperatuur</a></li><li><a href="about:blank">O2 saturatie</a></li><li><a href="https://zibs.nl/wiki/Polsfrequentie-v3.3(2020NL)">Polsfrequentie</a></li><li><a href="https://zibs.nl/wiki/Ademhaling-v3.2(2020NL)">Ademhaling</a></li><li><a href="https://zibs.nl/wiki/Hartfrequentie-v3.4(2020NL)">Hartfrequentie</a></li><li><a href="https://zibs.nl/wiki/Visus-v1.0(2020NL)">Visus</a></li><li><a href="https://zibs.nl/wiki/TekstUitslag-v4.4(2020NL)">Tekstuitslag</a></li></ol> | <ol><li>BGZ, eOverdracht</li><li>BGZ, eOverdracht</li><li>BGZ, eOverdracht</li><li>eOverdracht</li><li>eOverdracht</li><li>eOverdracht</li><li>eOverdracht</li><li>eOverdracht</li><li>N.v.t.</li><li>eOverdracht</li></ol> |
| Rapportages       | <ol><li>Rapportages vervangen door <a href="https://zibs.nl/wiki/SOEPVerslag-v1.0(2020NL)">SOEP verslag</a> (rapportages is nu nog vrije tekst)</li></ol>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | <ol><li>N.v.t.</li></ol>                                                                                                                                                                                                    |
| Actuele medicatie | <ol><li><a href="https://zibs.nl/wiki/MedicatieGebruik2-v1.1.1(2020NL)">Medicatie gebruik</a></li><li><a href="https://zibs.nl/wiki/MedicatieToediening2-v1.1.1(2020NL)">Medicatietoediening</a></li></ol>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | <ol><li>BGZ, eOverdracht</li><li>eOverdracht</li></ol>                                                                                                                                                                      |

## 5. Applicaties

Eerder is het uitgangspunt benoemd dat de ANW-medewerker werkt in de applicatie waarin hij/zij gewend is zijn/haar werk te doen. De medewerker wordt niet geconfronteerd met verschillende logins voor aparte ECD’s. In zijn/haar eigen ECD kan informatie over de desbetreffende cliënt worden ingezien (mogelijk opgehaald uit een ander ECD; notified pull, zie volgende paragraaf) of worden bijgewerkt en teruggestuurd aan het bronsysteem. Hoe de ECD-leveranciers dit om een gebruikersvriendelijke wijze inbouwen, is hun eigen verantwoordelijkheid (dit kan bijvoorbeeld middels een aparte ANW-view binnen het ECD). De manier waarop een medewerker wordt geautoriseerd kent een aantal scenario’s. Deze staan in de volgende paragraaf (6.1). Logischerwijs is het de regisseur die de autorisatie in gang zet.

## 6. IT infrastructuur

In dit hoofdstuk verkennen we hoe toegang tot gegevens wordt geautoriseerd. We nemen hierbij het Nuts manifest als leidraad. Daarnaast hebben we de uitdrukkelijke wens uitgesproken om de uitwerking van deze Bolt technisch zo veel mogelijk compatible te houden met de Bolts Zorginzage en eOverdracht.

### 6.1. Scenarioschets

We gaan er vanuit dat alle zorginstellingen onderaannemer van elkaar zijn. Gegevens opgeslagen bij zorginstelling A (de Custodian) moeten worden ingezien en aangevuld door medewerkers van zorginstelling B (de Actor). De “regisseur” moet door zorginstelling A worden vertrouwd voor het verlenen van autorisaties. Vervolgens kan de “regisseur” autorisaties aanmaken en die delen met zorginstelling B. Deze kan met de verkregen autorisatie in de hand gegevens ophalen bij A.

![](https://lh4.googleusercontent.com/6W8NxGOScGcgKdqEfRY7NLp0l7ANjFv0\_MbVDDNgDQjh\_kr-CYpJreqEL-oPvg-G49vfDx\_-XBHIErMJaN8fkBi35-sWgCSFXw-L9RW58Cm9R\_330vSRL\_cmpU7POBZpua8cCr38cwkwaXSXjQ)

Of in technische termen: De “regisseur” geeft een verifiable credential aan de Actor. De Actor gebruikt deze VC om (via haar eigen Nuts node) een access token te verkrijgen op het systeem van de Custodian. De Nuts node van de Custodian kan de VC cryptografisch valideren en vaststellen dat deze inderdaad door een (vertrouwde) “regisseur” is uitgegeven. Met het verkregen token kan vervolgens dossierinformatie worden opgehaald en worden aangevuld met behulp van FHIR APIs.

![](https://lh5.googleusercontent.com/3ryDYxvl1chUhjPWggY47w9Y78MPLkmdhaYUMaup3eKV7l85zg\_ZyENGLztuQ7JED-mVOInFlcx7PPpkhLvj1cGso7h31JVydZrUNZ2RdpI5Xuc6aBQHlJCDwyHQba1iXhwMOkugJiLDrcREqw)

Autorisaties kunnen door de “regisseur” naar de Actor worden gepusht, waardoor de Actor in staat is om aan de zorgverlener te tonen dat er een nieuwe autorisatie is. Afhankelijk van de inrichting van de leverancier kan er een push notificatie naar de juiste medewerker worden gestuurd en/of de meest recente autorisatie als eerste worden getoond in een applicatie. Daardoor wordt het voor de zorgverlener makkelijker en sneller om de juiste cliënt te vinden en te openen.

Er kunnen vanuit het perspectief van de Actor of de Custodian meerdere “regisseurs” in het spel zijn wanneer een zorginstelling in verschillende regio’s actief is, of wanneer er in een regio sprake is van meerdere partijen die op deze manier regie voeren. Bijvoorbeeld één partij die de geplande ANW zorg organiseert en een andere partij die de ongeplande zorg coördineert.

De bronhouder blijft verantwoordelijk voor de vraag welke “regisseurs” worden vertrouwd en welke organisaties als Actor mogen optreden. Hiertoe houdt de Custodian een “whitelist” bij van welke “regisseurs” autorisaties mogen afgeven voor welke Actors. Om de “regisseur” te ondersteunen kan deze lijst geautomatiseerd worden opgehaald, zodat een medewerker bij de “regisseur” kan zien of een specifieke ANW zorgverlener geautoriseerd kan worden voor een bepaalde cliënt bij een bepaalde Custodian. Door dit vooraf te tonen (en te blokkeren bij een mismatch) kunnen daar geen fouten in gemaakt worden, en ontstaan er geen situaties waarin een zorgverlener gegevens probeert op te halen maar het systeem van de Custodian op basis van haar eigen logica toch geen toegang verleent.

_**Noot**: Decentralized Identifiers (DID’s) vertrouwen per zorginstelling (i.p.v. Nuts-node breed) is nog geen onderdeel van de Nuts node. Is dus werk voor Nuts core team._

In het systeem van de Actor wordt een viewer ontwikkeld, waarin de gebruiker zich identificeert met een cryptografisch middel. De viewer kan vervolgens gegevens tonen uit verschillende bronnen op basis van verkregen autorisaties en VCs. Eventueel kan deze informatie nog worden aangevuld met aanwezige informatie in het eigen systeem waartoe de gebruiker geautoriseerd is. Nieuwe rapportages en meetwaarden kunnen worden aangemaakt en verzonden naar het bronsysteem.

### 6.2. Push/(notified) pull

In het ontwerp van deze Bolt gaan we uit van een notified pull. Dit houdt in dat gegevens niet actief gestuurd worden naar het doelsysteem (push) en dat het doelsysteem niet lukraak gegevens ophaalt (pull). In plaats daarvan stuurt het systeem van de “regisseur” een notificatie naar het doelsysteem dat er toegang wordt verleend tot specifieke gegevens in het bronsysteem. Alleen naar aanleiding van die notificatie haalt het doelsysteem de benodigde gegevens op en wordt de mogelijkheid geboden om gegevens weer te geven/bewerken. De ‘fysieke bewaring’ van de gegevens blijft uitsluitend in het bronsysteem.

Het voordeel van deze aanpak boven push is dat het doelsysteem gegevens alleen dan op hoeft te halen wanneer de ontvangende partij ook daadwerkelijk behoefte aan deze gegevens heeft. Op deze manier kan dus beter aan de eis van dataminimalisatie worden voldaan. Ook is het eenvoudiger om vast te stellen dat de persoon die gegevens ophaalt de juiste persoon is, en om te voldoen aan de NEN7513 en AVG verplichting om te loggen welke persoon de gegevens heeft ingezien. Vergelijk een persoonlijke e-mail inbox waar je inlogt om je e-mail op te halen met een faxmachine op de afdeling waar iedereen die langsloopt bij kan. Het is overigens aan de leverancier(s) om te voldoen aan de NEN7513: gebeurtenissen en acties van ANW-medewerkers dienen gelogd te worden (op gebruikers-ID/-rol). Het doelsysteem moet deze informatie aanleveren t.b.v. de logging; het bronsysteem slaat deze loggingsgegevens op.

Het voordeel van deze aanpak boven enkel een pull mechanisme is timing en (enige) vereenvoudiging van beveiliging. Wanneer het doelsysteem geen notificatie ontvangt moet er periodiek gepulled worden om te zien of er nieuwe gegevens staan te wachten. Analoog aan het constant herladen van een webpagina. Dit veroorzaakt veel onnodig extra netwerkverkeer en vertragingen in het ontvangen van berichten. Ook moet het bronsysteem dan bij elke pull het verzoek naast een complexe rechtenstructuur leggen om te ontdekken of de ontvanger het gevraagde bericht mag ophalen.

Het concept van notified pull is daarom de enige manier om alle gevraagde functionaliteit te ondersteunen, privacy te waarborgen en auditing (beoordeling op rechtmatigheid en betrouwbaarheid) op de juiste manier toe te passen. Het voorkomt onnodig kopiëren van gegevens tussen systemen en de bronhouder behoudt de volledige controle over wie er toegang krijgt tot gegevens.

De notificatie kan door de ontvangende partij gebruikt worden om direct een gebruiker te notificeren of andere processen in gang te zetten.

## 7. Wet- en regelgeving

De in dit kader meest van toepassing zijnde wetteksten zijn:

* de WGBO (wet op de geneeskundige behandelovereenkomst);
* de Wabvpz (wet aanvullende bepalingen verwerking persoonsgegevens in de zorg);
* de AVG (algemene verordening gegevensbescherming);
* de UAVG (uitvoeringswet algemene verordening gegevensbescherming);
* de Begz (besluit elektronische gegevensverwerking door zorgaanbieders);
* en straks mogelijk de Wegiz (wet elektronische gegevensuitwisseling in de zorg).

De WDO is niet relevant voor deze toepassing, aangezien deze het identificeren van patiënten regelt. In het kader van de ANWzorg hebben we alleen te maken met het identificeren van zorgverleners.

In de AVG (Artikel 9 Lid 1 van de AVG en Artikel 22 lid 1 van de UAVG) staat dat het verboden is om bijzondere (medische) persoonsgegevens te verwerken, tenzij er sprake is van een specifieke voorwaarde. In verband met het verstrekken van gezondheidszorg is het toegestaan dat bronhouders en ontvangende partij deze bijzondere persoonsgegevens verwerken voor het daarvoor gestelde doeleind (gezondheidszorg). Deze verwerking is alleen rechtmatig indien voldaan wordt aan 1 van de hieronder genoemde grondslagen in de AVG:

1. toestemming van de betrokkene
2. uitvoeren van een overeenkomst
3. wettelijke verplichting
4. vitaal belang van de betrokkene
5. uitvoeren van een publiekrechtelijke taak
6. gerechtvaardigd belang van de organisatie

Voor de bronhouder geldt de grond van wettelijke verplichting tot het bijhouden van een dossier, dit is in de WGBO opgenomen.

Verder is er sprake van het uitvoeren van een overeenkomst en een onderaannemerschap tussen bronhouder en collega-zorginstelling. Er wordt zorg verleend door een afgesproken derde partij onder verantwoordelijkheid van de bronhouder. Daarom mag deze derde partij ook gegevens verwerken namens de bronhouder. In de overeenkomst tussen partijen is onder meer gewaarborgd dat alle partijen passende technische en organisatorische maatregelen hebben genomen (conform art. 32 van de AVG).

Tenslotte stelt de WGBO (artikel 457 lid 2) dat degene die optreedt als de vervanger van een hulpverlener toegang mag krijgen tot het dossier van een patiënt, voor zover de verstrekking noodzakelijk is voor de door hen in dat kader te verrichten werkzaamheden.

## 8. Beveiliging & vertrouwen

Bij notified pull haalt de ontvangende partij gegevens op bij de bronhouder. De bronhouder, als gegevensbeheerder, moet zich ervan vergewissen dat de opvragende partij inderdaad bij de gegevens mag.

Bij het opvragen is naast de opvragende partij ook een specifieke gebruiker en het doelsysteem betrokken. Het is het doelsysteem dat, technisch gezien, de gegevens ophaalt en toont aan de gebruiker. Het is de gebruiker die daadwerkelijk inzage krijgt in de gegevens. Het doelsysteem haalt gegevens op in het kader van een van de “regisseur” verkregen autorisatie. Om te voorkomen dat ongeoorloofden de gegevens kunnen ophalen en inzien moet het bronsysteem de volgende zaken kunnen controleren:

1. Welk doelsysteem verbinding maakt
2. Dat het doelsysteem als verwerker optreedt voor de opvragende partij
3. Namens welke natuurlijke persoon het doelsysteem verbinding maakt
4. Dat deze persoon de opvragende partij vertegenwoordigt
5. Dat de opvragende partij een autorisatie heeft verkregen van de “regisseur”
6. Dat de autorisatie nog valide is (binnen gestelde tijdvak)
7. Dat de “regisseur” geautoriseerd is om autorisaties aan de opvragende partij uit te mogen geven (maw: maakt de opvragende partij wel deel uit van “onze regio”?)
8. Dat de “regisseur” geautoriseerd is om autorisaties voor deze cliënt uit te mogen geven (maw: ontvangt deze cliënt wel ANW zorg via deze “regisseur”?)

Ervan uitgaande dat er behoefte is aan een zo open mogelijke oplossing, waarbij elke partij evenveel kansen krijgt in de markt en waarbij de beveiliging van een erg hoog niveau is, is het gebruik van cryptografische bewijzen noodzakelijk. Elke van de bovengenoemde controles moet (op termijn) met behulp van een cryptografische handtekening uitgevoerd kunnen worden. Waar dit al mogelijk is zullen we dat vanaf dag één zo inrichten. Waar dat op dit moment nog niet mogelijk is zullen aanvullende afspraken worden gemaakt.

[RFC002 §7](https://nuts-foundation.gitbook.io/drafts/rfc/rfc002-authentication-token#7-supported-means) specificeert welke middelen geaccepteerd moeten worden voor de persoonlijke identificatie. Middelen die noodzakelijk zijn voor de identificatie van organisaties zijn nog in ontwikkeling. Voor de autorisaties die de “regisseur” uit mag geven zal gebruik worden gemaakt van verifiable credentials ([RFC011](https://nuts-foundation.gitbook.io/drafts/rfc/rfc011-verifiable-credential)).

## 9. Open & inclusief

We kiezen in het ontwerp van deze Bolt voor open standaarden, volgens het principe van comply or campaign. Er worden geen verplichtingen gesteld tot het gebruik van bepaalde services of software. Elke partij is vrij om te kiezen of ze gebruik maken van ondersteunende diensten zolang deze diensten geen eisen of restricties opleggen aan andere partijen.

In de basis betekent dit dat er uitgegaan wordt van een gedistribueerde oplossing, waarbij elke partij gelijk is en waarbij zonder tussenkomst van derden, twee partijen rechtstreeks met elkaar gegevens kunnen uitwisselen inzake de ANW zorg.
