# Fotocheck arrangement

Interne backoffice-tool van ViaLuxury om de fotoreeks van een arrangement samen te
stellen en te controleren voordat die live gaat.

Alles zit in één bestand, `index.html`. Geen build, geen dependencies. Openen in de
browser is genoeg.

## Vijf weergaven

Linksonder staat een schakelaar tussen de versies, ook met de cijfertoetsen.
De tool opent altijd in versie 5; wisselen geldt alleen voor de open pagina en
wordt niet bewaard, na herladen staat versie 5 er weer. De standaard staat in
`STANDAARD_VERSIE` en in het scriptje in de kop van het bestand, verander ze samen.

- **Versie 1** is de opbouw met hoge kaarten en een zijpaneel rechts.
- **Versie 2** zet de eerste 5 foto's om in compacte rijen, maakt de stand van
  zaken een balk, en heeft een extra stap **Foto's toevoegen** tussen stap 00 en
  de eerste vijf. Daar zet je de foto's klaar, bij de eerste 5 foto's kies je ze
  uit een strook miniaturen van twee rijen hoog die naar rechts scrolt; het type
  en de status staan daaronder, links uitgelijnd. Alleen de kaart die aan de
  beurt is (de eerste zonder foto) toont die strook. Kies je een foto, dan
  klapt de kaart in tot een kleine weergave van die foto en gaat de volgende
  open. Met *Andere foto kiezen* of *Foto kiezen* klap je een kaart weer uit.
  Een ingeklapte kaart gebruikt de breedte: links het nummer, dan titel en
  foto, en rechts ernaast het type en de toelichting; op een smal scherm
  staat dat weer onder elkaar.
  Het voorbeeld van de detailpagina (de eerste foto groot, de volgende vier
  ernaast, op volle breedte) blijft daarbij bovenaan het scherm plakken, zodat je bij elke keuze
  live ziet of de foto in de collage past. Klik op een vlak in dat voorbeeld
  en die kaart gaat open, net onder het voorbeeld. In versie 4 zijn de strook
  met vijf tegels en de voortgangsbalk weg, het voorbeeld zegt hetzelfde. Een foto staat op maximaal één positie: zodra hij
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

  Wat versie 4 erbij heeft: **feedback vragen zonder dat de pagina open moet
  blijven**, en zonder dat collega's iets anders nodig hebben dan een browser.
  Bij 05 Techniek staat *Maak reviewbestand*. Dat maakt één HTML-bestand met de
  foto's erin (te zware foto's worden eerst geoptimaliseerd), in de volgorde
  van de reeks. De bestandsnaam volgt de hotelnaam uit stap 00, bijvoorbeeld
  `fotoreeks-kasteel-winselerhof.html` (de zip heet `fotos-kasteel-winselerhof.zip`),
  zodat tien of twintig reeksen op een dag uit elkaar te houden blijven. Je stuurt het via Teams; de collega heeft alleen een browser
  nodig. Het bestand is bewust kaal: één zin uitleg, het voorbeeld zoals de
  gast het op de detailpagina ziet (foto 1 groot, 2 t/m 5 ernaast) met vier
  aandachtspunten eronder (foto 1 is het droombeeld, 2 t/m 5 hotel, kamer en
  includes, geen dubbele beelden, één sfeer en licht), en daaronder alle
  foto's op volgorde. Per foto kan de collega schuiven (‹ ›), vervangen of
  verwijderen, en met *+ Foto toevoegen* een foto achteraan zetten; dat zijn
  de feedback voor die foto. Een tekstveld per foto zit achter een klein
  linkje *Opmerking*, alleen voor wat de collega niet zelf kan oplossen.
  Onderaan staat één oordeel, *Akkoord, kan zo live* of *Nog niet akkoord*,
  met een optionele toelichting en de naam, en één hoofdknop *Download en
  stuur terug*; zonder oordeel gaat de download niet. *Stuur terug via Teams*
  probeert eerst het deelmenu van het systeem met het bestand erin (Chrome en
  Safari, als Teams als deeldoel is geïnstalleerd); lukt dat niet, dan wordt
  het bestand gedownload en gaat Teams open met de feedback als tekst, zodat
  alleen het bestand er nog in gesleept hoeft te worden. Een webpagina kan
  namelijk zelf geen bestand in een Teams-chat zetten. *Kopieer als tekst* is
  er voor een snel antwoord, maar een nieuwe volgorde of nieuwe foto's zitten
  alleen in het gedownloade bestand. In de tool werkt *Stuur door via Teams*
  onder *Feedback vragen* op dezelfde manier met het zojuist gemaakte
  reviewbestand. Komt het bestand terug,
  sleep het dan in de uploadzone: de reeks staat er in de nieuwe volgorde,
  nieuwe en vervangen foto's krijgen een type uit de bestandsnaam, en onder
  *Feedback vragen* staat het oordeel (groen of oranje), wat er is aangepast
  en de opmerkingen per foto.

  Draait de tool in de claude.ai-artifact, dan staan daar bovenop *Bewaar bij
  claude.ai* en het blok *Bewaarde reeksen*: dezelfde flow, maar dan met de
  opslag van claude.ai (`assets` en `db`) in plaats van een bestand, met live
  feedback en *Openen in de tool*. Dat werkt alleen voor wie de artifact kan
  openen; het reviewbestand werkt voor iedereen.

  De CSS van versie 3 staat achter `html:is([data-versie="3"],[data-versie="4"])`,
  wat alleen voor versie 4 geldt achter `html[data-versie="4"]`; in het script
  zijn `metNummers()` en `deelActief()` het onderscheid.

Alles wat geen versie 1 is deelt dezelfde opbouw, dus de CSS daarvoor staat
achter `html:not([data-versie="1"])` en in het script achter `metFotos()`. Een
versie toevoegen is een regel in `VERSIES` plus een blok CSS achter
`html[data-versie="5"]`.

- **Versie 5** is een losse weergave waarin alleen het converteren, SEO-vriendelijk
  hernoemen en downloaden overblijft: upload → hotelinformatie → onderwerp
  herkennen → SEO-bestandsnaam → converteren → controleren → downloaden. Stappen
  00 t/m 04 zijn er niet en alt-teksten ook niet. Boven de foto's staan velden
  voor hotelnaam, plaats, regio, land, naam arrangement en een extra zoekwoord,
  en daaronder de soorten arrangement van de site als aanklikbare knoppen met
  icoon (Nieuwe hotels, Zwembad, Wellnesshotels, Hotels in België, Hond mee,
  Bijzondere overnachtingen, Aan zee, Exclusief bij ViaLuxury, In de natuur,
  Kerstmarkten, Stedentrips, Hotels in Frankrijk, Fietsarrangement, Hotels met
  wellness, Bubbelbad op kamer, 5 sterren, Met diner, Mini vakanties,
  Kasteelhotels, SUPER DEAL); alles blijft in de browser bewaard en wordt alleen
  voor de bestandsnaam gebruikt. Hotels in België of Frankrijk vult het land in
  als dat leeg is. Typ je een hotelnaam, dan zoekt de tool na een korte
  pauze plaats, regio en land op via OpenStreetMap (gratis, zonder sleutel) en
  vult lege velden in; lukt dat niet, dan vraagt de artifact-versie het aan
  Claude. Wat je zelf al had ingevuld blijft staan, en onder het veld staat
  waar de gegevens vandaan komen. Het onderwerp per foto (hotel, hotelkamer, suite, restaurant,
  ontbijt, diner, wellness, zwembad, terras, natuur, bergen, strand, wandelen,
  fietsen, stad, gebouw, omgeving) komt uit de bestandsnaam (`kamer-balkon.jpg`
  → hotelkamer, `wandelpad.jpg` → wandelen); in de artifact kijkt Claude na het
  uploaden naar de foto's zelf en vult het onderwerp in. Niet herkend, dan
  staat dat bij de foto en kies je het zelf. De naam volgt de prioriteit
  onderwerp → zoekintentie → locatie → hotelnaam, met hooguit een paar sterke
  termen: kamers krijgen hotelnaam, regio en onderwerp
  (`hotel-winselerhof-limburg-luxe-hotelkamer`, met Bubbelbad op kamer
  `…-hotelkamer-met-jacuzzi`), voorzieningen krijgen de hotelterm uit de soort
  arrangement (Kasteelhotels → `kasteelhotel-…`, Wellnesshotels →
  `wellness-hotel-limburg-zwembad`, 5 sterren → `luxe-hotel-…`, Aan zee →
  `hotel-aan-zee-…`; Met diner maakt van een restaurantfoto `…-diner`,
  Kerstmarkten van een stadsfoto `…-kerstmarkt`), omgeving en activiteiten krijgen
  hotelnaam, onderwerp en regio (`hotel-winselerhof-wandelen-zuid-limburg`),
  het hotel zelf wordt `hotel-winselerhof-limburg` of
  `landgoedhotel-winselerhof-limburg`. Alles lowercase met koppeltekens,
  hooguit 64 tekens, het woord hotel maar een keer, nooit een volgnummer.
  Dreigen twee foto's dezelfde naam te krijgen, dan wordt de tweede
  inhoudelijk specifieker: eerst met een woord uit de eigen bestandsnaam
  (`…-luxe-hotelkamer-balkon`), dan met de plaats, het arrangement, een kenmerk
  of het land; pas als er niets onderscheidends meer is komt er een cijfer.
  Elke naam is per foto aan te passen. Converteren is vast: altijd JPG,
  maximaal 300 KB, afmetingen blijven waar dat kan; er zijn geen instellingen.
  De groene knop *Optimaliseren* zet alles om, met een voortgangsbalk; de tool
  zoekt de hoogste kwaliteit die past en verkleint pas als de laagste stand
  nog te groot is. Per foto zie je preview, oude en nieuwe naam, onderwerp,
  bestandstype, oude en nieuwe grootte en afmetingen; na het optimaliseren
  verschijnen de downloadknoppen: per foto, of alles in één keer met
  *Download als zip*, met exact de gegenereerde namen.

## Wat de tool doet

- **De eerste 5 foto's** zijn het hart van de tool. Per positie kies je een type beeld,
  waarna de tool adviseert wat de volgende foto het beste kan zijn. Het advies houdt
  rekening met wat al gebruikt is, of de omgeving al zichtbaar is, en of het gebouw
  aantrekkelijk genoeg is.
- **Rest van de reeks** in versie 2: een raster van drie plekken per rij, een plek per foto vanaf foto 6; de rest loopt door op de volgende rij, er hoeft niet naar rechts gescrold te worden.
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
Het zware werk gebeurt al bij het toevoegen in stap 01: elke foto krijgt daar
meteen een werkversie van maximaal 2400 px (een origineel van 20 MB wordt zo
ruim 1 MB) en een miniatuur van 720 px die de tool overal toont. Zonder dat
moest de browser de originelen bij elke tekenbeurt opnieuw decoderen en werd de
pagina traag. Onder de uploadzone loopt daarbij een balk `Foto's klaarzetten`.
Bij 05 werkt `Alles optimaliseren` verder op die werkversie tot de echte eisen
(1920 px en 400 KB voor het hoofdbeeld, 1600 px en 250 KB voor de rest). Dat
gaat snel, dus de balk houdt per foto even aan zodat je ziet wat er gebeurt;
`Opnieuw controleren` loopt op dezelfde manier kort door de reeks.
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
Kies je zelf een type in het veld *Wat voor soort beeld is dit?*, dan staat de
positie meteen op Klopt, ook als het afwijkt van het advies: een eigen keuze is
een bewuste keuze. De waarschuwing met de reden blijft alleen bestaan voor een
type dat de tool zelf uit de foto haalt; met Klopt zet je die weg.

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
