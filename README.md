# Fotocheck arrangement

Interne backoffice-tool van ViaLuxury om de fotoreeks van een arrangement samen te
stellen en te controleren voordat die live gaat.

Alles zit in één bestand, `index.html`. Geen build, geen dependencies. Openen in de
browser is genoeg.

## Vier weergaven

Linksonder staat een schakelaar tussen de versies, ook met de cijfertoetsen.
Versie 4 is de standaard, de keuze van een gebruiker blijft daarna in zijn eigen
browser bewaard. De standaard staat in `STANDAARD_VERSIE` en in het scriptje in
de kop van het bestand, verander ze samen.

- **Versie 1** is de opbouw met hoge kaarten en een zijpaneel rechts.
- **Versie 2** zet de eerste 5 foto's om in compacte rijen, maakt de stand van
  zaken een balk, en heeft een extra stap **Foto's toevoegen** tussen stap 00 en
  de eerste vijf. Daar zet je de foto's klaar, bij de eerste 5 foto's kies je ze
  uit een strook miniaturen van twee rijen hoog die naar rechts scrolt; het type
  en de status staan daaronder, links uitgelijnd. Alleen de kaart die aan de
  beurt is (de eerste zonder foto) toont die strook. Kies je een foto, dan
  klapt de kaart in tot een kleine weergave van die foto en gaat de volgende
  open. Met *Andere foto kiezen* of *Foto kiezen* klap je een kaart weer uit. Een foto staat op maximaal één positie: zodra hij
  ergens staat, bij de eerste 5 of in de rest van de reeks, verdwijnt hij uit
  de keuzerij van de andere posities. Haal je hem weg, dan komt hij terug.
  Kies je een foto, dan vult de tool
  het type beeld meteen in uit de bestandsnaam (`lobby hotel.png` wordt
  Interieur, op plek 1 wordt het de droomvariant), zodat je niet twee keer
  hoeft te kiezen. Zegt de naam niets, dan kijkt Claude in de artifact-versie
  naar de foto zelf en kiest een onderwerp uit de vaste lijst (snel model, één
  keer per foto, meteen bij het toevoegen). Draait de tool als los bestand, of
  herkent Claude niets, dan blijft de keuzelijst leeg met de melding dat je het
  type zelf kiest. Een type dat je zelf hebt gekozen wordt nooit overschreven.

- **Versie 3** is versie 2 zonder de uitleg: geen blokomschrijvingen, geen
  labels als Richtlijn of Harde eis, geen hints en geen specificatietabel, met
  krappere marges. Naast het nummer staan pijl omhoog, sleepgreep en pijl omlaag
  onder elkaar. Ook de balk **Stand van zaken** en de knop Kopieer
  samenvatting zijn hier weg; het Overzicht onderaan laat dezelfde telling
  zien. Het advies zelf blijft staan, de reden
  erachter verhuist naar de tooltip van het adviesblok. Tekst die alleen in de
  uitgebreide versies hoort staat in de markup in een `span.uitgebreid`.

- **Versie 4** is versie 3 zonder controlestap, gebouwd op feedback van de
  backoffice: de feedback op de fotoselectie komt in de praktijk pas nadat het
  pakket in het CMS staat, de volgorde verandert dan nog wel eens en er komen
  op pakketniveau foto's bij. Een extra controlemoment vooraf vertraagt het
  aanmaken alleen maar. Daarom in versie 4:
  - **geen volgnummer in de bestandsnaam**: de naam is alleen de beschrijving,
    dus een latere andere volgorde of een extra foto maakt geen enkele naam
    fout. De zip staat nog wel in de volgorde van de reeks, en elk bestand
    krijgt een oplopende tijd, zodat sorteren op datum in de map dezelfde
    volgorde geeft;
  - **geen Klopt / Afwijking / N.v.t.**, geen blok Visuele kwaliteit, geen
    Overzicht onderaan en geen controlepunt bij Techniek. Het advies per
    positie blijft staan, het is alleen geen check meer die je moet afwerken.
    Kiezen, ordenen, optimaliseren, downloaden.

  Wat versie 4 erbij heeft, alleen in de artifact-versie: **feedback vragen
  zonder dat de pagina open moet blijven.** Bij 05 Techniek staat *Bewaar reeks
  voor feedback*: de foto's gaan naar de opslag van de pagina (`assets`), de
  samenstelling (hotel, kenmerken, posities, beschrijvingen) naar de gedeelde
  database (`db`). Daarna staan *Deel via Teams* (opent het Teams-deelvenster
  met een kant-en-klaar bericht en de link naar de tool) en *Kopieer bericht*
  klaar. De collega opent de tool, kiest de reeks bovenaan onder **Bewaarde
  reeksen**, ziet de foto's in volgorde met hun beschrijving en zet er feedback
  bij, over de hele reeks of over één foto; die feedback komt live bij iedereen
  die de reeks open heeft. Met *Openen in de tool* haalt wie mag bewerken de
  hele reeks terug in de tool om hem aan te passen. Bewaren en verwijderen kan
  alleen wie de pagina mag bewerken, bekijken en feedback geven kan iedereen in
  de organisatie met toegang. Er gaat alleen iets naar de opslag als iemand op
  bewaren klikt; de gewone flow zonder bewaren blijft in de browser. Draait de
  tool als los bestand, dan is dit hele onderdeel onzichtbaar.

  De CSS van versie 3 staat achter `html:is([data-versie="3"],[data-versie="4"])`,
  wat alleen voor versie 4 geldt achter `html[data-versie="4"]`; in het script
  zijn `metNummers()` en `deelActief()` het onderscheid.

Alles wat geen versie 1 is deelt dezelfde opbouw, dus de CSS daarvoor staat
achter `html:not([data-versie="1"])` en in het script achter `metFotos()`. Een
versie toevoegen is een regel in `VERSIES` plus een blok CSS achter
`html[data-versie="5"]`.

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
  volgorde neer. De vrije foto's onder de slider staan in dezelfde strook van
  twee rijen hoog die naar rechts scrolt.
- **Korte controle** op de visuele kwaliteit en de opbouw van de reeks. Met
  **Alles aanvinken** boven de lijst zet je alle punten in een keer op klopt;
  daarna kun je per punt nog terug.
- **Techniek lost zichzelf op**: het blok gaat alleen over de foto's die in de
  reeks staan, dus die je bij de eerste 5 en bij de rest van de reeks hebt
  gekozen. De overige foto's uit de bibliotheek blijven buiten de controle, de
  zip en de beschrijvingen; erboven staat hoeveel dat er zijn. Is er nog niets
  gekozen, dan volgt het blok gewoon alle toegevoegde foto's. De tool
  controleert breedte,
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
Foto's sla je los op, of samen met `Download afbeeldingen in zip`. Elke
bestandsnaam begint (behalve in versie 4) met het volgnummer in de reeks, `01-`, `02-` enzovoort, zodat
de map na het downloaden in de volgorde van de reeks staat: eerst foto 1 t/m 5,
dan de rest van de reeks. Blok 05 toont de foto's in dezelfde volgorde. De zip bevat
alleen de foto's; de beschrijvingen staan in blok 05 bij elke foto. Tijdens het
inpakken vult een balk onder de knoppen per foto tot 100%; dezelfde balk loopt
mee bij `Alles optimaliseren`. De zip wordt in de browser
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

De beschrijving die de tool genereert is kort en algemeen: het onderwerp plus de
hotelnaam, dus "Kamer in Hotel Acropolis" of "Restaurant van Hotel Acropolis",
en bij het gebouw gewoon de hotelnaam. Geen details als tweepersoonskamer of
vooraanzicht, want die kloppen lang niet altijd. Bij het hoofdbeeld komt de
context uit de gekozen kenmerken erbij, bijvoorbeeld "Kasteel Winselerhof, een
kasteelhotel in de natuur", en foto's van de omgeving krijgen de locatie erbij.

De beschrijving volgt het onderwerp van die ene foto. Staat de foto op een van de
eerste vijf posities, dan volgt hij het gekozen type. Is er nog geen type gekozen,
of staat de foto verderop in de reeks, dan leest de tool het onderwerp uit de
oorspronkelijke bestandsnaam: woorden als restaurant, sauna, suite, binnenplaats
of fietsen wijzen het onderwerp aan, waarbij het laatste woord in de naam wint.
Kies je Andere include en typ je zelf een omschrijving, dan is dat het onderwerp.

In de artifact-versie staat er een knop bij: **Beschrijvingen door Claude**. Die
stuurt de foto's zelf mee, zodat het onderwerp klopt met wat er echt op staat.
Claude houdt zich aan dezelfde korte vorm van drie tot zes woorden. Het loopt via de
sample-capability, dus het gebruikt het Claude-tegoed van degene die de tool
open heeft en het vraagt de eerste keer toestemming. Wat iemand zelf heeft
getypt blijft staan. Draait de tool als los bestand, dan is de knop er niet en
blijven de beschrijvingen hieronder gelden.

Elk onderwerp heeft meerdere formuleringen. Staan er twee kamerbeelden in de
reeks, dan krijgt de tweede een andere zin, zodat geen twee foto's dezelfde
beschrijving hebben.

De bestandsnaam is die beschrijving, als slug van maximaal 60 tekens, dus
"Kamer in Hotel Acropolis" wordt `kamer-in-hotel-acropolis.jpg`. Bij het
optimaliseren hernoemt de tool de foto altijd, ook als de oorspronkelijke naam
al goed was, zodat naam en beschrijving bij elkaar horen. Typ je zelf een
beschrijving, dan volgt de bestandsnaam zodra je het veld verlaat. Komen twee
namen toch op hetzelfde uit, dan telt de tweede door als `-2`. Wijzigt de
hotelnaam, een kenmerk of het type beeld, dan lopen de beschrijvingen en de
namen meteen mee.

## Iconen

Het favicon is het oranje VL-blokje. Het staat als `favicon.png` in de map en zit
als data-URI in de kop van `index.html`, zodat het ene bestand genoeg blijft.

De iconen komen uit de set in `MAZUREL/assets/iconen` en staan als `<symbol>`
bovenaan `index.html`. Ze zijn omgezet naar `currentColor`, zodat de tool ze
groen, oranje of rood kleurt via CSS. Een icoon toevoegen betekent: het pad uit
het bronbestand kopiëren, `#212121` vervangen door `currentColor` en er een
`<symbol id="ic-...">` van maken.

## Opslag

De ingevulde check blijft in de `localStorage` van de browser staan tot iemand op
leegmaken klikt. Er gaat niets naar een server.
