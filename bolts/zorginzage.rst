Zorginzage
##########

Beschrijving
************

De zorginzage bolt gaat over het beschikbaar maken van dossiergegevens voor professionals die de voortgang van een zorgtraject willen volgen.
Hierbij worden observaties en metingen gedaan door verplegend personeel en deze worden dan bekeken door een specialist.
Een concreet voorbeeld hiervan is een thuiszorgdossier wat wordt bijgehouden door de thuiszorg en welke wordt ingezien door de huisarts.

Achtergrond
***********

Wettelijke basis
================

Wanneer het voorbeeld in de beschrijving genomen wordt als uitgangspunt dan moet de patiënt/cliënt expliciet toestemming geven om de dossiergegevens te delen met de huisarts.
Hoewel de huisarts wel verantwoordelijk is voor de gezondheid van de gezamenlijke cliënten heeft er geen verwijzing plaatsgevonden van de huisarts naar de thuiszorginstelling.
De cliënt tekent bij de zorginstelling ook apart een zorgleveringsovereenkomst welke dan de verlichaming is van de behandelrelatie tussen thuiszorginstelling en cliënt.
Omdat dit een andere behandelrelatie is dan tussen cliënt en huisarts moet er expliciet toestemming gegeven worden om gegevens te mogen delen vanuit thuiszorginstelling naar huisarts.

Binnen Nuts termen moet het *consent* dus een verwijzing bevatten naar een ondertekend document of digitale handtekening.

Kopiëren of inzien
==================

Medische data over een patiënt (subject) die beschikbaar gesteld wordt vanuit de ene zorgorganisatie (custodian) aan een andere zorgorganisatie (actor) kan alleen bekeken worden wanneer nodig of kan gekopieerd worden (hogere beschikbaarheid).
Wat mag eigenlijk en wat willen we?

Vanuit de patiënt:

De gemiddelde patiënt zal het verschil niet inzien en heeft de behoefte dat hij de best mogelijke zorg krijgt. Patiënten die meer controle willen kunnen wellicht beter ondersteund worden bij het nagaan waar gegevens ‘leven’.

Vanuit de zorgprofessional:

Een zorgprofessional wil gegevens beschikbaar hebben wanneer nodig en wil niet gelimiteerd zijn door techniek. Medische gegevens dienen ter verbetering van de zorg. Dit betekent dat een professional beslissingen zal maken op basis van deze gegevens, wanneer dit gedaan wordt zullen die gegevens ook onderdeel moeten worden van het dossier. Dus zelfs bij inzien moeten gegevens in dat geval ook gekopieerd worden.

Vanuit wetgeving:

De AVG bevat ondermeer ‘het recht op verwijderen’, wanneer gegevens alleen ingezien worden hoeft er natuurlijk niet verwijderd te worden. Wanneer deze gekopieerd worden natuurlijk wel.
Bijkomstigheid is natuurlijk dat gegevens niet zomaar naar een derde partij zijn gegaan, maar dat deze naar een zorgorganisatie zijn gegaan welke ook een behandelrelatie heeft met de patiënt en dus al gegevens had (welke misschien in een los verwijderverzoek ook verwijderd kunnen worden).
Side-note: bewaarplicht medische gegevens is inmiddels 20jaar.
Dit alles gezegd hebbende, heeft Nuts een voordeel, de wettelijke grondslag voor gegevens uitwisseling is namelijk opgeslagen en kan als bron dienen om de patiënt te informeren met welke partij gegevens mogelijk gedeeld zijn.
Daarnaast bevat de logging natuurlijk elke raadpleging door een zorgprofessional van buiten de zorgorganisatie.
Hierdoor zijn we van mening dat de keuze voor alleen inzien of ook kopiëren kan liggen bij het ‘doel-systeem’ (het XIS/ECD van de actor) met als extra voorwaarde dat bij kopiëren de mogelijkheid aanwezig moet zijn dat er genotificeerd moet kunnen worden indien eerder gekopieerde gegevens verwijderd moeten worden. Het niet verwijderen kan namelijk leiden tot een ‘data-breach’ of erger, er kunnen keuzes gemaakt worden o.b.v. foutieve data.

Technologie
***********

ZIBs
====

De gebruikte ZIBs zijn:

===================== ========
Naam                  Versie
===================== ========
Patient               3.1.1
Bloeddruk             3.2
LaboratoriumUitslag   4.5
Lichaamsgewicht       3.1
Lichaamslengte        3.1
Lichaamstemperatuur   3.1.1
O2Saturatie           3.1
Polsfrequentie        3.3
===================== ========

FHIR mapping
============

Voor de uitwisseling wordt HL7 FHIR gebruikt (STU3). De ZIBs zijn gemapt op de volgende FHIR profielen:

===================  ====================================================================  ========================================================
Item                 FHIR profiel                                                          ZIB
===================  ====================================================================  ========================================================
Patient              https://simplifier.net/nictizstu3-zib2017-2-x/nl-core-patient         https://zibs.nl/wiki/Patient-v3.1.1(2019NL)
Bloeddruk            https://simplifier.net/NictizSTU3-Zib2017/ZIB-BloodPressure           https://zibs.nl/wiki/Bloeddruk-v3.1(2018NL)
Lichaamsgewicht      https://simplifier.net/nictizstu3-zib2017/zib-bodyweight              https://zibs.nl/wiki/Lichaamsgewicht-v3.1(2018NL)
Lichaamslengte       https://simplifier.net/nictizstu3-zib2017/zib-bodyheight              https://zibs.nl/wiki/Lichaamslengte-v3.1(2018NL)
Lichaamstemperatuur  https://simplifier.net/nictizstu3-zib2017/zib-bodytemperature         https://zibs.nl/wiki/Lichaamstemperatuur-v3.1.1(2018NL)
O2Saturatie          https://simplifier.net/nictizstu3-zib2017/zib-oxygensaturation        https://zibs.nl/wiki/O2Saturatie-v3.1(2018NL)
Polsfrequentie       https://simplifier.net/NictizSTU3-Zib2017/Zib-PulseRate               https://zibs.nl/wiki/Polsfrequentie-v3.3(2019NL)
Glucose-meting       https://simplifier.net/NictizSTU3-Zib2017/vitalsigns-bloodglucose     https://zibs.nl/wiki/LaboratoriumUitslag-v4.4(2019NL)
===================  ====================================================================  ========================================================

Daarnaast is de 'gewone' tekstrapportage uit de thuiszorg gemapt op een FHIR `Observation`.

FHIR operaties
==============

Patient ophalen
^^^^^^^^^^^^^^^

.. code-block:: rest

   GET /{base_url}/Patient?identifier=http://fhir.nl/fhir/NamingSystem/bsn=999999990

Observaties ophalen
^^^^^^^^^^^^^^^^^^^

.. code-block:: rest

   GET /{base_url}/Observation?subject.identifier=http://fhir.nl/fhir/NamingSystem/bsn=999999990
