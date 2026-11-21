# Mijn cyclus

Een app met illustraties gemaakt met Higgsfield die laat zien hoe lichaam, stemming en zelfbeeld door de menstruatiecyclus heen kunnen veranderen. De inhoud is gebaseerd op wetenschappelijk onderzoek en Nederlandse medische informatie (Thuisarts, De Gynaecoloog/NVOG).

## Onderdelen

- **Vandaag**: vul de eerste dag van je laatste menstruatie, je cycluslengte en de duur van je menstruatie in. Je ziet dan op welke cyclusdag en in welke fase je zit, wat er in die fase gemiddeld gebeurt en wanneer de eisprong, de premenstruele dagen en je volgende menstruatie ongeveer komen.
- **Verkennen**: schuif door de dagen van de cyclus, met een schematische grafiek van de hormonen (oestrogeen, progesteron, LH, FSH) en per fase een uitgebreide tabel met energie, stemming, zelfbeeld, slaap, pijn en meer.
- **Dagboek**: per dag in overzichtelijke blokken (menstruatie, lichaam, gevoel, zelfbeeld, slaap, leefstijl en notities) met woorden bij elk cijfer, klachten en gevoelens om aan te tikken, uren slaap, en een hint welke klachten in je huidige fase vaak voorkomen. Met een grafiek van je eigen patroon en een knop om alles als tekst te kopiëren.
- **Mijn cycli**: elke startdag van je menstruatie wordt opgeslagen (met de knop op ‘Vandaag’ of automatisch als je in je dagboek bloedverlies invult). De app berekent je gemiddelde cycluslengte, de kortste en langste cyclus, hoe regelmatig je bent en hoe lang je menstruatie duurt, en vergelijkt dat met de normale waarden van FIGO.
- **Mijn gemiddelden per fase**: uit je dagboek zie je hoe jij je gemiddeld voelt per fase, naast het gemiddelde uit onderzoek.
- **Weten**: achtergrond over je mooi of minder mooi voelen, stemming, PMS, PMDD en premenstruele verergering, tips, wanneer je naar de huisarts gaat, en de bronnen.

## Gebruiken

Open `index.html` in een browser. Er is geen installatie of server nodig.

### Online zetten met GitHub Pages

1. Ga in deze repository naar **Settings → Pages**.
2. Kies bij **Branch** voor `main` en de map `/ (root)` en klik op **Save**.
3. Na een minuut staat de app op `https://abraxas075.github.io/mijn-cyclus-/`.

### Op je telefoon installeren

Open de app in Chrome of Brave, tik op de drie puntjes (⋮) en kies **Toevoegen aan startscherm** of **App installeren**. De app krijgt dan een eigen icoon (een zon), opent zonder adresbalk en werkt ook zonder internet.

## Telefoon-app bouwen met Rork

In [`RORK.md`](RORK.md) staat een complete bouwopdracht om deze app met Rork als echte iOS- en Android-app te laten bouwen: alle schermen, berekeningen, teksten, kleuren en de afbeeldingen uit [`assets/images/`](assets/images/).

## Privacy

Alles wat je invult (je instellingen en je dagboek) wordt alleen opgeslagen in de browser van het apparaat waarop je de app gebruikt (`localStorage`). Er wordt niets naar een server verstuurd. Op een ander apparaat of in een andere browser zie je je gegevens dus niet; gebruik **Kopieer als tekst** om een back-up te maken.

## Let op

- De voorspelde data zijn een schatting op basis van een gemiddelde cyclus. Gebruik ze niet als anticonceptie of om je vruchtbare dagen precies te bepalen.
- De app beschrijft gemiddelde patronen bij een natuurlijke cyclus (zonder hormonale anticonceptie) en vervangt geen persoonlijk medisch advies.
- Denk je aan zelfdoding? Bel 113 Zelfmoordpreventie: 0800-0113 (gratis) of ga naar [113.nl](https://www.113.nl). Bij direct gevaar: 112.
