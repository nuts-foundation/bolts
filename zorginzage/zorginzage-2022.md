# 🔩 Zorginzage 2022

![](../.gitbook/assets/by-sa.png)

_Dit werk valt onder een_ [_Creative Commons Naamsvermelding-GelijkDelen 4.0 Internationaal-licentie_](https://creativecommons.org/licenses/by-sa/4.0/)_._

## 1. Inleiding

Een moer is waardeloos zonder een bout, en evenzo is Nuts waardeloos zonder een Bolt. “Bolt” is onze term voor een concrete toepassing van het Nuts gedachtengoed en open technologie om een tastbare use-case in de zorg mogelijk te maken.

Voor u ligt de leveranciersspecificatie van de Bolt Zorginzage, zoals opgesteld door de samenwerkende softwareleveranciers in de Nuts community. Dit document specificeert eenduidig hoe het inzien van dossiergegevens van een betrokkene door een zorgverlener van de ene zorgaanbieder in het dossier van een andere zorgaanbieder procesmatig en technisch zal plaatsvinden, met gebruikmaking van open standaarden.

Dit document is primair bedoeld voor softwareleveranciers die een implementatietraject gaan starten van een zorgtoepassing die gebruik maakt van de Bolt Zorginzage. Voor hen biedt dit document in combinatie met het relevante zorgtoepassingprofiel de noodzakelijke inzichten. Daarnaast beoogt dit document leesbaar te zijn voor betrokken zorgorganisaties, zodat helder wordt of het proces zoals dat hier beschreven staat, voldoet aan hun wensen.

Dit document is niet volledig. Het is op dit moment een levend document om tussen leveranciers tot werkende afspraken en technische oplossingen te komen. Er zal dus nog het één en ander in gaan veranderen in de komende maanden. Daarnaast is dit document ook een index: op veel plekken zal het refereren naar bestaande standaarden en andere documentatie. Zo houden we dit document beknopt en leesbaar, en kunnen we de documentatie waar we naar verwijzen ook weer hergebruiken voor andere Bolts.

Wij wensen u veel leesplezier, en mocht dit document vragen bij u oproepen dan houden we ons aanbevolen[ om die te beantwoorden](https://nuts.nl/contact/).

### 1.1 Zorgtoepassingen

De Bolt Zorginzage is generiek van aard en biedt ondersteuning voor meerdere zorgtoepassingen. Voorbeelden van ondersteunde zorgtoepassingen zijn geboortezorg en huisartsinzage in thuiszorgdossier. Toepassing van de generieke Bolt Zorginzage op een specifieke zorgtoepassing vindt plaats aan de hand van een zorgtoepassingprofiel. Het zorgtoepassingprofiel wordt apart gedocumenteerd en beschrijft o.a. de governance, de informatiestandaarden, de toegestane authenticatiemiddelen, de toegestane grondslagen en de access policy van de zorgtoepassing.

### 1.2 Verklarende woordenlijst

Voor de keuze van de diverse rollen hebben we zoveel mogelijk gebruik gemaakt van de DIZRA [https://dizra.gitbook.io/dizra/begrippenlijst](https://dizra.gitbook.io/dizra/begrippenlijst)

* Afnemer: Een afnemer is een rol van een zorgaanbieder die data afneemt van een bronhouder. De afnemer heeft een informatiebehoefte van gegevens van de bronhouder.
* Afnemersysteem: Een geheel van softwaresystemen waarmee een zorgverlener gegevens kan opvragen. Dit betreft zowel gegevens die zijn geregistreerd binnen dezelfde zorgaanbieder als gegevens van andere bronhouders.
* Authenticatiemiddel: Cryptografisch sterk authenticatiemiddel. Een authenticatiemiddel waarmee authenticatiecontracten kunnen worden aangemaakt die voldoen aan [RFC002](https://nuts-foundation.gitbook.io/drafts/rfc/rfc002-authentication-token). Voorbeelden hiervan zijn IRMA en de UZI-pas.
* Autorisatie: Het gecontroleerd verlenen van toegang tot medische gegevens door en bronhouder. Hierbij wordt onderscheid gemaakt tussen het aanmaken en delen van autorisatie-records enerzijds en het controleren op autorisatie-records anderzijds.
* Autorisatie-record: Een machineleesbaar recht waarin 1 bronhouder in het kader van 1 zorgtoepassing aan 1 afnemer toegang geeft tot een afgesproken scope van het dossier van 1 betrokkene. Per zorgtoepassing kunnen specifieke aanvullende afspraken over het autorisatie-record in het zorgtoepassingprofiel worden beschreven.
* Betrokkene: De patiënt of cliënt wiens dossier wordt ingezien.
* Bolt: Een concrete toepassing van het Nuts gedachtengoed en open technologie om een tastbare use-case in de zorg mogelijk te maken.
* Bronhouder: De zorgaanbieder waar een dossier wordt bijgehouden over de betrokkene. Binnen de Bolt Zorginzage is dit het dossier dat een zorgverlener van een andere zorgaanbieder gaat inzien.
* Bronsysteem: Een geheel van applicaties dat de gegevens van de bronhouder beheert.
* Gebruiker: Zorgverlener of andere medewerker die vanuit de context van een afnemer dossiergegevens over een betrokkene raadpleegt bij een of meerdere bronhouders.
* Grondslag: Een wettelijke basis voor het mogen doorbreken van het beroepsgeheim van de bronhouder.
* Nuts: Een samenwerkingsverband van partijen in de zorg om tot een breed gedragen, open, decentraal vertrouwensnetwerk te komen ten behoeve van de uitwisseling van gegevens in de zorg en andere hieraan gerelateerde domeinen. Zie[ www.nuts.nl](http://www.nuts.nl).
* Zorgaanbieder: Zorgorganisatie
* Zorgtoepassing: Een specifiek zorgproces waarin gegevensbeschikbaarheid een rol speelt. Voorbeelden hiervan zijn geboortezorg, huisartsinzage in thuiszorgdossier en actueel medicatieoverzicht.
* Zorgtoepassingprofiel: Beschrijving van afspraken die gelden voor een specifieke zorgtoepassing. Deze afspraken zijn aanvullend aan de inhoud van de Bolt Zorginzage.
* Zorgverlener: Individuele zorgverlener

## 2. Procesbeschrijving

Met deze bolt ondersteunen we het proces van Zorginzage. Dat proces bestaat enerzijds uit het interoperabel, toegankelijk, vindbaar en herbruikbaar (FAIR) maken van dossiergegevens door een bronhouder ('publiceren') en anderzijds uit het daadwerkelijk inzien van dossiergegevens door een afnemer ('raadplegen'). Door Zorginzage zijn zorgverleners in staat het voor hen relevante deel van het zorgtraject van de betrokkene te volgen dat zich bij andere zorgaanbieders afspeelt.

Een concreet voorbeeld hiervan is een thuiszorgdossier dat wordt bijgehouden door de thuiszorgorganisatie en wordt ingezien door de huisarts. Observaties en metingen worden verricht door verplegend en verzorgend personeel van de thuiszorgorganisatie en deze kunnen specifiek en gericht worden ingezien door de betrokken huisarts.

Een ander concreet voorbeeld hiervan is integrale geboortezorg waarbij verschillende zorgaanbieders (eerstelijns verloskundepraktijken, ziekenhuizen, echoscopiepraktijken, kraamzorgorganisaties, jeugdgezondheidszorgorganisaties) in onderlinge samenwerking de juiste zorg voor de betrokkene en diens omgeving leveren. Alle betrokken zorgverleners dienen (wanneer dat voor hen relevant is en wanneer daarvoor een grondslag is) inzage te hebben in de observaties, metingen en andere zorggegevens die zijn geregistreerd door personeel van andere betrokken zorgaanbieders.

Een ander concreet voorbeeld is het opvragen van de Basisgegevensset Zorg (BgZ) binnen de medisch-specialistische zorg . Zorgverleners dienen (wanneer dat voor hen relevant is en wanneer daarvoor een grondslag is) inzage te hebben in de Basisgegevensset Zorg die is geregistreerd door een eerdere behandelaar.

Een ander concreet voorbeeld is een actueel medicatieoverzicht (AMO). Zorgverleners dienen (wanneer dat voor hen relevant is en wanneer daarvoor een grondslag is) inzage te hebben in de medicatie-informatie die is ingevoerd door andere zorgaanbieders.

### 2.1 Publiceren

Voordat dossiergegevens kunnen worden ingezien, moeten deze vindbaar, toegankelijk interoperabel en herbruikbaar (FAIR) worden gemaakt. Dit wordt publiceren genoemd.

* Dossiergegevens zijn interoperabel wanneer deze conform een afgesproken informatiestandaard zijn gestructureerd (bijvoorbeeld in de vorm van FHIR-resources).
* Dossiergegevens zijn toegankelijk wanneer
  * deze conform een afgesproken informatiestandaard benaderbaar zijn (bijvoorbeeld in de vorm van een RESTful FHIR-API)
  * authenticatie en autorisatie conform een afgesproken standaard plaatsvinden
* Dossiergegevens zijn vindbaar wanneer deze door een afnemer kunnen worden gelokaliseerd. Lokalisatie betreft het beantwoorden van de volgende vraag van een afnemer: Bij welke bronhouders is data over deze betrokkene te vinden?

#### 2.1.1 Informatiestandaard

Het interoperabel en het gestandaardiseerd benaderbaar maken van dossiergegevens valt buiten de scope van deze Bolt en is expliciet onderdeel van informatiestandaarden (bijvoorbeeld de Informatiestandaard Geboortezorg PWD 3.2). In de Bolt Zorginzage wordt ervan uitgegaan dat dossiergegevens via een door een gezamenlijke informatiestandaard gespecificeerde interface (bijv. een RESTful FHIR-API) toegankelijk zijn. Voor het correct functioneren van een implementatie van de Bolt Zorginzage is het noodzakelijk dat het beheer van de gehanteerde informatiestandaard adequaat is ingericht.

#### 2.1.2 Autoriseren

Gestandaardiseerde autorisatie van dossiergegevens is een essentieel onderdeel van deze Bolt. Hierbij is het allereerst nodig onderscheid te maken tussen autorisatie-records enerzijds en grondslagen anderzijds. In de context van deze Bolt is een autorisatie-record een machineleesbaar recht van een zorgaanbieder voor het inzien van een bepaalde scope aan gegevens van een betrokkene. Een autorisatie-record is altijd gebaseerd op een grondslag voor de verwerking van persoonsgegevens. Een bewijs voor een bepaalde grondslag hoeft niet per se machineleesbaar te zijn. In het geval van de grondslag 'toestemming' kan het bewijs bijvoorbeeld bestaan uit een ingescande handtekening of een aangevinkt selectievakje. Er kan echter ook sprake zijn van een mondeling gegeven toestemming. Voor elke zorgtoepassing die gebruik maakt van de Bolt Zorginzage wordt in het zorgtoepassingprofiel beschreven welke grondslagen onder welke voorwaarden kunnen worden gebruikt.

In deze Bolt wordt primair gebruik gemaakt van autorisatie-records die zijn gebaseerd op specifieke toestemmingen. Deze toestemmingen kunnen expliciet of verondersteld zijn.

* Een _expliciete specifieke toestemming vooraf_ is een door de betrokkene gegeven toestemming aan exact één bronhouder voor het beschikbaar stellen van een afgesproken scope van de dossiergegevens van die betrokkene aan exact één afnemer.
* Een _veronderstelde toestemming_ is een toestemming aan exact één bronhouder voor het beschikbaar stellen van de dossiergegevens van die betrokkene aan exact één afnemer, die voortkomt uit een door de betrokkene geaccordeerde verwijzing van bronhouder naar afnemer. In het geval van een verwijzing is altijd bekend welke zorgaanbieders de bronhouder en de afnemer zijn, waardoor een veronderstelde toestemming altijd specifiek van aard is.

De Bolt Zorginzage kan daarnaast ook ondersteuning bieden voor autorisatie-records op basis van een uitdrukkelijke toestemming voor het vooraf beschikbaar stellen met meerdere niet op voorhand bekende zorgaanbieders (via een elektronisch uitwisselingssysteem) conform de Wabvpz (vanaf hier: '_Wabvpz-toestemming vooraf_'). Dit is een door de betrokkene gegeven toestemming aan één specifieke bronhouder of aan alle bronhouders binnen een bepaalde categorie met een behandelovereenkomst met betrokkene, voor het beschikbaar stellen van een afgesproken scope van dossiergegevens van die betrokkene aan een categorie van afnemers met een behandelovereenkomst met betrokkene. Een voorbeeld van een _Wabvpz-toestemming vooraf_ is gedefinieerd door [Mitz](https://www.programma-otv.nl/documentatie/). Wanneer een _Wabvpz-toestemming vooraf_ wordt voorzien van de juiste cryptografische waarborgen kan deze door een bronhouder, die de Nuts Bolt Zorginzage heeft geïmplementeerd, worden gebruikt voor het autoriseren tijdens het proces raadplegen.

**Expliciete specifieke toestemming vooraf**

In het geval van een expliciete specifieke toestemming vooraf wordt als onderdeel van het proces publiceren een autorisatie-record via de volgende stappen gecommuniceerd van bronhouder naar afnemer:

1. Betrokkene communiceert mondeling of schriftelijk een expliciete toestemming aan een zorgverlener van de bronhouder.
2. Zorgverlener legt de expliciete toestemming van betrokkene vast in het bronsysteem.
3. Het bronsysteem communiceert deze expliciete toestemming in de vorm van één of meerdere specifieke autorisatie-records (1 autorisatie-record per beoogd afnemer) aan de Nuts-node van de bronhouder.
4. De Nuts-node van de bronhouder slaat ieder autorisatie-record op en communiceert dit vervolgens naar de Nuts-node van de afnemer die is opgenomen in dat autorisatie-record. Het autorisatie-record is altijd specifiek voor een bepaalde afnemer, een bepaalde zorgtoepassing en een bepaalde scope. Per zorgtoepassing kunnen specifieke aanvullende afspraken over het autorisatie-records in het zorgtoepassingprofiel worden beschreven.
5. De Nuts-node van de afnemer slaat het ontvangen autorisatie-record op.

Een belangrijk onderdeel van autorisatie is het toetsen op de aanwezigheid van een behandelovereenkomst tussen de afnemer en de betrokkene en de aanwezigheid van een behandelrelatie tussen de gebruiker en de betrokkene. Dit is onderdeel van het proces Raadplegen.

**Veronderstelde toestemming**

In het geval van een verwijzing wordt als onderdeel van het proces publiceren een autorisatie-record via de volgende stappen gecommuniceerd van bronhouder naar afnemer :

1. Betrokkene heeft in een eerder stadium toestemming gegeven voor een verwijzing van bronhouder naar afnemer. Door het geven van toestemming voor een verwijzing, kan toestemming voor het delen van gegevens impliciet verondersteld worden. Dit vereist wel dat betrokkene en zorgverlener goed zijn geïnformeerd over de beoogde verwijzing en gegevensdeling (informed consent).
2. Een medewerker van bronhouder kan optioneel (een bewijs van) de verwijzing registreren in het bronsysteem.
3. Het bronsysteem communiceert de veronderstelde toestemming die voortkomt uit de verwijzing aan de Nuts-node van de bronhouder, in de vorm van een autorisatie-record.
4. De Nuts-node van de bronhouder slaat het autorisatie-record op en communiceert dit vervolgens naar de Nuts-node van de afnemer die is opgenomen in dat autorisatie-record.
5. De Nuts-node van de afnemer slaat het ontvangen autorisatie-record op.

**Wabvpz-toestemming vooraf**

In het geval van een Wabvpz-toestemming vooraf wordt geen autorisatie-record gecommuniceerd van bronhouder naar afnemer.

#### 2.1.3 Lokaliseren

Binnen de context van de Nuts Bolt Zorginzage worden dossiergegevens globaal op twee verschillende manieren gelokaliseerd:

1. Primair: Dossiergegevens zijn vindbaar op basis van specifieke autorisatie-records aanwezig in de Nuts-node van de afnemer. Dit wordt in meer detail beschreven in paragraaf 2.1.2.
2. Secundair: Dossiergegevens zijn vindbaar via informatie in een derde applicatie, bijvoorbeeld Mitz of een door de betrokkene zelf beheerde applicatie.

De Nuts-node van iedere afnemer kan op basis van de opgeslagen autorisatie-records eenvoudig data lokaliseren waarvoor de afnemer is geautoriseerd. Hiermee is de data vindbaar gemaakt. In meer detail verloopt het lokaliseren van data conform de volgende stappen:

1. Autorisatie-records met daarin informatie over de betrokkene, de bronhouder, de afnemer de zorgtoepassing en de scope worden gesynchroniseerd tussen bronhouder en afnemer
2. Afnemer vraagt actieve autorisatie-records op bij de eigen Nuts-node. De identifiers van bronhouders zijn opgenomen in de autorisatie-records. Hierdoor heeft de afnemer nu antwoord op de vraag "Bij welke bronhouders is data over deze betrokkene te vinden die ik mag inzien?"
3. Afnemer vraagt endpoints van bronhouders op met behulp van de identifiers uit de autorisatie-records. Hierdoor heeft de afnemer nu antwoord op de vraag "Bij welke technische endpoints is data over deze betrokkene te vinden die ik mag inzien?"

Een _Wabvpz-toestemming vooraf_ is anders dan een _expliciete specifieke toestemming vooraf_ of een _veronderstelde toestemming_ doordat bij een _Wabvpz-toestemming vooraf_ geen autorisatie-records worden gesynchroniseerd tussen bronhouder en afnemer (zie stap 1). Een _Wabvpz toestemming vooraf_ is ontwikkeld voor brede, minder gerichte beschikbaarstelling van gegevens en kan daardoor niet worden gebruikt ten behoeve van datalokalisatie zoals hierboven beschreven.

#### 2.1.4 Invulling FAIR-principes

Hieronder wordt in meer detail beschreven op welke wijze met behulp van de Nuts Bolt Zorginzage invulling wordt gegegeven aan de principes van FAIR data. Deze paragraaf is niet normatief.

Findable

* F1. (Meta)data are assigned a globally unique and persistent identifier
  * Onderdeel van informatiestandaarden per zorgtoepassing
* F2. Data are described with rich metadata (defined by R1 below)
  * Onderdeel van informatiestandaarden per zorgtoepassing
* F3. Metadata clearly and explicitly include the identifier of the data they describe
  * Onderdeel van informatiestandaarden per zorgtoepassing
  * Hoofdstuk 2.1.2. Nuts autorisatie-records bevatten identifiers van betrokkene en geautoriseerde gegevens
* F4. (Meta)data are registered or indexed in a searchable resource
  * Hoofdstuk 2.1.3. Data is vindbaar op basis van specifieke autorisatie-records aanwezig in de Nuts-node van de afnemer

Accessible

* A1. (Meta)data are retrievable by their identifier using a standardised communications protocol
  * Hoofdstuk 4.5, RFC008
  * Bronsystemen maken data toegankelijk via https
* A1.1 The protocol is open, free, and universally implementable
  * De Nuts-standaarden zijn open en de Nuts-software is gratis en open source
* A1.2 The protocol allows for an authentication and authorisation procedure, where necessary
  * Hoofdstuk 2.
* A2. Metadata are accessible, even when the data are no longer available
  * Hoofdstuk 2. Autorisatie-records blijven desgewenst beschikbaar wanneer de brondata niet meer beschikbaar is

Interoperable

* I1. (Meta)data use a formal, accessible, shared, and broadly applicable language for knowledge representation.
  * Onderdeel van informatiestandaarden per zorgtoepassing
  * RFC014, Autorisatie-records zijn gestandaardiseerd
* I2. (Meta)data use vocabularies that follow FAIR principles
  * Onderdeel van informatiestandaarden per zorgtoepassing
* I3. (Meta)data include qualified references to other (meta)data
  * Onderdeel van informatiestandaarden per zorgtoepassing

Reusable

* R1. (Meta)data are richly described with a plurality of accurate and relevant attributes
  * Onderdeel van informatiestandaarden per zorgtoepassing
* R1.1. (Meta)data are released with a clear and accessible data usage license
  * Hoofdstuk 4.1. Data mogen alleen worden gebruikt in het kader van een relevante grondslag.
* R1.2. (Meta)data are associated with detailed provenance
  * Onderdeel van informatiestandaarden per zorgtoepassing
* R1.3. (Meta)data meet domain-relevant community standards
  * Onderdeel van informatiestandaarden per zorgtoepassing

### 2.2 Raadplegen

Raadplegen is het daadwerkelijk ophalen en inzien van dossiergegevens door een individuele zorgverlener (gebruiker) van een afnemer.

Het ophalen van medische gegevens is een handeling die niet zonder tussenkomst van een gebruiker kan plaatsvinden. Daarover meer in paragraaf 3.3. In plaats daarvan vereist het altijd een bewuste handeling van een gebruiker. Deze gebruiker identificeert en authenticeert zich met een authenticatiemiddel dat voldoet aan RFC002 zodat de bronhouder zich er met zekerheid van kan vergewissen wie de gegevens opvraagt.

Raadplegen bestaat uit de volgende stappen:

1. Het afnemersysteem controleert of er sprake is van een behandelovereenkomst tussen de afnemer en de betrokkene.
2. Het afnemersysteem controleert of er sprake is van een behandelrelatie (betrokkenheid bij behandeling) tussen de ingelogde gebruiker en de betrokkene.
3. Het afnemersysteem controleert bij de Nuts-node van de afnemer of er sprake is van een of meerdere geldige autorisatie-records.
4. De gebruiker identificeert zich met een authenticatiemiddel met als resultaat een cryptografisch ondertekend authenticatie-contract. Aanvullende eisen op het gebied van de authenticatie van de gebruiker (bijv. aangaande de levensduur of het meermalig gebruik van het authenticatie-contract) kunnen worden opgenomen in het zorgtoepassingprofiel.
5. Het afnemersysteem vraagt per bronhouder voor de geauthenticeerde gebruiker een access token op bij de Nuts-node van de bronhouder, via de Nuts-node van de afnemer. Meerdere autorisatie-records (van 1 bronhouder) kunnen worden gebundeld in 1 token-request.
6. Het afnemersysteem vraagt de gegevens op bij het bronsysteem met behulp van het access token.
7. De bronhouder beoordeelt de binnenkomende gegevensverzoeken en past hierop de geldende access policy toe. Deze access policy verschilt per zorgtoepassing en wordt bepaald in nauwe afstemming met de beheerder van de desbetreffende informatiestandaard.

Na het ophalen van de gegevens is het aan de implementatie van het afnemersysteem en de juridische en organisatorische keuzes van de afnemer wat hiermee wordt gedaan. Globaal zijn er de volgende opties:

1. Gegevens worden enkel eenmalig ter inzage getoond
2. Gegevens worden getoond en de gebruiker heeft de mogelijkheid om specifieke gegevens over te nemen als kopie in het eigen dossier of in een separaat archief (niet in het eigen dossier), gemarkeerd als “kopie”.
3. Alle gegevens worden direct overgenomen als kopie in het eigen archief (niet in het eigen dossier)

Deze keuze is een verantwoordelijkheid van de afnemer en kan per zorgtoepassing of individuele casus worden gemaakt. Het advies is om zo veel mogelijk invulling te geven aan het DIZRA-principe "Data blijven bij de bron". Op deze manier:

* blijven data onder de verantwoordelijkheid van de bronhouder;
* is het voor de betrokkene transparant welke bronhouders welke gezondheidsgegevens registreren;
* is het voor de betrokkene transparant welke personen van welke afnemers deze gezondheidsgegevens raadplegen; en
* is het niet nodig een extra proces voor het actualiseren van overgenomen data te implementeren.

## 3. Oplossingsrichting

Onderstaande paragrafen gegeven een oplossingsrichting voor het proces dat beschreven is in het vorige hoofdstuk. We nemen hierbij [het Nuts manifest](https://nuts.nl/manifest/) als leidraad. De Bolt Zorginzage behelst het publiceren en raadplegen van gegevens (op basis van een expliciete of impliciete toestemming). Het volledig ondersteunen van het verwijsproces valt buiten de scope van de Bolt Zorginzage.

### 3.1 Pull

In het ontwerp van deze Bolt gaan we uit van een pull-mechanisme. Dit houdt in dat gegevens of notificaties niet actief worden gestuurd naar het doelsysteem (push) maar dat het doelsysteem vrij is om op ieder moment gegevens op te halen (pull).

Het voordeel van deze aanpak boven push is dat het afnemersysteem gegevens alleen dan op hoeft te halen wanneer de afnemer ook daadwerkelijk behoefte aan deze gegevens heeft. Op deze manier kan dus beter aan de eis van dataminimalisatie worden voldaan. Ook is het eenvoudiger om vast te stellen dat de gebruiker die gegevens ophaalt de juiste persoon is, en om te voldoen aan de NEN7513- en AVG-verplichting om te loggen welke persoon de gegevens heeft ingezien.

Ook is er gekozen voor een pull-mechanisme omdat voor het initiëren van de Zorginzage het zorgproces bij de afnemer leidend is. De zorgverlener van de afnemer wil voor het voorbereiden of verlenen van zorg de meest actuele relevante gegevens kunnen inzien, ongeacht bij welke bronhouders deze zijn opgeslagen.

### 3.2 Wetgeving

De Bolt Inzage geeft op een gestandaardiseerde manier invulling aan de van toepassing zijnde wetgeving. Dit wordt gedetailleerd beschreven in appendix A.

### 3.3 Beveiliging & vertrouwen

Bij Zorginzage haalt de afnemer gegevens op bij de bronhouder. De bronhouder, als gegevensbeheerder, moet zich ervan vergewissen dat de afnemer inderdaad bij de gegevens mag.

Bij het opvragen is naast de afnemende zorgaanbieder ook een specifieke gebruiker en het afnemersysteem betrokken. Het is het afnemersysteem dat, technisch gezien, de gegevens ophaalt en toont aan de gebruiker. Het is de gebruiker die daadwerkelijk inzage krijgt in de gegevens. Om te voorkomen dat ongeoorloofden de gegevens kunnen ophalen en inzien moet het bronsysteem de volgende zaken kunnen controleren:

1. Welk afnemersysteem verbinding maakt
2. Dat het afnemersysteem als verwerker optreedt voor de afnemende zorgaanbieder
3. Namens welke natuurlijke persoon het afnemersysteem verbinding maakt
4. Dat deze persoon de afnemende zorgaanbieder vertegenwoordigt
5. Dat deze persoon daadwerkelijk geautoriseerd is om de gegevens in te zien

Ervan uitgaande dat er behoefte is aan een zo open mogelijke oplossing, waarbij elke partij evenveel kansen krijgt in de markt (conform DIZRA-principe 4: "gelijk speelveld voor alle leveranciers") en waarbij de beveiliging van een erg hoog niveau is, is gestandaardiseerd gebruik van cryptografische bewijzen noodzakelijk. Elke van de bovengenoemde controles moet met behulp van een cryptografische handtekening kunnen worden uitgevoerd. [RFC002 §7](https://nuts-foundation.gitbook.io/drafts/rfc/rfc002-authentication-token#7-supported-means) specificeert welke middelen geaccepteerd worden voor de persoonlijke identificatie. Middelen die noodzakelijk zijn voor de identificatie van organisaties zijn nog in ontwikkeling. Aanvullende afspraken over het gebruik van authenticatiemiddelen kunnen per zorgtoepassing worden opgenomen in het zorgtoepassingprofiel.

### 3.4 Open & inclusief

We kiezen in het ontwerp van deze Bolt voor open standaarden, volgens het principe van comply or explain. Er worden geen verplichtingen gesteld tot het gebruik van bepaalde services of software. Elke partij is vrij om te kiezen of ze gebruik maken van ondersteunende diensten zolang deze diensten geen eisen of restricties opleggen aan andere partijen.

In de basis betekent dit dat er uitgegaan wordt van een gedistribueerde oplossing, waarbij elke partij gelijk is en waarbij zonder tussenkomst van derden, twee partijen rechtstreeks met elkaar gegevens kunnen delen inzake een bepaalde zorgtoepassing.

## 4. Specificaties

De Bolt Zorginzage heeft als kern het raadplegen van gegevens die bij een bronhouder zijn opgeslagen. In dit hoofdstuk zullen we de specificaties verder uitwerken. Er wordt zo veel mogelijk uitgegaan van breed toepasbare en gestandaardiseerde oplossingen. De Bolt Zorginzage kan in verschillende zorgtoepassingen worden toegepast. Per zorgtoepassing zijn over een aantal onderwerpen specifieke afspraken van toepassing:

1. Identifier van de zorgtoepassing
2. Governance
3. Informatiestandaarden
4. Toegestane authenticatiemiddelen
5. Toegestane grondslagen en bewijzen
6. Beschikbaarheid
7. De toe te passen access policy

Deze specifieke afspraken worden per zorgtoepassing vastgelegd in het zorgtoepassingprofiel. Ieder zorgtoepassingprofiel beschrijft afspraken die aanvullend zijn aan de inhoud de Bolt Zorginzage. Voor afspraken over informatie en data wordt waar mogelijk verwezen naar Nictiz informatiestandaarden. Zie hieronder een overzicht van zorgtoepassingen, identifiers en zorgtoepassingprofielen. Dit overzicht kan eenvoudig worden uitgebreid met andere zorgtoepassingen.

| **Zorgtoepassing**                | **Identifier**                     | **Profiel**                                                                                                                                                  |
| --------------------------------- | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Geboortezorg                      | zorginzage-geboortezorg            | [Zorgtoepassingprofiel Geboortezorg](https://babyconnect.atlassian.net/wiki/spaces/VBC/pages/906166273/) in Afsprakenstelsel Interoperabiliteit Geboortezorg |
| Uitwisseling thuiszorg- huisarts  | zorginzage-huisartsinzagethuiszorg | n.t.b.                                                                                                                                                       |
| AMO (Actueel Medicatie Overzicht) | zorginzage-amo                     | n.t.b.                                                                                                                                                       |

De technische ontwerpen van de Nictiz informatiestandaarden bevatten veel inhoudelijke aspecten van de gegevensuitwisseling (datastructuren en data-interface). De Bolt Zorginzage vult dit aan met 'infrastructurele' zaken: authenticatie, autorisatie, lokalisatie, adressering en logging. Sommige onderdelen zijn op dit moment verder uitgewerkt dan andere. Waar mogelijk wordt verwezen naar bestaande standaarden. Waar een onderdeel nog niet voldoende is uitgewerkt zal worden besproken wat er nog mist en hoe tot een oplossing kan worden gekomen. Het is mogelijk dat het vinden van een oplossingsrichting in een later stadium onderdeel moet worden van de verdere samenwerking tussen de verschillende leveranciers.

### 4.1 Grondslagen en autorisatie-records

Zoals beschreven in hoofdstuk 2 scheiden we de processen raadplegen en publiceren van elkaar. Het daadwerkelijke raadplegen van gegevens door een afnemer is daarmee gescheiden van het aanmaken van een autorisatie-record voor gegevenstoegang door een bronhouder. Dit betekent dat er een mechanisme aanwezig moet zijn dat de toegang tot gegevens regelt. Per zorgtoepassing wordt in het zorgtoepassingprofiel beschreven op basis van welke grondslagen gegevens verwerkt mogen worden.

De verschillende processtappen in Zorginzage vereisen een bepaalde toegang tot gegevens en iedere toegang tot gegevens is gebaseerd op een bepaalde grondslag. Een gestandaardiseerd autorisatie-record maakt duidelijk welke specifieke afnemer toegang heeft tot welke specifieke gegevens bij welke specifieke bronhouder inzake welke specifieke betrokkene. Het autorisatie-record is beschreven in [RFC014](https://nuts-foundation.gitbook.io/drafts/rfc/rfc014-authorization-credential). Per zorgtoepassing wordt in het zorgtoepassingprofiel de access policy beschreven.

In het kader van de Nuts Bolt Zorginzage kan het autorisatie-record een referentie bevatten naar een bewijs dat er daadwerkelijk sprake is van een grondslag, zoals een verwijzing of een expliciete toestemming. Het gebruik van gestandaardiseerde autorisatie-records draagt bij aan de herbruikbaarheid. Daarnaast kan de betrokkene op basis hiervan in de toekomst desgewenst een overzicht (bijv. in haar/zijn PGO) krijgen van wie haar of zijn gegevens kan inzien, en waarom.

### 4.2 Registratie services en endpoints

Voor het autoriseren van afnemers en het verkrijgen van het technisch adres van een bronhouder wordt gebruik gemaakt van het Nuts register volgens [RFC006](https://nuts-foundation.gitbook.io/drafts/rfc/rfc006-distributed-registry) van de Nuts specificatie. Om een zorgtoepassing op basis van de Nuts Bolt Zorginzage te ondersteunen moeten de bronhouder en de afnemer beide een service met hetzelfde type (dezelfde zorgtoepassing) registreren in het register.

Diensten kunnen per leverancier en/of organisatie geregistreerd worden volgens de [service specificatie](https://nuts-foundation.gitbook.io/drafts/rfc/rfc006-distributed-registry#4-services). Voor de bronhouder en afnemer dient er een dienst geregistreerd te worden:

```
{
    "id": "did:nuts:organization_identifier#F1Dsgwngfdg3SH6TpDv0Ta1aOE",
    "type": "zorginzage-xyz",
    "serviceEndpoint": {
        "oauth": "did:nuts:vendor_identifier/serviceEndpoint?type=oauth-xyz",
        "fhir": "did:nuts:vendor_identifier/serviceEndpoint?type=fhir-xyz"
    }
}
```

Het element 'id' zal volgens de specificatie moeten worden opgesteld. Het element 'type' moet hierin gelijk zijn aan de identifer van de zorgtoepassing zoals beschreven in de tabel in paragraaf 4.1. Het serviceEndpoint moet de velden oauth en fhir bevatten. Beide waardes moeten een dynamische verwijzing hebben naar een endpoint zoals gespecificeerd in [RFC006](https://nuts-foundation.gitbook.io/drafts/rfc/rfc006-distributed-registry). Het endpoint waarnaar verwezen wordt vanuit het fhir veld moet een serviceEndpoint hebben welke verwijst naar het fhir base-path. Het endpoint waarnaar verwezen wordt vanuit het oauth veld moet een serviceEndpoint hebben welke verwijst naar het accesstoken endpoint van een OAuth authentication server. Het type in het query veld mag daarbij door de leverancier zelf gekozen worden. De dynamische verwijzingen uit het voorbeeld verwijzen bijvoorbeeld naar de endpoints die de leverancier geregistreerd heeft:

```
{
    "id": "did:nuts:vendor_identifier#F1Dsgwngfdg3SH6TpDv0Ta1aOE",
    "type": "zorginzage-xyz-fhir", 
    "serviceEndpoint": "https://fhir.example.com/base"
}
```

```
{
    "id": "did:nuts:vendor_identifier#F1Dsgwngfdg3SH6TpDv0Ta1aOE",
    "type": "zorginzage-xyz-oauth",
    "serviceEndpoint": "https://nuts.example.com/n2n/auth/v1/accesstoken"
}
```

Endpoints moeten worden geregistreerd zonder / op het einde.

### 4.3 Informatiestandaarden, ZIBs en FHIR

Voor verschillende zorgtoepassingen is door Nictiz een lijst met zorginformatiebouwstenen (ZIBs) gedefinieerd. Dit is onderdeel van het Functioneel Ontwerp van een Informatiestandaard. Deze bouwstenen zijn een feitelijke omschrijving van alle informatie benodigd voor een bepaalde zorgtoepassing. De ZIBs beschrijven de benodigde informatie, maar beschrijven niet hoe de benodigde data door computersystemen kunnen worden uitgewisseld. Daarvoor maken we gebruik van HL7 FHIR. De FHIR-profielen en FHIR search queries voor de verschillende zorgtoepassingen worden door Nictiz beheerd en gepubliceerd als onderdeel van het Technisch Ontwerp van een Informatiestandaard. De mapping van een ZIB naar FHIR is vaak niet een-op-een. Zo wordt de ZIB Persoonsgegevens gemapt op de FHIR profielen: Patient, Coverage en Related Person. De mapping van ZIBs op FHIR profielen is onderdeel van het Technisch Ontwerp van een Informatiestandaard. Per zorgtoepassing wordt in het zorgtoepassingprofiel beschreven aan welke informatiestandaarden de uit te wisselen informatie en data dienen te voldoen.

### 4.4 Context van gegevensuitwisseling

Naast de informatie over welke FHIR-resource kan worden opgehaald is het ook nodig om te specificeren in welke context dit gebeurt. De context van gegevensuitwisseling moet namelijk worden gebruikt voor de beveiliging, zo moet er bekend zijn:

1. Wat is de identiteit van de gebruiker?
2. Vanuit welke partij komt het request?
3. Vanuit welk afnemersysteem komt het request?
4. Vanuit welke bronhouder wordt de data beschikbaar gesteld?

Daarnaast zouden er nog extra attributen opgenomen kunnen worden indien nodig. Elke van bovenstaande punten wordt gedekt door [RFC003](https://nuts-foundation.gitbook.io/drafts/rfc/rfc003-oauth2-authorization).

### 4.5 Beveiligde verbinding

Alle requests worden gedaan over een mutual TLS (mTLS) verbinding. Naast het servercertificaat is er ook nog een client-certificaat benodigd. Het clientcertificaat identificeert het afnemersysteem. Zie [RFC008](https://nuts-foundation.gitbook.io/drafts/rfc/rfc008-certificate-structure).

### 4.6 Authenticatie & Autorisatie

Het bronsysteem heeft de verantwoordelijkheid de data van de bronhouder te beschermen. Het is dus van groot belang een degelijk autorisatie- en authenticatiemodel te hebben. Dit hoofdstuk beschrijft de techniek om de FHIR endpoints te beveiligen middels de veelgebruikte OAuth 2.0 specificatie \[[RFC6749](https://tools.ietf.org/html/rfc6749)].

Voor het verkrijgen van een access token biedt OAuth 2.0 diverse _grant types_. In tegenstelling tot de veel gebruikte gebruikersnaam-en-wachtwoord flow, bestaat de identiteit niet op de autorisatie server maar maken we gebruik van cryptografische afschriften uit vertrouwde registers. Hierdoor is het mogelijk dat de third-party client (in ons geval het afnemersysteem) de gebruiker identificeert en diens identiteit, digitaal ondertekend, doorstuurt naar de authorization server (beheerd door het bronsysteem). Deze OAuth flow wordt beschreven in [RFC7523](https://tools.ietf.org/html/rfc7523).

In het access token request staan:

* Identifiers van de bronhouder en afnemer
* Een identiteit van de gebruiker
* Een identifier van de zorgtoepassing
* Een lijst van autorisatie-records op basis waarvan een access token wordt aangevraagd

Gegeven deze informatie en de aanwezige vertrouwensinformatie in het bronsysteem kan worden vastgesteld dat de gebruiker, de afnemer en het afnemersysteem gerelateerd zijn en kan conform RFC003 een access token worden teruggestuurd. Dit access token kan dan in de data-requests worden gebruikt om toegang te krijgen tot de juiste gegevens.

Aan de hand van het door de bronhouder uitgegeven autorisatie-record en access token kan worden gecontroleerd of toegang tot de gegevens voor een gebruiker van een afnemer is toegestaan. In het autorisatie-record staat immers welke afnemer bij welke resource mag en op welke grondslag deze toegang is gebaseerd. Verschillende autorisatie-records kunnen in 1 access token worden gecombineerd zodat een gebruiker van een afnemer toegang krijgt tot meerdere resources. Dit mechanisme ondersteunt dus automatisch ook meerdere use cases. Zie [RFC003](https://nuts-foundation.gitbook.io/drafts/rfc/rfc003-oauth2-authorization) voor de volledige specificatie.

### 4.7 Auditing

Omdat de identiteit van de gebruiker en de afnemer zijn vastgelegd in het access token kunnen deze ook worden gelogd door het bronsysteem, waarmee voldaan is aan de NEN7513-eisen inzake logging. Deze logging kan desgewenst in de toekomst (bijvoorbeeld via een PGO) inzichtelijk worden gemaakt voor de betrokkene.

### 4.8 Service autorisatie

Vanuit [RFC014](https://nuts-foundation.gitbook.io/drafts/rfc/rfc014-authorization-credential) is er de verplichting om voor elke Bolt een access policy te definiëren. Een access policy beschrijft hoe het autorisatie-record opgesteld moet worden en waar het vervolgens toegang tot geeft. Alleen de bronhouder kan een geldig autorisatie-record aanmaken. Autorisatie-records binnen de context van de Nuts Bolt Zorginzage zijn te herkennen aan een 'purposeOfUse' die start met de prefix "zorginzage". Een autorisatie-record voor een zorgtoepassing die gebruikt maakt van de Bolt Zorginzage geeft geen algemene rechten tot resources. De toegang tot resources wordt bepaald door de inhoud van het element 'resources' van het autorisatie-record te combineren met de inhoud van de access policy van de desbetreffende zorgtoepassing. Hoofdstuk 5 beschrijft de kenmerken van een access policy voor een door de Nuts Bolt Zorginzage ondersteunde zorgtoepassing.

### 4.9 Sequenties

Deze paragraaf beschrijft de sequenties voor de twee processen van de Bolt Zorginzage: publiceren en raadplegen. Het doel is om op technisch niveau meer inzicht te geven in de te doorlopen stappen. De inhoud van deze paragraaf is niet normatief: Voor de implementatie zijn de Nuts specificaties leidend.

#### 4.9.1 Publiceren

Voordat dossiergegevens kunnen worden ingezien, dienen ze door de bronhouder te worden gepubliceerd. Publiceren bestaat uit 2 onderdelen:

1. Bronhouder prepareert de data
2. Bronhouder registreert autorisatie-records.

Deze onderdelen worden hieronder nader uitgewerkt.

**4.9.1.1 Prepareren data**

Voordat een afnemer data kan inzien dient de bronhouder de data toegankelijk en interoperabel te maken. Dit dient te gebeuren conform de voor de zorgtoepassing afgesproken standaarden op het gebied van informatie en data. Het is de bronhouder vrij om de data vooraf te prepareren of deze realtime te genereren.

**4.9.1.2 Registreren autorisatie-record**

De toegang van een afnemer tot data van een bronhouder wordt bepaald aan de hand van autorisatie-records. Een autorisatie-record bevat o.a. informatie over de bronhouder, de afnemer, de betrokkene en de scope van gegevens. De exacte inhoud van autorisatie-records wordt per zorgtoepassing gedetailleerd beschreven in de access policy (zie hoofdstuk 6).

Deze paragraaf beschrijft op een hoog niveau de uit te voeren stappen door de bronhouder, benodigd voor het aanmaken en distribueren van een autorisatie-record. De stappen zijn niet normatief.

![](https://lh5.googleusercontent.com/MROwUFfCGAZ2H-RcXrXDLiPTMZEtAc6VY-4twzRG61osi7NlBq4z3NsWwnZmaP640R7Jgtmp8Z7zHGcuuhkQ\_y4vfBnCmk\_-gQYjMn6Dr957mak3Tuuk9c7jyjIIoGTMuUHqbpkN)

| **Stap** | **Omschrijving**                                                                                                                                                                                  |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2        | Vastleggen toestemming in bronsysteem. De toestemming van expliciet of verondersteld zijn.                                                                                                        |
| 3-4      | Bronsysteem registreert Nuts autorisatie-record in Nuts-node van bronhouder. Hiervoor dient RFC014 gevolgd te worden. Zie het zorgtoepassingprofiel voor de invulling van het autorisatie-record. |
| 5        | Het Nuts netwerk zorgt voor de aflevering van het autorisatie-record bij de Nuts-node van de afnemer opgenomen in het autorisatie-record.                                                         |

#### 4.9.2 Raadplegen

Het door een afnemer daadwerkelijk inzien van dossiergegevens van een andere bronhouder bestaat uit de volgende onderdelen:

1. Authenticeren gebruiker
2. Verkrijgen autorisatie-records, endpoints en access tokens
3. Ophalen resources

Deze onderdelen worden in onderstaand diagram weergegeven en vervolgens nader uitgewerkt.

![](https://lh6.googleusercontent.com/iHWpiRx7-tXMBxdtQ9-FGcQJnFtm457VA2et0EnlBDUVYCNO8cSgm1C2rIqDJ0ekeOEiI0ZXPco7hP7LunXTFzEurqPqC7jvuVvlw3-M8JEy--uelJEF09zGZcFkuG9uI3ry9Mrm)

**4.9.2.1 Authenticeren gebruiker**

Het daadwerkelijke ophalen van gegevens bij een bronhouder is een handeling die niet zonder tussenkomst van een gebruiker kan plaatsvinden (zie o.a. paragraaf 3.3). De gebruiker, die handelt vanuit de context van de afnemer, dient te worden geauthenticeerd op een manier die door de bronhouder wordt vertrouwd.

| Stap     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Stap** | **Omschrijving**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| 1-3      | Het afnemersysteem zal op verzoek van de gebruiker een authenticatieflow starten met een door de gebruiker gekozen authenticatiemiddel. We nemen in deze beschrijving IRMA als authenticatiemiddel. Andere authenticatiemiddelen zullen een soortgelijke flow gebruiken. Er wordt een authenticatiesessie gestart op de Nuts node. Deze geeft een contract en sessie ID terug. RFC002 bevat de details.                                                                                                                                                                                                                      |
| 4-6      | Het resultaat van stap 3 wordt als QR code weergegeven aan de gebruiker. De gebruiker scant de QR code met de camera van de telefoon. Het contract dat in stap 2 is aangemaakt wordt in de IRMA app getoond. De gebruiker ondertekent dit contract met de gevraagde attributen. De app stuurt het ondertekende contract naar de Nuts node van de opvragende partij waarna het afnemersysteem het resultaat van de ondertekening zal opvragen. Het ondertekende contract dient opgeslagen te worden in de sessie van de gebruiker. De levensduur van het authenticatiecontract wordt beschreven in het zorgtoepassingprofiel. |

**4.9.2.2 Verkrijgen autorisatie-records, endpoints en access tokens**

Om data te kunnen ophalen bij een bronhouder dient de afnemer te weten naar welke resource-endpoints (FHIR basepath in combinatie met relatieve paden) requests dienen te worden gestuurd. Om daadwerkelijk toegang te krijgen tot de data van de bronhouder dient de afnemer de juiste access tokens te overleggen. Het samenstellen van de juiste resource-endpoints en het verkrijgen van access tokens start met het verkrijgen van de juiste autorisatie-records.

| **Stap** | **Omschrijving**                                                                                                                                                                                                                                                                                                                   |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 7        | Het afnemersysteem vraagt de actieve autorisatie-records op bij de Nuts-node van de afnemer. Het BSN van de betrokkene, de identifier van de afnemer en de identifier van de use case zijn hierbij essentiële parameters.                                                                                                          |
| 8        | De Nuts-node van de afnemer geeft een lijst van autorisatie-records terug. Ieder autorisatie-record bevat de identifier van de bronhouder en een lijst van resources (relatieve paden) waarvoor de autorisatie geldig is.                                                                                                          |
| 9-14     | Stappen 9-14 worden voor iedere bronhouder doorlopen                                                                                                                                                                                                                                                                               |
| 9-10     | <p>Het autorisatie-record bevat de identifier van de bronhouder. Het afnemersysteem vraagt op basis van deze identifier het FHIR-endpoint op bij de eigen Nuts-node.</p><p>Het afnemersysteem stelt op basis van het FHIR-endpoint en de afspraken in het access policy een lijst met geautoriseerde resource-endpoints samen.</p> |
| 11       | <p>Het afnemersysteem vraagt een access token op bij de Nuts-node van de afnemer op basis van:</p><ul><li>Lijst van autorisatie-records van 1 bronhouder (resultaat van stap 8)</li><li>Authenticatie-contract gebruiker (resultaat van stap 6)</li><li>Identifier van de use case/ zorgtoepassing</li></ul>                       |
| 12-13    | De Nuts-node van de afnemer vraagt volgens [RFC003](https://nuts-foundation.gitbook.io/drafts/rfc/rfc003-oauth2-authorization) een access token aan bij de Nuts-node van de bronhouder.                                                                                                                                            |
| 14       | De Nuts-node van de afnemer levert het access token op aan het afnemersysteem                                                                                                                                                                                                                                                      |

**4.9.2.3 Ophalen resources**

Op dit punt zijn alle voorbereidingen getroffen voor het ophalen van de data bij een of meerdere bronhouders. Na het ophalen van de data kan deze worden gepresenteerd aan de gebruiker.

| **Stap** | **Omschrijving**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 15-19    | Stappen worden voor iedere bronhouder en voor ieder geautoriseerd resource-endpoint doorlopen                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| 15       | <p>Het afnemersysteem verstuurt een request voor een specifieke resource naar het bronsysteem. Het resource-endpoint bestaat uit het FHIR-endpoint als base-url (resultaat van stap 10) aangevuld met een relatief pad. Het relatieve pad is gebaseerd op de lijst resources in het autorisatie-record en de afspraken in de access policy. De access policy specificeert per zorgtoepassing welke relatieve paden worden toegestaan.</p><p>Het access token uit stap 14 wordt hierbij als autorisatie header meegestuurd conform <a href="https://nuts-foundation.gitbook.io/drafts/rfc/rfc003-oauth2-authorization#3.3-use-access-token">RFC003</a>.</p> |
| 16-19    | Het bronsysteem laat het access token controleren door de Nuts-node van de bronhouder. De Nuts-node van de bronhouder zal o.b.v. de gebruikte autorisatie-records inzicht geven in de mogelijke resources die door de afnemer mogen worden geraadpleegd. Het bronsysteem kijkt hoe het binnenkomende request op basis van het autorisatie-record en de voor de zorgtoepassing geldende access policy (hoofdstuk 5) moet worden afgehandeld.                                                                                                                                                                                                                |
| 20-21    | Het afnemersysteem presenteert de gegevens aan de gebruiker                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |

## 5. Access policy

Een onderdeel van een Bolt is het zo exact mogelijk beschrijven welke regels en afspraken gelden bij het bepalen van de toegang tot bepaalde resources. Deze worden per zorgtoepassing beschreven in een access policy. De access policy wordt per zorgtoepassing vastgelegd in het zorgtoepassingprofiel. Het is de taak van een bronsysteem om de access policy te volgen wanneer autorisatie-records worden aangemaakt en wanneer resources worden opgevraagd.

De volgende tabel geeft weer welke zaken per zorgtoepassing in de access policy moeten worden vastgelegd.

| Item                                          | Toelichting                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| access policy niet-persoonsgebonden resources | De aanvullende regels en afspraken die binnen de zorgtoepassing gelden voor het verlenen van toegang tot niet-persoonsgebonden resources (bijv. notificaties)                                                                                                                                                                                                                                                                                                                                                                                                                |
| access policy persoonsgebonden resources      | <p>De aanvullende regels en afspraken die binnen de zorgtoepassing gelden voor het verlenen van toegang tot persoonsgebonden resources. Hierbij dienen de volgende onderwerpen te worden beschreven:</p><ul><li>Inhoud autorisatie-record</li><li>Uiterlijke einddatum autorisatie</li><li>Autorisatie op resource-niveau</li><li>Access token</li><li>Authenticatiecontract</li><li>Regels gegevenstoegang</li><li>Search narrowing</li><li><p>Intrekken autorisatie</p><p>Een voorbeeld hiervan is te vinden in het zorgtoepassingprofiel geboortezorg: LINK</p></li></ul> |

## Bijlage A: wetgeving

De volgende wetteksten zijn relevant in relatie tot de Bolt Zorginzage:

* de WGBO (wet op de geneeskundige behandelovereenkomst);
* de AVG (algemene verordening gegevensbescherming);
* en straks mogelijk de Wegiz (wet elektronische gegevensuitwisseling in de zorg).

De Wabvpz (wet aanvullende bepalingen verwerking persoonsgegevens in de zorg) is niet van toepassing omdat systemen die de Nuts Bolt Zorginzage implementeren niet voldoen aan de definitie van een Elektronisch Uitwisselingssysteem. Dit wordt in paragraaf 3.2.3 nader toegelicht.

De WDO is niet relevant voor deze toepassing, aangezien deze het identificeren van cliënten/patiënten regelt. In het kader van de Zorginzage hebben we alleen te maken met het identificeren van zorgverleners.

In de AVG (Artikel 9 Lid 1 van de AVG en Artikel 22 lid 1 van de UAVG) staat dat persoonsgegevens en bijzondere (medische) persoonsgegevens niet zonder meer verwerkt mogen worden. Het gaat hierbij om zowel de bronhouder als de afnemer. De AVG noemt 6 grondslagen voor verwerking:

1. toestemming van de betrokkene
2. uitvoeren van een overeenkomst
3. wettelijke verplichting
4. vitaal belang van de betrokkene
5. uitvoeren van een publiekrechtelijke taak
6. gerechtvaardigd belang van de organisatie

Voor de bronhouder geldt de grond van wettelijke verplichting (in feite een wettelijke overeenkomst), dit is in de WGBO opgenomen. Voor de afnemer liggen de zaken wat minder eenvoudig. We maken onderscheid tussen 2 scenario's’:

1. Gegevensdeling zonder verwijzing
2. Gegevensdeling in het kader van een verwijzing

Gegevensdeling in het kader van een overdracht wordt nader beschreven in de Nuts Bolt eOverdracht ([link](https://nuts-foundation.gitbook.io/bolts/eoverdracht/leveranciersspecificatie)).

### A.1 Gegevensdeling zonder verwijzing

Er zijn diverse situaties waarin meerdere zorgaanbieders inzage willen hebben in de door elkaar geregistreerde gegevens, zonder dat er sprake is van een verwijzing. Twee voorbeelden:

Een betrokkene kan in zorg zijn bij een thuiszorginstelling en een huisarts. In dit geval heeft er geen verwijzing plaatsgevonden. De betrokkene heeft twee aparte behandelovereenkomsten, een met de thuiszorginstelling en een met de huisarts. Voor het delen van de gegevens tussen de thuiszorginstelling en de huisarts is daarom een expliciete toestemming voor het delen van de gegevens tussen de thuiszorginstelling en de huisarts nodig.

Een betrokkene kan in het kader van integrale geboortezorg gelijktijdig in zorg zijn bij meerdere verschillende zorgaanbieders, bijvoorbeeld een eerstelijns verloskundepraktijk en een kraamzorgorganisatie. In dit geval heeft er geen verwijzing plaatsgevonden. De betrokkene heeft twee aparte behandelovereenkomsten, een met de eerstelijns verloskundepraktijk en een met de kraamzorgorganisatie. Voor het delen van de gegevens tussen de eerstelijns verloskundepraktijk en de kraamzorgorganisatie is daarom een expliciete toestemming voor het delen van de gegevens tussen de eerstelijns verloskundepraktijk en de kraamzorgorganisatie nodig.

De Nuts Bolt Zorginzage vereist dat de bronhouder kan bewijzen dat er sprake is van een expliciete toestemming vooraf of een veronderstelde toestemming. Dit bewijs kan bijvoorbeeld bestaan uit een registratie in het bronsysteem, een ondertekend document of een digitale handtekening. In de autorisatie-records die in de Nuts-nodes van bronhouder en afnemer worden opgeslagen kan een referentie naar dit bewijs worden opgenomen.

### A.2 Gegevensdeling in het kader van een verwijzing

Als er sprake is van een verwijzing, en de betrokkene reeds toestemming heeft gegeven voor de verwijzing mogen relevante gegevens gedeeld worden conform de WGBO.

De Nuts Bolt Zorginzage vereist dat de bronhouder kan bewijzen dat er sprake is van een verwijzing (waarvoor de betrokkene toestemming heeft gegeven). Dit bewijs kan bijvoorbeeld bestaan uit een registratie in het bronsysteem. In de autorisatie-records die in de Nuts-nodes van bronhouder en afnemer worden opgeslagen kan een referentie naar dit bewijs worden opgenomen.

De Bolt Zorginzage ondersteunt van het verwijsproces alleen het raadplegen van gegevens. Andere onderdelen van het verwijsproces zoals bijvoorbeeld workflow-ondersteuning vallen niet binnen de scope.

### A.3 Elektronisch uitwisselingssysteem (Wabvpz)

De Wabvpz stelt in artikel 15a:

"De zorgaanbieder stelt gegevens van de betrokkene slechts beschikbaar via een elektronisch uitwisselingssysteem, voor zover de zorgaanbieder heeft vastgesteld dat de betrokkene daartoe uitdrukkelijk toestemming heeft gegeven."

Een “elektronisch uitwisselingssysteem” is gedefinieerd als:

"een systeem waarmee zorgaanbieders op elektronische wijze, dossiers, gedeelten van dossiers of gegevens uit dossiers voor andere zorgaanbieders raadpleegbaar kunnen maken, waaronder niet begrepen een systeem binnen een zorgaanbieder, voor het bijhouden van een elektronisch dossier;"

Het "raadpleegbaar maken" is door minister Bruins duidelijker omschreven als "vooraf beschikbaar stellen voor onbekend later gebruik".

Binnen de Nuts Bolt Zorginzage worden gegevens vanaf het moment van het aanmaken van een autorisatie-record inderdaad vooraf beschikbaar gesteld aan de geautoriseerde afnemer(s). Er is echter geen sprake van "onbekend later gebruik" (voor meerdere op voorhand niet bekende afnemers) omdat de autorisatie alleen geldig is voor een specifiek doeleinde (specieke afnemer, specifieke betrokkene, specifieke zorgtoepassing, specifieke gegevensset).  Per zorgtoepassing kunnen aanvullende autorisatie-afspraken worden gemaakt die de scope van het "latere gebruik" verder afkaderen. Voor de zorgtoepassing geboortezorg bijvoorbeeld geldt een autorisatie alleen voor een of meerdere aan geboortezorg gerelateerde specifieke episodes (bijv. zwangerschappen). Systemen die de Nuts Bolt Zorginzage implementeren stellen dus geen gegevens beschikbaar voor onbekend later gebruik en vallen daarom niet onder de Wabvpz-definitie van een “elektronisch uitwisselingssysteem”.

In de Wabvpz is opgenomen dat voor gegevensuitwisseling via een “elektronisch uitwisselingssysteem” altijd uitdrukkelijke expliciete toestemming van de betrokkene nodig is. Dit heeft ook een relatie met de AVG vereiste van toestemming voor het verwerken van persoonsgegevens (in een elektronisch uitwisselingssysteem). Omdat er geen verwerking van persoonsgegevens door anderen dan de direct betrokken zorgaanbieders plaatsvindt en de Wabvpz-definitie van een “elektronisch uitwisselingssysteem” niet van toepassing is op de Nuts Bolt Zorginzage, kan de Nuts Bolt Zorginzage zowel voor zowel het inzien op basis van een 'expliciete toestemming vooraf' als voor het inzien op basis van een 'veronderstelde toestemming’ geschikt.
