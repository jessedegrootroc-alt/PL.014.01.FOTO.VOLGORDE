# Fotocheck arrangement

Interne backoffice-tool van ViaLuxury om de fotoreeks van een arrangement samen te
stellen en te controleren voordat die live gaat.

Alles zit in één bestand, `index.html`. Geen build, geen dependencies. Openen in de
browser is genoeg.

## Wat de tool doet

- **De eerste 5 foto's** zijn het hart van de tool. Per positie kies je een type beeld,
  waarna de tool adviseert wat de volgende foto het beste kan zijn. Het advies houdt
  rekening met wat al gebruikt is, of de omgeving al zichtbaar is, en of het gebouw
  aantrekkelijk genoeg is.
- **Fotoniveau** per positie: hotelniveau komt terug bij elk pakket van het hotel,
  arrangementniveau geldt alleen voor dit arrangement.
- **Hoofdbeeldregister**: elk arrangement van hetzelfde hotel heeft een eigen
  droombeeld nodig. Dubbele bestandsnamen worden gemeld.
- **Korte controle** op de rest van de reeks, visuele kwaliteit, seizoen en techniek.

Het zijn richtlijnen, geen harde regels. Afwijken mag, met een toelichting.
Alleen technische eisen en een dubbel hoofdbeeld blokkeren het afronden.

## Lokaal draaien

```bash
python3 -m http.server 8014
```

Daarna http://localhost:8014 openen. Serveer het bestand via een server met
`charset=utf-8`, anders lopen de accenttekens mis.

## Opslag

De ingevulde check blijft in de `localStorage` van de browser staan tot iemand op
leegmaken klikt. Er gaat niets naar een server.
