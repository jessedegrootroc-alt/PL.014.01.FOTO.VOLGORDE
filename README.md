# Fotocheck arrangement

Interne backoffice-tool van ViaLuxury om de fotoreeks van een arrangement samen te
stellen en te controleren voordat die live gaat.

Alles zit in één bestand, `index.html`. Geen build, geen dependencies. Openen in de
browser is genoeg.

## Twee weergaven

Linksonder staat een schakelaar tussen versie 1 en versie 2, ook met de
cijfertoetsen. De keuze blijft in de browser bewaard.

- **Versie 1** is de opbouw met hoge kaarten en een zijpaneel rechts.
- **Versie 2** zet de eerste 5 foto's om in compacte rijen, maakt de stand van
  zaken een balk, en heeft een extra stap **Foto's toevoegen** tussen stap 00 en
  de eerste vijf. Daar zet je de foto's klaar, bij de eerste 5 foto's kies je ze
  uit een rij miniaturen. Een foto staat op maximaal één positie en laat op de
  andere posities zien waar hij al staat.

Een versie toevoegen is een regel in `VERSIES` plus een blok CSS achter
`html[data-versie="3"]`.

## Wat de tool doet

- **De eerste 5 foto's** zijn het hart van de tool. Per positie kies je een type beeld,
  waarna de tool adviseert wat de volgende foto het beste kan zijn. Het advies houdt
  rekening met wat al gebruikt is, of de omgeving al zichtbaar is, en of het gebouw
  aantrekkelijk genoeg is.
- **Rest van de reeks** in versie 2: een slider met een plek per foto vanaf foto 6.
  Het aantal plekken volgt de teller van 8 tot 14. Per plek zegt de tool wat er
  hoort, hotelbeeld of omgeving, en bij een foto die daar niet bij past kleurt de
  plek oranje met de reden. Klikken op een foto onder de slider zet hem op de
  eerstvolgende lege plek, met de pijlen schuif je hem naar voren of naar
  achteren. Automatisch aanvullen zet de overgebleven foto's in de geadviseerde
  volgorde neer.
- **Korte controle** op de rest van de reeks en de visuele kwaliteit.
- **Techniek lost zichzelf op**: voeg de foto's toe en de tool controleert breedte,
  bestandsgrootte, bestandstype, bestandsnaam en alt-tekst. Wat hij zelf kan
  oplossen, lost hij op: comprimeren tot onder de limiet, WebP, HEIC of AVIF
  omzetten naar JPG, een beschrijvende bestandsnaam maken en een alt-tekst
  genereren. De breedte staat er als informatie bij, verkleinen gebeurt nooit
  onder de minimale breedte en te kleine foto's worden niet opgeblazen. Het
  onderwerp volgt de volgorde waarin je de foto's toevoegt, foto 1 t/m 5 horen
  bij de posities uit stap 01.

Het zijn richtlijnen, geen harde regels. Afwijken mag, met een toelichting.
Alleen technische eisen en een dubbel hoofdbeeld blokkeren het afronden.

## Lokaal draaien

```bash
python3 -m http.server 8014
```

Daarna http://localhost:8014 openen. Serveer het bestand via een server met
`charset=utf-8`, anders lopen de accenttekens mis.

## Foto's optimaliseren

Alles gebeurt in de browser, met canvas. Er gaat geen enkele foto naar een server.
Foto's sla je los op, of samen als zip met `Alles als zip`. In die zip zit ook
`alt-teksten.txt` met per bestandsnaam de alt-tekst. De zip wordt in de browser
gemaakt, zonder bibliotheek en zonder compressie, want de foto's zijn al
gecomprimeerd.

WebP, AVIF en HEIC worden meteen bij het toevoegen omgezet naar JPG, of naar PNG
als er transparantie in zit. Voor WebP en AVIF is de browser genoeg. Voor HEIC
niet: browsers kunnen dat formaat niet uitpakken, dus daarvoor haalt de tool eenmalig `libheif` op bij jsDelivr, en alleen zodra er
echt een HEIC-bestand wordt toegevoegd. Het is een bestand van ongeveer 1,2 MB
met de decoder erin, er gaat dus nog steeds geen foto naar buiten. Lukt het
ophalen niet, bijvoorbeeld zonder internet, dan blijft de foto staan met de
melding dat het bestand nog naar JPG moet en kun je het later opnieuw proberen
met `Los op`.

## Beschrijvingen en bestandsnamen

De beschrijving die de tool genereert is gericht op vindbaarheid. Hij begint met
wat er op de foto staat, noemt de hotelnaam, en bij het hoofdbeeld komt de
context uit de gekozen kenmerken erbij, bijvoorbeeld "Vooraanzicht van Kasteel
Winselerhof, een kasteelhotel in de natuur". Foto's van de omgeving krijgen de
locatie erbij, de rest blijft kort en beschrijvend, zodat er geen zoekwoorden
worden gestapeld.

De beschrijving volgt het onderwerp van die ene foto. Staat de foto op een van de
eerste vijf posities, dan volgt hij het gekozen type. Is er nog geen type gekozen,
of staat de foto verderop in de reeks, dan leest de tool het onderwerp uit de
oorspronkelijke bestandsnaam: woorden als restaurant, sauna, suite, binnenplaats
of fietsen wijzen het onderwerp aan, waarbij het laatste woord in de naam wint.
Kies je Anders en typ je zelf een omschrijving, dan is dat het onderwerp.

Elk onderwerp heeft meerdere formuleringen. Staan er twee kamerbeelden in de
reeks, dan krijgt de tweede een andere zin, zodat geen twee foto's dezelfde
beschrijving hebben. Hetzelfde geldt voor de bestandsnaam, die telt door als
`-2`. Wijzigt de hotelnaam, een kenmerk of het type beeld, dan lopen de
beschrijvingen en de namen meteen mee.

## Iconen

De iconen komen uit de set in `MAZUREL/assets/iconen` en staan als `<symbol>`
bovenaan `index.html`. Ze zijn omgezet naar `currentColor`, zodat de tool ze
groen, oranje of rood kleurt via CSS. Een icoon toevoegen betekent: het pad uit
het bronbestand kopiëren, `#212121` vervangen door `currentColor` en er een
`<symbol id="ic-...">` van maken.

## Opslag

De ingevulde check blijft in de `localStorage` van de browser staan tot iemand op
leegmaken klikt. Er gaat niets naar een server.
