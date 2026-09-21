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
  de eerste vijf. Daar zet je de foto's klaar en geef je per foto aan of hij bij
  elk pakket van dit hotel hoort of alleen bij dit pakket. Bij de eerste 5 foto's
  kies je ze daarna uit een rij miniaturen. Kiezen zet meteen het niveau van die
  positie, een foto staat op maximaal één positie, en bij foto 1 krijgen
  hotelniveau-foto's een waarschuwing, want een hoofdbeeld mag niet bij elk
  pakket van het hotel terugkomen.

Een versie toevoegen is een regel in `VERSIES` plus een blok CSS achter
`html[data-versie="3"]`.

## Wat de tool doet

- **De eerste 5 foto's** zijn het hart van de tool. Per positie kies je een type beeld,
  waarna de tool adviseert wat de volgende foto het beste kan zijn. Het advies houdt
  rekening met wat al gebruikt is, of de omgeving al zichtbaar is, en of het gebouw
  aantrekkelijk genoeg is.
- **Fotoniveau** per positie: hotelniveau komt terug bij elk pakket van het hotel,
  arrangementniveau geldt alleen voor dit arrangement.
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

HEIC wordt ook omgezet naar JPG. Browsers kunnen HEIC zelf niet uitpakken, dus
daarvoor haalt de tool eenmalig `libheif` op bij jsDelivr, en alleen zodra er
echt een HEIC-bestand wordt toegevoegd. Het is een bestand van ongeveer 1,2 MB
met de decoder erin, er gaat dus nog steeds geen foto naar buiten. Lukt het
ophalen niet, bijvoorbeeld zonder internet, dan blijft de foto staan met de
melding dat het bestand nog naar JPG moet en kun je het later opnieuw proberen
met `Los op`.

## Iconen

De iconen komen uit de set in `MAZUREL/assets/iconen` en staan als `<symbol>`
bovenaan `index.html`. Ze zijn omgezet naar `currentColor`, zodat de tool ze
groen, oranje of rood kleurt via CSS. Een icoon toevoegen betekent: het pad uit
het bronbestand kopiëren, `#212121` vervangen door `currentColor` en er een
`<symbol id="ic-...">` van maken.

## Opslag

De ingevulde check blijft in de `localStorage` van de browser staan tot iemand op
leegmaken klikt. Er gaat niets naar een server.
