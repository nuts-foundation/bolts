# Overzicht

### Beschrijving

De zorginzage bolt gaat over het beschikbaar maken van dossiergegevens voor professionals die de voortgang van een zorgtraject willen volgen. Hierbij worden observaties en metingen gedaan door verplegend personeel en deze worden dan bekeken door een specialist. Een concreet voorbeeld hiervan is een thuiszorgdossier dat wordt bijgehouden door de thuiszorg en wordt ingezien door de huisarts.

### Achtergrond

#### Wettelijke basis

Wanneer het voorbeeld in de beschrijving genomen wordt als uitgangspunt dan moet de patiënt/cliënt expliciet toestemming geven om de dossiergegevens te delen met de huisarts. Hoewel de huisarts wel verantwoordelijk is voor de gezondheid van de gezamenlijke cliënten heeft er geen verwijzing plaatsgevonden van de huisarts naar de thuiszorginstelling. De cliënt tekent bij de zorginstelling ook apart een zorgleveringsovereenkomst welke dan de belichaming is van de behandelrelatie tussen thuiszorginstelling en cliënt. Omdat dit een andere behandelrelatie is dan tussen cliënt en huisarts moet er expliciet toestemming gegeven worden om gegevens te mogen delen vanuit de thuiszorginstelling met de huisarts.

In Nuts termen moet het _consent_ dus een referentie bevatten naar een ondertekend document of digitale handtekening waarmee de patiënt expliciet toestemming geeft.

#### Kopiëren of inzien

Medische data over een patiënt \(subject\) die beschikbaar gesteld wordt vanuit de ene zorgorganisatie \(custodian\) aan een andere zorgorganisatie \(actor\) kan alleen ingezien worden wanneer nodig of kan gekopieerd worden naar het eigen ICT systeem. Dit laatste leidt tot een hogere beschikbaarheid. Wat mag eigenlijk en wat willen we?

Vanuit de patiënt:

De gemiddelde patiënt zal het verschil niet inzien en heeft de behoefte dat hij de best mogelijke zorg krijgt. Patiënten die meer controle willen kunnen wellicht beter ondersteund worden bij het nagaan waar gegevens ‘leven’.

Vanuit de zorgprofessional:

Een zorgprofessional wil gegevens beschikbaar hebben wanneer nodig en wil niet gelimiteerd zijn door techniek. Medische gegevens dienen ter verbetering van de zorg. Dit betekent dat een professional beslissingen zal maken op basis van deze gegevens, wanneer dit gedaan wordt zullen die gegevens ook onderdeel moeten worden van het dossier. Dus zelfs bij inzien moeten gegevens in dat geval ook gekopieerd worden.

Vanuit wetgeving:

De AVG bevat ondermeer ‘het recht op verwijderen’, wanneer gegevens alleen ingezien worden hoeft er natuurlijk niet verwijderd te worden. Wanneer deze gekopieerd worden wel. Bijkomstigheid is natuurlijk dat gegevens niet zomaar naar een derde partij zijn gegaan, maar dat deze naar een zorgorganisatie zijn gegaan welke ook een behandelrelatie heeft met de patiënt en dus al gegevens had \(welke misschien in een los verwijderverzoek ook verwijderd kunnen worden\). Side-note: bewaarplicht medische gegevens is inmiddels 20 jaar.

Dit alles gezegd hebbende, heeft Nuts een voordeel: de wettelijke grondslag voor gegevensuitwisseling is opgeslagen en kan als bron dienen om de patiënt te informeren met welke partij gegevens mogelijk gedeeld zijn. Daarnaast bevat de logging natuurlijk elke raadpleging door een zorgprofessional van buiten de zorgorganisatie. Hierdoor zijn we van mening dat de keuze voor alleen inzien of ook kopiëren kan liggen bij het ‘doel-systeem’ \(het XIS/ECD van de actor\) met als extra voorwaarde dat bij kopiëren de mogelijkheid aanwezig moet zijn dat er genotificeerd moet kunnen worden indien eerder gekopieerde gegevens verwijderd moeten worden. Het niet verwijderen kan namelijk leiden tot een ‘data-breach’ of erger, er kunnen keuzes gemaakt worden o.b.v. foutieve data.

### Technologie

#### ZIBs

De gebruikte ZIBs zijn:

| Naam | Versie |
| :--- | :--- |
| Patient | 3.1.1 |
| Bloeddruk | 3.2 |
| LaboratoriumUitslag | 4.5 |
| Lichaamsgewicht | 3.1 |
| Lichaamslengte | 3.1 |
| Lichaamstemperatuur | 3.1.1 |
| O2Saturatie | 3.1 |
| Polsfrequentie | 3.3 |

#### FHIR mapping

Voor de uitwisseling wordt HL7 FHIR gebruikt \(STU3\). De ZIBs zijn gemapt op de volgende FHIR profielen:

| Item | FHIR profiel | ZIB |
| :--- | :--- | :--- |
| Patient | [https://simplifier.net/nictizstu3-zib2017-2-x/nl-core-patient](https://simplifier.net/nictizstu3-zib2017-2-x/nl-core-patient) | [Patient-v3.1.1](https://zibs.nl/wiki/Patient-v3.1.1%282019NL%29) |
| Bloeddruk | [https://simplifier.net/NictizSTU3-Zib2017/ZIBBloodPressure](https://simplifier.net/NictizSTU3-Zib2017/ZIBBloodPressure) | [Bloeddruk-v3.1](https://zibs.nl/wiki/Bloeddruk-v3.1%282018NL%29) |
| Lichaamsgewicht | [https://simplifier.net/nictizstu3-zib2017/zibbodyweight](https://simplifier.net/nictizstu3-zib2017/zibbodyweight) | [Lichaamsgewicht-v3.1](https://zibs.nl/wiki/Lichaamsgewicht-v3.1%282018NL%29) |
| Lichaamslengte | [https://simplifier.net/nictizstu3-zib2017/zibbodyheight](https://simplifier.net/nictizstu3-zib2017/zibbodyheight) | [Lichaamslengte-v3.1](https://zibs.nl/wiki/Lichaamslengte-v3.1%282018NL%29) |
| Lichaamstemperatuur | [https://simplifier.net/nictizstu3-zib2017/zibbodytemperature](https://simplifier.net/nictizstu3-zib2017/zibbodytemperature) | [Lichaamstemperatuur-v3.1.1](https://zibs.nl/wiki/Lichaamstemperatuur-v3.1.1%282018NL%29) |
| O2Saturatie | [https://simplifier.net/nictizstu3-zib2017/ziboxygensaturation](https://simplifier.net/nictizstu3-zib2017/ziboxygensaturation) | [O2Saturatie-v3.1](https://zibs.nl/wiki/O2Saturatie-v3.1%282018NL%29) |
| Polsfrequentie | [https://simplifier.net/NictizSTU3-Zib2017/ZibPulseRate](https://simplifier.net/NictizSTU3-Zib2017/ZibPulseRate) | [Polsfrequentie-v3.3](https://zibs.nl/wiki/Polsfrequentie-v3.3%282019NL%29) |
| Glucose-meting | [https://simplifier.net/NictizSTU3-Zib2017/vitalsignsbloodglucose](https://simplifier.net/NictizSTU3-Zib2017/vitalsignsbloodglucose) | [LaboratoriumUitslag-v4.4](https://zibs.nl/wiki/LaboratoriumUitslag-v4.4%282019NL%29) |

Daarnaast is de ‘gewone’ tekstrapportage uit de thuiszorg gemapt op een FHIR Observation.

#### FHIR operaties

**Patient ophalen**

```text
GET /{base_url}/Patient?identifier=http://fhir.nl/fhir/NamingSystem/bsn|999999990
```

**Observaties ophalen**

```text
GET /{base_url}/Observation?subject.identifier=http://fhir.nl/fhir/NamingSystem/bsn|999999990

```

