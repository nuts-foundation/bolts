Capaciteit
##########

Beschrijving
************

De Capaciteit Bolt draait om het inzichtelijk maken bij welke instellingen voor langdurige zorg en thuiszorg er capaciteit ("bedden") beschikbaar is. Ziekenhuizen en artsen die zorg moeten overdragen kunnen van deze informatie gebruik maken om sneller en efficiënter over te dragen. Het zogenaamde "belrondje" kan hiermee worden verkort of zelfs overbodig worden gemaakt.

Deze Bolt heeft in verband met de Corona crisis opeens een hogere prioriteit gekregen, en er is bij veel zorginstellingen een zorgtype "Corona" bijgekomen.

Inzet van de Nuts Node
======================

Gegeven de verhoogde prioriteit, en de vaststelling dat de Nuts Node vandaag nog niet productie-rijp is, willen we deze Bolt mogelijk maken met *en* zonder gebruikmaking van de Nuts Node. De service kan worden vastgelegd in het Nuts register, maar moet ook aanbiedbaar en bevraagbaar zijn zonder gebruik te maken van een Nuts Node.

Achtergrond
***********

Wettelijke basis
================

Informatie over beschikbare capaciteit van zorginstellingen is in beginsel niet wettelijk geregeld. Het is aan instellingen zelf om individueel te bepalen of hier sprake is van concurrentiegevoelige gegevens, maar in de meeste gevallen zal men deze informatie graag vrijelijk beschikbaar stellen.

Er is ook geen sprake van risico's voor de privacy van patiënten.

Dit samen leidt ertoe dat er ook geen beveiliging nodig is, anders dan bescherming tegen DDOS aanvallen, van de technische endpoints voor deze Bolt. We kunnen endpoints open ter beschikking stellen en adverteren in het Nuts register.

Het grootste voordeel van deze informatie openlijk beschikbaar stellen is dat het ook voor het eerst mogelijk wordt om real-time een integraal beeld te krijgen van de beschikbare zorgcapaciteit in Nederland. Dit is waardevolle informatie voor beleidsmakers en voor crisismanagement.

Kopiëren of inzien
==================

Omdat deze informatie van moment tot moment zal variëren is het niet zinnig om informatie te gaan kopiëren, anders dan in de vorm van een cache ten behoeve van beschikbaarheid van de informatie en ter voorkoming van het overvragen van het eigen systeem of dat van anderen.

Technologie
***********

Modellen
========

Onder normale omstandigheden zouden we hier gebruik willen maken van ZIBs over FHIR. Echter is er voor capaciteit nog geen ZIB gedefiniëerd, en we willen vanwege de verhoogde prioriteit in verband met de Corona crisis de "implementeerbaarheid" van deze Bolt zo groot mogelijk maken, vooral ook voor leveranciers met verschillende technische "stacks". Daarom is er (voor nu) gekozen voor een JSON REST API.

CapacityStatement
^^^^^^^^^^^^^^^^^

CapacityList en PostalCodeList zijn voor de leesbaarheid hieronder apart gedocumenteerd, maar worden in praktijk ge-embed in het CapacityStatement.

.. code-block:: json

  {
    "capacity": {
      "intramural": [
        {
          "location": {
            "name":       "Location name (human readable)",
            "address":    "Location address (human readable)",
            "contact":    "Location contact info (human readable)",
            "postalcode": "Location postal code",
            "number":     "Location street number (with additions like A, B, Bis, etc)"
          },
          "total":     <CapacityList>,
          "available": <CapacityList>
        }
      ],
      "extramural": [
        {
          "neighbourhood": {
            "name":        "Neighbourhood name (human readable)",
            "postalcodes": <PostalCodeList>,
            "contact":     "Location contact info (human readable)"
          },
          "total":     <CapacityList>,
          "available": <CapacityList>
        }
      ]
    }
  }

Indien één van de soorten :code:`intramural` of :code:`extramural` niet wordt aangeboden door de organisatie mag de categorie in zijn geheel weggelaten worden. Beide lijsten kunnen vanzelfsprekend nul, één of meerdere locaties of wijken bevatten.

De CapacityList in het veld :code:`total` geeft de totaal beschikbare capaciteit van de locatie of de wijk aan. Dus feitelijk de "omvang" van die locatie. Indien deze informatie niet beschikbaar is mag dit hele veld weggelaten worden.

De CapacityList in het veld :code:`available` geeft de momenteel beschikbare capaciteit aan. Met andere woorden: wat de locatie of de wijk er nog bij kan hebben aan toegevoegde belasting (de spreekwoordelijke "lege bedden").

PostalCodeList
^^^^^^^^^^^^^^

De PostalCodeList is een lijst van strings met daarin postcodes. Een postcode is in dit model een string van 4, 5 of 6 karakters.

Bijvoorbeeld:

* "1234"
* "1234A"
* "1234AB"

De lijst van postcodes mag dus meerdere van zulke strings bevatten, van verschillende specifiekheid. Hiermee wordt een gebied opgebouwd (een wijk of *neighbourhood*) waarin de capaciteit wordt aangeboden.

Indien deze informatie niet beschikbaar is en de naam van de wijk voldoende specifiek is (denk ook wijken die in verschillende steden voorkomen), mag het veld :code:`postalcodes` ook achterwege gelaten worden. Dit maakt het echter wel tamelijk onmogelijk om op een kaart te plotten, dus het heeft zeker niet de voorkeur.

CapacityList
^^^^^^^^^^^^

.. code-block:: json

  [
    {
      type: "Type of care given (human readable)",
      count: <integer>
    }
  ]

De CapacityList is een array met objecten, waarvan elk een :code:`type` en een :code:`count` heeft. :code:`type` is vrij te bepalen, en ter interpretatie voor de (menselijke) ontvangende gebruiker. :code:`count` is het aantal individuele patiënten dat van deze capaciteit gebruik kan maken.

Géén capaciteit dient te worden aangegeven met :code:`"count": 0`, niet door het weglaten van het zorgtype. De twee lijsten :code:`total` en :code:`available` zouden dus, indien beide worden aangeboden, dezelfde zorgtypen moeten bevatten, met een andere :code:`count`. Een geheel lege array is ook geen geldige waarde, dan dient de hele categorie (:code:`intramural` of :code:`extramural`) weggelaten te worden.

Interpretatie
^^^^^^^^^^^^^

Merk dus op dat dit **niet** alleen de fysieke ruimtes of aanwezige bedden betreft, maar ook de benodigde personele bezetting en ruimte in de planning om deze capaciteit aan die patiënten te kunnen leveren. Het is belangrijk dat dit onderscheid duidelijk gepresenteerd wordt aan zorginstellingen en diens medewerkers (bijvoorbeeld in de applicatie), om miscommunicatie te voorkomen.

Kortom: simpelweg het aantal beschikbare kamers exporteren is geen juiste implementatie van deze specificatie.

Operaties
=========

Capaciteit ophalen
^^^^^^^^^^^^^^^^^^

.. code-block:: rest

  GET /{base_url}/capacity.json

Retourneert een CapacityStatement.
