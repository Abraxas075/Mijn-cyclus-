# Mijn cyclus — bouwopdracht voor Rork

Dit bestand beschrijft de app **Mijn cyclus** volledig, zodat Rork er een echte telefoon-app (iOS en Android) van kan bouwen. De webversie bestaat al en dient als voorbeeld: https://abraxas075.github.io/Mijn-cyclus-/index.html (broncode: https://github.com/Abraxas075/Mijn-cyclus-).

**Zo gebruik je dit bestand in Rork:** plak eerst de startprompt (hoofdstuk 1) als eerste bericht. Plak daarna hoofdstuk voor hoofdstuk de rest, of voeg het hele bestand als bijlage toe en vraag Rork om het stap voor stap te bouwen.

---

## 1. Startprompt

> Bouw een Nederlandstalige mobiele app "Mijn cyclus" met Expo (React Native, TypeScript, Expo Router). De app laat zien hoe lichaam, stemming en zelfbeeld van een vrouw door de menstruatiecyclus heen kunnen veranderen, op basis van wetenschappelijk onderzoek. Er zijn vier tabbladen: **Vandaag** (cyclusring met de dag van vandaag, wat eraan komt, instellingen en een uitgebreide fasekaart), **Verkennen** (schuif door de dagen met een hormoongrafiek), **Dagboek** (dagelijks invullen in blokken, eigen cycli en eigen gemiddelden per fase) en **Weten** (achtergrondinformatie). Alle gegevens blijven lokaal op het toestel (AsyncStorage), er is geen account en geen server. Stijl: rustig en warm, papierknip-illustraties, diep pruimpaars met vijf fasekleuren, lettertypen Outfit (koppen) en Figtree (tekst), licht en donker thema. Volg de specificatie in de volgende berichten precies, inclusief alle teksten.

---

## 2. Wat de app doet

- Berekent op welke **cyclusdag en fase** de gebruiker zit (menstruatie, folliculaire fase, eisprong, luteale fase, premenstruele fase) en wanneer de eisprong, premenstruele dagen en volgende menstruatie ongeveer komen.
- Laat per fase zien wat er gemiddeld gebeurt met **hormonen, lichaam, stemming en zelfbeeld**, met een uitgebreide gevoelstabel (12 onderdelen × 5 fasen).
- Een **dagboek** om dagelijks bloedverlies, pijn, klachten, stemming, energie, gevoelens, zelfbeeld, zin in seks, slaap, beweging, stress en notities bij te houden.
- **Cycli automatisch bijhouden**: startdagen van de menstruatie worden opgeslagen (ook automatisch vanuit het dagboek). De app berekent gemiddelde cycluslengte, regelmaat en duur en vergelijkt die met de normaalwaarden van FIGO.
- **Eigen gemiddelden per fase** uit het dagboek, naast het gemiddelde uit onderzoek.
- **Achtergrondinformatie** over je mooi of minder mooi voelen, stemming, PMS/PMDD/PME, nuances, tips, wanneer naar de huisarts, en bronnen.

Belangrijk: de app beschrijft **gemiddelde patronen** bij een natuurlijke cyclus (zonder hormonale anticonceptie). Ze is **geen anticonceptie**, bepaalt de vruchtbare dagen niet precies en vervangt geen medisch advies. Toon dit duidelijk (zie hoofdstuk 10).

---

## 3. Techniek

- **Expo** (nieuwste SDK), **TypeScript**, **Expo Router** met 4 tabbladen onderaan: `Vandaag`, `Verkennen`, `Dagboek`, `Weten`.
- **Opslag:** `@react-native-async-storage/async-storage`, alles lokaal. Geen login, geen backend, geen analytics.
- **Grafieken:** `react-native-svg` voor de cyclusring, de hormoongrafiek en de patroongrafiek.
- **Afbeeldingen:** `expo-image`. Gebruik de afbeeldingen uit hoofdstuk 4.3 lokaal vanuit `assets/images/` (niet hotlinken).
- **Lettertypen:** `@expo-google-fonts/outfit` (500, 600) en `@expo-google-fonts/figtree` (400, 600, 700).
- **Delen/back-up:** `Share` uit React Native (of `expo-sharing` met een .csv-bestand) in plaats van klembord.
- **Datum kiezen:** `@react-native-community/datetimepicker`.
- **Thema:** volg het systeem (`useColorScheme`), licht en donker.
- Optioneel (hoofdstuk 11): `expo-notifications` voor herinneringen.

---

## 4. Ontwerp

### 4.1 Kleuren

| Token | Licht | Donker | Gebruik |
|---|---|---|---|
| achtergrond | #FFFFFF | #16101A | schermachtergrond |
| vlak | #F8F3F7 | #211925 | panelen, kaarten |
| tekst | #2A1F2B | #F1E9F0 | gewone tekst |
| gedempt | #6B5F6C | #B8AAB8 | toelichting |
| lijn | #EADFE8 | #3A2F3D | randen, lege balken |
| pruim | #4A2340 | #E7B6D8 | koppen, knoppen |
| band | #502039 | #502039 | kopbalk en tabbalk (gelijk aan de achtergrond van de hoofdillustratie) |
| tekst-op-kleur | #FFFFFF | #1A1018 | tekst op een fasekleur |
| waarschuwing | #9E2F45 (vlak #FBEDEF) | #F08A9C (vlak #3A1C22) | 113-blok, afwijkingen |

Fasekleuren (sterk / lichte tint):

| Fase | Licht | Donker |
|---|---|---|
| Menstruatie (`men`) | #A63D5A / #F8E1E7 | #E07892 / #3A1F29 |
| Folliculair (`fol`) | #2F7F6A / #DFF0E9 | #5DBB9F / #173029 |
| Eisprong (`eis`) | #A86E10 / #FBEDD0 | #E0A645 / #33270F |
| Luteaal (`lut`) | #5E4C9A / #E8E3F5 | #A593E0 / #262040 |
| Premenstrueel (`pre`) | #7E3F72 / #F1E0EE | #CF86BF / #33192F |

Hormoonlijnen (getest op kleurenblindheid): oestrogeen #B23A6B (donker #D9588C), progesteron #3E6FB0 (#5E8FDB), LH #B07612 (#B8852A), FSH #16907A (#26A286, gestippeld).

### 4.2 Vormgeving

- Koppen in Outfit 600, tekst in Figtree 16–17 px; niets kleiner dan 13 px.
- Panelen met afgeronde hoeken (16–20 px), fijne rand in `lijn`.
- Knoppen en tikvlakken minimaal 44 px hoog.
- Geen emoji als icoon; gebruik de Higgsfield-illustraties.
- Eén rustige animatie: het markeerbolletje op de cyclusring schuift naar de dag van vandaag (uit bij "beweging verminderen").

### 4.3 Afbeeldingen (gemaakt met Higgsfield, papierknipstijl)

De afbeeldingen staan in de repository in `assets/images/` (zelfde bestandsnamen). Kopieer ze naar `assets/images/` van de Expo-app. De ronde plaatjes tonen als cirkel (`borderRadius` = helft van de breedte); de hoeken van de bronafbeelding vallen dan weg.

| Bestand | Gebruik | Bron |
|---|---|---|
| `hero.jpg` | Kopbalk: maanfasen tussen bloemen op pruimpaars (#502039). Op de telefoon 130 px hoog, `contentFit="cover"`. | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/hero.jpg |
| `fase-men.jpg` | Menstruatie: nieuwe maan met rozenblaadjes | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/fase-men.jpg |
| `fase-fol.jpg` | Folliculair: wassende maan met knoppen | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/fase-fol.jpg |
| `fase-eis.jpg` | Eisprong: volle maan in een gouden bloem | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/fase-eis.jpg |
| `fase-lut.jpg` | Luteaal: afnemende maan met lavendel en bessen | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/fase-lut.jpg |
| `fase-pre.jpg` | Premenstrueel: afnemende sikkel met vallende blaadjes | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/fase-pre.jpg |
| `blok-menstruatie.jpg` | Dagboekblok Menstruatie (druppel) | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/blok-menstruatie.jpg |
| `blok-lichaam.jpg` | Dagboekblok Lichaam (hart met blaadjes) | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/blok-lichaam.jpg |
| `blok-gevoel.jpg` | Dagboekblok Gevoel (zon achter wolk) | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/blok-gevoel.jpg |
| `blok-slaap.jpg` | Dagboekblok Slaap (maan op wolk) | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/blok-slaap.jpg |
| `blok-zelfbeeld.jpg` | Dagboekblok Zelfbeeld (spiegel met bloemen) | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/blok-zelfbeeld.jpg |
| `blok-leefstijl.jpg` | Dagboekblok Leefstijl (plantje in de wind) | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/blok-leefstijl.jpg |
| `blok-notities.jpg` | Dagboekblok Notities (notitieboekje) | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/blok-notities.jpg |
| `icon.png` | App-icoon en splash: gouden zon met bloemblaadjes op #502039 (splash-achtergrond #502039) | https://raw.githubusercontent.com/Abraxas075/Mijn-cyclus-/main/assets/images/icon.png |

---

## 5. Schermen

### 5.0 Kop en tabbladen
Bovenaan een pruimpaarse band (`band`) met de titel **Mijn cyclus**, ondertitel "Hoe je lichaam, stemming en zelfbeeld door de maand heen kunnen veranderen" en `hero.png`. Tabbalk onderaan met vier tabbladen; actief tabblad in `pruim`.

### 5.1 Vandaag
1. **Cyclusring** (groot, SVG, max. 440 px breed). Een ring met per fase een boog in de fasekleur, kleine gaatjes tussen de bogen. Binnen de ring een stippenkring met één stip per dag. Op de dag van vandaag een witte markering met donkere rand op de ring en een gekleurde stip. In het midden: "dag", groot het dagnummer (Outfit 60), "van ± {lengte} dagen" en de korte fasenaam in de fasekleur. Ligt de startdatum in de toekomst: "Kies een datum in het verleden". Is de cyclus langer dan verwacht: "over tijd · dag".
2. **Legenda** onder de ring: per fase een gekleurde stip, de korte naam en de datums ("12 nov – 16 nov").
3. **Paneel "Wat komt eraan"**: rijen met gekleurde stip, titel, datum en relatieve tijd ("over 3 dagen", "morgen", "vandaag"):
   - Eisprong (geschat): datum van (eisprongdag − 1) tot (eisprongdag + 1), of "nu ongeveer / deze dagen".
   - Premenstruele dagen vanaf (dag L − 5).
   - Volgende menstruatie rond (dag L + 1).
   Daaronder: "Berekend met jouw gemiddelde cycluslengte (n cycli)." of "Berekend met de cycluslengte uit je instellingen." en altijd: "Een schatting, niet geschikt als anticonceptie of om de vruchtbare dagen precies te bepalen."
   Is de menstruatie later dan verwacht: waarschuwingsblok "Volgens je instellingen werd je menstruatie rond {datum} verwacht ({relatief}). Begonnen? Tik op ‘Mijn menstruatie is vandaag begonnen’ of pas de datum aan. Blijft je menstruatie uit en kun je zwanger zijn, doe dan een zwangerschapstest."
4. **Paneel "Jouw cyclus"**: datumkiezer "Eerste dag laatste menstruatie", stappers "Cycluslengte" (21–40 dagen) en "Menstruatie duurt" (2–8 dagen), schakelaar "Cycluslengte automatisch berekenen uit mijn cycli" (alleen zichtbaar als er minstens één volledige cyclus is; toont "Gemiddelde van n cycli: X dagen"; als hij aan staat is de stapper uitgeschakeld), grote knop **Mijn menstruatie is vandaag begonnen** (met "Ongedaan maken" in een toast). Bij de eerste keer staat er een voorbeelddatum (vandaag − 9) met de tekst "De datum hierboven is een voorbeeld. Vul je eigen datum in; die wordt op dit apparaat onthouden."
5. **Fasekaart** (zie 5.5) voor de fase van vandaag, met badge "dag X" en "vandaag {lange datum}".

### 5.2 Verkennen
1. Titel "Door de cyclus heen", uitleg "Schuif door de dagen of tik in de grafiek. De lijnen tonen het verloop van de hormonen schematisch, geen echte meetwaarden."
2. Paneel met groot "dag X van L", een schuifregelaar (1–L) en knoppen ‹ ›.
3. Legenda van de vier hormonen en de **hormoongrafiek** (SVG, 240–340 px hoog): fasebanden als lichte achtergrondvlakken met de fasenaam bovenaan, vier lijnen (formules in 6.6), een verticale lijn op de gekozen dag met gekleurde bolletjes, directe labels "oestrogeen", "LH", "progesteron", "FSH" bij de pieken, x-as met dagen. Tikken/slepen in de grafiek kiest een dag.
4. Onder de grafiek per hormoon een balk met het niveau op die dag en het woord "laag" (< 0,3), "gemiddeld" (< 0,6) of "hoog".
5. Fasekaart (5.5) van de gekozen dag.
6. **Een maand in vier weken**: vier kaarten in de lichte fasetint met fase-illustratie (zie `vierWeken` in hoofdstuk 8).
7. **Alle fasen naast elkaar**: tabel met rijen = fasen (met klein rond plaatje) en kolommen Hormonen, Lichaam, Stemming, Zelfbeeld (op de telefoon gestapeld per fase).

### 5.3 Dagboek
**A. Invulformulier** (paneel):
- Bovenaan: knop ‹, datumkiezer, knop ›, knop "Vandaag". Toekomstige dagen kunnen niet.
- **Samenvattingsblok** in de lichte fasetint: rond faseplaatje, "{weekdag dag maand} · cyclusdag X", daaronder "{Fasenaam}. Veel vrouwen merken nu: …" (uit `typischPerFase`). Als de dag al is ingevuld: "Al ingevuld: opslaan werkt deze dag bij."
- **Zeven inklapbare blokken** (zie `dagboekBlokken` in hoofdstuk 8), elk met het rond blok-icoon (46 px), de titel en een **live samenvatting** van wat is ingevuld ("Pijn: licht · krampen, moe") of "Nog niet ingevuld". Menstruatie, Lichaam, Gevoel, Zelfbeeld en Slaap staan standaard open; Leefstijl en Notities dicht.
  - Type `s` (schaal): knoppen naast elkaar, elk met het cijfer groot en het woord eronder klein. Gekozen knop gevuld in de blokkleur met tekst-op-kleur. Schalen met `d: null` zijn optioneel: nogmaals tikken wist de keuze (toon "optioneel · nogmaals tikken = wissen").
  - Type `c` (keuzes): pillen om aan te tikken (meerdere mogelijk). Pillen die in `typischPerFase` voor de fase van de gekozen dag staan, krijgen een klein stipje ●; toon "● = vaak in deze fase".
  - Type `b`: vinkje "Pijnstiller genomen".
  - Type `h`: stapper uren geslapen, 0–14 in stappen van 0,5; leeg tot de eerste tik (begint op 7); knop "Wissen".
  - Type `n`: tekstvak notities (placeholder "Bijvoorbeeld: ibuprofen genomen, druk op werk, vraag voor de huisarts").
- **Opslaanbalk** die onderaan in beeld blijft: knop **Dag opslaan** en "Leegmaken". Na opslaan een toast "Dag opgeslagen" / "Dag bijgewerkt"; bij automatisch gestarte cyclus "Nieuwe cyclus automatisch gestart op {datum}" met "Ongedaan maken".

**B. Patroongrafiek** "Jouw stemming en zelfbeeld per cyclusdag": x = cyclusdag 1..L met fasebanden, y = 1..5, per ingevulde dag twee stippen (stemming in luteaal-paars, zelfbeeld in menstruatie-rood, iets naast elkaar).

**C. Lijst van ingevulde dagen** (nieuwste eerst): gekleurd streepje in de fasekleur, "{za 21 nov} · dag X", regel "Stemming Goed · Energie Hoog · Slaap Goed (7,5 uur) · Zelfbeeld Mooi · Pijn Geen · Bloedverlies Geen", daaronder pillen met klachten, gevoelens, "Pijnstiller", "Zin in seks: …", "Beweging: …", "Stress: …" en de notitie. Knoppen "Wijzig" (laadt de dag in het formulier) en ✕ (verwijderen met "Ongedaan maken").

**D. Back-up**: tekst "Je dagboek blijft alleen op dit apparaat. Maak af en toe een back-up of neem het mee naar de huisarts." Knop **Delen als bestand** (CSV via deelmenu, kolommen in 7.3) en **Alles wissen** (twee keer tikken ter bevestiging, met "Ongedaan maken").

**E. Mijn cycli**: tegels (2×2) met gemiddelde cycluslengte (n cycli), kortste en langste cyclus, regelmaat ("regelmatig", "iets wisselend", "onregelmatig" of "nog te weinig") met "X dagen verschil tussen kortste en langste", en bloedverlies per menstruatie (uit dagboek). Daaronder per cyclus een balk (lengte/45) met startdatum en "29 d"; balken buiten 24–38 dagen in de waarschuwingskleur. Opmerkingen volgens 6.4. Lijst "Startdagen" (nieuwste eerst, "cyclus van X dagen" of "huidige cyclus · dag X", ✕ om te verwijderen) en een datumkiezer "Eerdere startdag toevoegen".

**F. Mijn gemiddelden per fase**: tabel met kolommen = 5 fasen (rond plaatje + korte naam) en rijen Stemming, Energie, Slaap, Zelfbeeld, Pijn (0–3), Uren slaap (als ingevuld), Vaak genoemd (top 3 klachten/gevoelens met "4×"), Ingevulde dagen. Elke cel: gemiddelde met één decimaal (komma) en een dunne balk in de fasekleur. Daaronder een zin als "Jouw stemming is gemiddeld het hoogst in de folliculaire fase (4,1) en het laagst in de premenstruele fase (2,3)." (alleen fasen met ≥ 3 dagen; verschil < 0,3: "is in alle fasen ongeveer gelijk"), anders "Vul minstens 3 dagen in per fase om je eigen patroon te zien." en "Pijn: lager is minder pijn."

**Voorbeeldgegevens:** zolang er nog niets is ingevuld, toont het dagboek 24 dagen voorbeeldgegevens en 4 voorbeeldstartdagen, duidelijk gemarkeerd met een label "voorbeeld" en de tekst "Hieronder staan voorbeeldgegevens. Ze verdwijnen zodra je je eerste eigen dag opslaat." Ze worden nooit opgeslagen en tellen niet mee in de voorspellingen.

### 5.4 Weten
Een rij knoppen bovenaan die naar de secties springt: Mooi of minder mooi · Vrolijk of somber · PMS en PMDD · Nuances · Tips · Naar de huisarts · Bronnen. Teksten in hoofdstuk 9. Het 113-blok altijd in de waarschuwingskleur. Links openen in de browser.

### 5.5 Fasekaart (op Vandaag en Verkennen)
- Kop in de fasekleur met rond faseplaatje (64 px), fasenaam, "dag a–b" en badge "dag X".
- Introzin (groot), daarna vier vakken: Hormonen, Lichaam, Stemming, Zelfbeeld (op telefoon onder elkaar, breed 2×2 of 4 naast elkaar).
- **Gevoelstabel** "Hoe voel je je gemiddeld in deze fase?" met uitleg "De vijf balkjes zijn de vijf fasen van de maand: hoger is prettiger, de fase van nu is omlijnd. Het gaat om gemiddelden met kleine verschillen; bij jou kan het anders zijn." Per onderdeel (`gevoelstabel`, gegroepeerd per `groep`): naam, woord voor deze fase in de fasekleur, vijf minibalkjes (hoogte = niveau × 20%, eigen fasekleur, niet-actieve fasen 50% doorzichtig, actieve fase omlijnd, labels Mens/Foll/Eis/Lut/Pre) en de uitleg. Onderdelen met niveau `null` (groep "Ter informatie") hebben geen balkjes.
- Heeft de gebruiker ≥ 3 eigen dagen in deze fase: onder Energie, Stemming, Je mooi voelen, Slaap en Pijn en krampen de regel "**Jij: gemiddeld 3,8 van 5 · 12 dagen**" (pijn: "van 3 (lager = minder pijn)").
- Tipvak in de lichte fasetint: "**Tip** · …".

---

## 6. Logica en berekeningen

### 6.1 Basis
- Datums als lokale datum `YYYY-MM-DD`; verschil in hele dagen.
- `L` = effectieve cycluslengte (6.3), `duur` = menstruatieduur (instelling, 2–8), eisprongdag `o = L − 14`.

### 6.2 Fase van een cyclusdag `d` bij lengte `L`
```ts
function phaseAt(d: number, L: number, duur: number): 0|1|2|3|4 {
  const o = L - 14;
  if (d <= duur) return 0;            // menstruatie
  if (Math.abs(d - o) <= 1) return 2; // eisprong (o-1 .. o+1)
  if (d < o) return 1;                // folliculair
  if (d > L - 6) return 4;            // premenstrueel (laatste 6 dagen)
  return 3;                           // luteaal
}
```
Bij 28 dagen: menstruatie 1–5, folliculair 6–12, eisprong 13–15, luteaal 16–22, premenstrueel 23–28. Is `d > L` (over tijd): premenstrueel.

### 6.3 Cycli en effectieve lengte
- `cycles`: gesorteerde lijst unieke startdagen. Laatste startdag = "eerste dag laatste menstruatie".
- Lengtes = verschil tussen opeenvolgende startdagen. Alleen lengtes van 15–60 dagen tellen mee.
- `autoL` = afgerond gemiddelde van de laatste (max.) 6 geldige lengtes.
- `L = (auto aan && autoL) ? clamp(autoL, 21, 45) : instelling`.
- **Startdag toevoegen** (knop, datumkiezer of dagboek): ligt er al een startdag binnen 14 dagen, vervang die (correctie); anders toevoegen. Altijd ongedaan te maken.
- **Cyclusdag van een datum**: zoek de laatste startdag ≤ datum → `cd = verschil + 1`, lengte van die cyclus = afstand tot de volgende startdag (of `L` voor de huidige). Geen startdag ervoor: tel terug vanaf de ingestelde startdatum met lengte `L` (modulo).

### 6.4 Statistiek en FIGO-normaalwaarden
- Regelmaat (verschil kortste–langste, pas vanaf 3 lengtes): ≤ 7 "regelmatig", 8–9 "iets wisselend", ≥ 10 "onregelmatig".
- Duur menstruatie: per startdag het aantal aaneengesloten dagboekdagen vanaf de startdag met bloedverlies > 0; gemiddelde.
- Altijd tonen: "Normaal volgens de internationale gynaecologenvereniging FIGO: een cyclus van 24 tot 38 dagen, hooguit 7 à 9 dagen verschil tussen je kortste en langste cyclus, en maximaal 8 dagen bloedverlies."
- Extra bij afwijking: gemiddelde < 24 of > 38 → "Je gemiddelde cycluslengte valt buiten 24–38 dagen. Houd het nog een paar cycli bij en bespreek het met je huisarts als het zo blijft." Verschil ≥ 10 → "Je cycli verschillen 10 dagen of meer van elkaar. Stress, ziekte of reizen kunnen meespelen; blijft het zo, bespreek het dan met je huisarts." Duur > 8 → "Je bloedverlies duurt gemiddeld langer dan 8 dagen. Bespreek dit met je huisarts." Lengte buiten 15–60 → "Een cyclus korter dan 15 of langer dan 60 dagen telt niet mee in het gemiddelde. Klopt de datum?"

### 6.5 Automatisch een nieuwe cyclus starten vanuit het dagboek
Bij opslaan van een dag (niet in de toekomst) met bloedverlies > 0: loop terug zolang de vorige dag ook bloedverlies > 0 heeft; dat is de eerste bloeddag `f`. Als `f` nog geen startdag is en de vorige startdag ≥ 15 dagen eerder ligt (of er nog geen eigen startdag is), voeg `f` toe als startdag en toon "Nieuwe cyclus automatisch gestart op {datum}" met "Ongedaan maken".

### 6.6 Hormoonkrommen (schematisch, 0–1,1)
```ts
const g = (x:number, m:number, s:number, h:number) => h * Math.exp(-0.5 * ((x - m) / s) ** 2);
function H(d:number, L:number) {
  const o = L - 14;
  return {
    e2:  0.12 + g(d, o - 1, 2.2, 0.88) + g(d, o + 7, 3, 0.42),   // oestrogeen
    p4:  0.04 + g(d, o + 7.5, 3.2, 0.82),                        // progesteron
    lh:  0.08 + g(d, o, 0.55, 0.95),                             // LH-piek
    fsh: 0.10 + g(d, 0.46 * o, 3, 0.2) + g(d, o, 0.6, 0.3) + g(d, L + 1.5, 2, 0.12)
  };
}
```

### 6.7 Eigen gemiddelden
Per dagboekdag de fase bepalen met 6.3 + 6.2 (met de lengte van díe cyclus). Per fase: aantal dagen, gemiddelde van stemming, energie, slaap, zelfbeeld, pijn en uren slaap, en tellingen van klachten + gevoelens (top 3).

---

## 7. Gegevens en opslag (AsyncStorage)

### 7.1 Sleutels
- `mc.set`: `{ start: 'YYYY-MM-DD', L: number, duur: number, auto: boolean }`
- `mc.cycles`: `string[]` startdagen
- `mc.log`: `Entry[]`
- `mc.noex`: `true` als de gebruiker het dagboek heeft gewist (dan geen voorbeelden meer)

### 7.2 Entry (één per datum; opslaan op dezelfde datum overschrijft)
```ts
type Entry = {
  d: string;            // datum
  bl: 0|1|2|3;          // bloedverlies: geen, licht, gemiddeld, veel
  pijn: 0|1|2|3;        // geen, licht, matig, veel
  st: 1|2|3|4|5;        // stemming
  en: 1|2|3|4|5;        // energie
  zb: 1|2|3|4|5;        // je mooi voelen
  sl: 1|2|3|4|5;        // slaapkwaliteit
  uur?: number;         // uren geslapen (0–14, stap 0,5)
  sym?: string[];       // klachten
  mood?: string[];      // gevoelens
  pil?: boolean;        // pijnstiller genomen
  seks?: 1|2|3;         // zin in seks
  bew?: 0|1|2;          // beweging: geen, licht, gesport
  stress?: 1|2|3;       // stress
  n: string;            // notitie
};
```

### 7.3 CSV-export
Kolommen: `datum;cyclusdag;bloedverlies;pijn;stemming;energie;slaap;uren_slaap;zelfbeeld;zin_in_seks;beweging;stress;klachten;gevoelens;pijnstiller;notitie`, daarna een lege regel, `startdagen menstruatie` en de startdagen.

---

## 8. Inhoud (alle teksten en tabellen)

Gebruik deze JSON letterlijk als `content.ts`. Fase-id's: `men`, `fol`, `eis`, `lut`, `pre`. In `gevoelstabel` is `niveau` 1–5 (hoger = prettiger) of `null` (alleen informatie). In `dagboekBlokken` is `t` het type (`s` schaal, `c` keuzes, `b` vinkje, `h` uren, `n` notitie), `a` de eerste waarde van de schaal, `d` de standaardwaarde (`null` = optioneel), `c` de kleur (fasekleur of hormoonkleur) en `o` of het blok standaard open staat.

```json
{
 "fasen": [
  {
   "id": "men",
   "kort": "Menstruatie",
   "naam": "Menstruatie",
   "intro": "Het baarmoederslijmvlies laat los. Voor veel vrouwen de zwaarste dagen van de maand, al trekt het na een paar dagen meestal snel weg.",
   "hormonen": "Oestrogeen en progesteron zijn laag. Omdat ze de hersenen niet meer remmen, stijgt FSH weer en gaan nieuwe eiblaasjes rijpen.",
   "lichaam": "Krampen zijn heel gewoon (45 tot 95% van de vrouwen, 2 tot 29% ernstig). Ze komen door prostaglandinen die de baarmoeder laten samentrekken. Ook moeheid, rugpijn en slechter slapen door pijn.",
   "stemming": "Negatieve stemming is vaak het sterkst op de bloedingsdagen zelf. Na de eerste dagen voelen veel vrouwen opluchting.",
   "zelfbeeld": "Vaak minder tevreden: opgeblazen, puistjes, ‘niet lekker in je vel’. Hoe groot je je lichaam inschat, verandert in onderzoek níét.",
   "tip": "Neem een pijnstiller zoals ibuprofen vroeg, bij de eerste pijn. Een kruik helpt ook. Plan deze dagen wat rustiger."
  },
  {
   "id": "fol",
   "kort": "Folliculair",
   "naam": "Folliculaire fase",
   "intro": "De ‘lichte’ fase: het groeiende eiblaasje maakt steeds meer oestrogeen en veel vrouwen krijgen weer energie.",
   "hormonen": "Oestrogeen stijgt sterk; progesteron blijft laag.",
   "lichaam": "Meer energie, betere en rustigere slaap, minder vocht vasthouden. De huid is vaak rustiger.",
   "stemming": "Stemming en zelfvertrouwen nemen bij veel vrouwen geleidelijk toe.",
   "zelfbeeld": "Minder ontevredenheid over het lichaam dan in de tweede helft van de cyclus.",
   "tip": "Een goed moment voor uitdagingen, drukke afspraken of zwaardere trainingen, als jij je nu sterker voelt."
  },
  {
   "id": "eis",
   "kort": "Eisprong",
   "naam": "Eisprong",
   "intro": "Gemiddeld het ‘beste’ moment van de maand: veel vrouwen voelen zich nu positiever, zelfverzekerder en aantrekkelijker.",
   "hormonen": "Oestrogeen piekt, gevolgd door een korte, hoge LH-piek; ongeveer 24 tot 36 uur later springt de eicel. Testosteron stijgt licht.",
   "lichaam": "Soms korte, eenzijdige pijn in de onderbuik (middenpijn) en meer helder, rekbaar slijm. De eetlust is gemiddeld iets lager.",
   "stemming": "Positiever en zelfverzekerder. Zin in seks piekt halverwege de cyclus, vooral samen met de oestrogeenpiek.",
   "zelfbeeld": "Piek in je aantrekkelijk en begeerlijk voelen, zo blijkt uit een dagboekstudie onder 872 vrouwen. Het effect is gemiddeld klein.",
   "tip": "Geniet ervan als je het zo ervaart, maar maak je geen zorgen als je er weinig van merkt."
  },
  {
   "id": "lut",
   "kort": "Luteaal",
   "naam": "Luteale fase",
   "intro": "Na de eisprong maakt het gele lichaam progesteron. Voor de meeste vrouwen een rustige, neutrale fase.",
   "hormonen": "Progesteron stijgt sterk, oestrogeen geeft een tweede, kleiner piekje. Je lichaamstemperatuur ligt ’s nachts ongeveer 0,3 °C hoger.",
   "lichaam": "Meer trek, een iets hoger energieverbruik, en vaak wat onrustiger slapen: minder droomslaap, meer lichte slaap.",
   "stemming": "Meestal stabiel. Progesteron werkt normaal licht kalmerend, maar bij gevoelige vrouwen kunnen de schommelingen juist onrust en prikkelbaarheid geven.",
   "zelfbeeld": "Meestal neutraal; aan het eind van deze fase neemt de ontevredenheid bij sommigen toe.",
   "tip": "Houd de slaapkamer koel en vaste slaaptijden aan. Meer trek is normaal: je lichaam verbruikt iets meer energie."
  },
  {
   "id": "pre",
   "kort": "Premenstrueel",
   "naam": "Premenstruele fase",
   "intro": "Progesteron en oestrogeen dalen snel. Bij een deel van de vrouwen geeft juist die daling klachten.",
   "hormonen": "De hormonen dalen. Vrouwen met PMS hebben niet méér of minder hormonen, maar zijn gevoeliger voor de wisselingen (Thuisarts).",
   "lichaam": "Gespannen of pijnlijke borsten, opgeblazen buik, vocht vasthouden, hoofdpijn, moeheid, slecht slapen, trek in zoet. Acne verergert vaak.",
   "stemming": "Bij een deel prikkelbaar, huilerig, somber of gespannen; bij PMS of PMDD ernstig. Lang niet bij alle vrouwen.",
   "zelfbeeld": "Samen met de menstruatie het moment waarop vrouwen zich het minst mooi voelen.",
   "tip": "Neem geen grote beslissingen over je uiterlijk of eetpatroon. Minder zout, cafeïne en alcohol kan helpen."
  }
 ],
 "gevoelstabel": [
  {
   "groep": "Gevoel",
   "onderdeel": "Energie",
   "perFase": [
    {
     "fase": "men",
     "niveau": 2,
     "woord": "lager",
     "uitleg": "Bloedverlies, krampen en slechter slapen maken veel vrouwen moe."
    },
    {
     "fase": "fol",
     "niveau": 4,
     "woord": "stijgt",
     "uitleg": "Stijgend oestrogeen en rustigere slaap geven veel vrouwen weer meer energie."
    },
    {
     "fase": "eis",
     "niveau": 5,
     "woord": "hoog",
     "uitleg": "Rond de oestrogeenpiek voelen veel vrouwen zich het fitst."
    },
    {
     "fase": "lut",
     "niveau": 3,
     "woord": "gemiddeld",
     "uitleg": "Meestal stabiel; door wat onrustiger slapen soms iets minder fit."
    },
    {
     "fase": "pre",
     "niveau": 2,
     "woord": "lager",
     "uitleg": "Moeheid is een van de meest genoemde premenstruele klachten."
    }
   ]
  },
  {
   "groep": "Gevoel",
   "onderdeel": "Stemming",
   "perFase": [
    {
     "fase": "men",
     "niveau": 2,
     "woord": "vaak lager",
     "uitleg": "In onderzoek is negatieve stemming vaak het sterkst op de eerste bloedingsdagen zelf; daarna volgt vaak opluchting."
    },
    {
     "fase": "fol",
     "niveau": 4,
     "woord": "stijgt",
     "uitleg": "De stemming neemt bij veel vrouwen geleidelijk toe."
    },
    {
     "fase": "eis",
     "niveau": 5,
     "woord": "gemiddeld het best",
     "uitleg": "In dagboekstudies stijgt positieve stemming rond het midden van de cyclus."
    },
    {
     "fase": "lut",
     "niveau": 3,
     "woord": "meestal stabiel",
     "uitleg": "Voor de meeste vrouwen een neutrale fase."
    },
    {
     "fase": "pre",
     "niveau": 2,
     "woord": "bij een deel lager",
     "uitleg": "Een deel wordt somberder of huileriger. In 38% van 47 studies was er géén verband met de cyclus."
    }
   ]
  },
  {
   "groep": "Gevoel",
   "onderdeel": "Rust en geduld",
   "perFase": [
    {
     "fase": "men",
     "niveau": 2,
     "woord": "soms prikkelbaar",
     "uitleg": "Vooral in de eerste dagen, door pijn en moeheid."
    },
    {
     "fase": "fol",
     "niveau": 4,
     "woord": "meestal rustig",
     "uitleg": "Een fase met weinig spanning voor de meeste vrouwen."
    },
    {
     "fase": "eis",
     "niveau": 4,
     "woord": "meestal rustig",
     "uitleg": "Geen bekende toename van spanning of prikkelbaarheid."
    },
    {
     "fase": "lut",
     "niveau": 3,
     "woord": "wisselend",
     "uitleg": "Progesteron werkt normaal licht kalmerend, maar bij gevoelige vrouwen kunnen de schommelingen juist onrust geven."
    },
    {
     "fase": "pre",
     "niveau": 2,
     "woord": "vaker prikkelbaar",
     "uitleg": "Prikkelbaarheid, spanning en boosheid zijn kernklachten bij PMS en PMDD."
    }
   ]
  },
  {
   "groep": "Gevoel",
   "onderdeel": "Zelfvertrouwen",
   "perFase": [
    {
     "fase": "men",
     "niveau": 3,
     "woord": "wisselend",
     "uitleg": "Hangt sterk samen met hoe je je lichamelijk voelt."
    },
    {
     "fase": "fol",
     "niveau": 4,
     "woord": "neemt toe",
     "uitleg": "Zelfvertrouwen neemt bij veel vrouwen in deze fase toe."
    },
    {
     "fase": "eis",
     "niveau": 5,
     "woord": "hoogst",
     "uitleg": "In de grote dagboekstudie steeg het zelfvertrouwen rond de eisprong, maar minder sterk dan je aantrekkelijk voelen."
    },
    {
     "fase": "lut",
     "niveau": 3,
     "woord": "gemiddeld",
     "uitleg": "Meestal neutraal."
    },
    {
     "fase": "pre",
     "niveau": 3,
     "woord": "wisselend",
     "uitleg": "Kan lager zijn als je je somber, moe of opgeblazen voelt."
    }
   ]
  },
  {
   "groep": "Zelfbeeld en seks",
   "onderdeel": "Je mooi voelen",
   "perFase": [
    {
     "fase": "men",
     "niveau": 2,
     "woord": "minder",
     "uitleg": "Meer ontevreden over je lichaam, terwijl je lichaam in onderzoek niet groter wordt ingeschat."
    },
    {
     "fase": "fol",
     "niveau": 4,
     "woord": "verbetert",
     "uitleg": "Minder ontevredenheid over het lichaam dan in de tweede helft van de cyclus."
    },
    {
     "fase": "eis",
     "niveau": 5,
     "woord": "piek",
     "uitleg": "Vrouwen voelen zich vóór en rond de eisprong aantrekkelijker en begeerlijker (872 vrouwen, 70 dagen). Met de pil blijft dit uit."
    },
    {
     "fase": "lut",
     "niveau": 3,
     "woord": "neutraal",
     "uitleg": "Aan het eind van deze fase neemt de ontevredenheid bij sommigen toe."
    },
    {
     "fase": "pre",
     "niveau": 2,
     "woord": "minder",
     "uitleg": "Opgeblazen gevoel, vocht en puistjes. In een studie bij tienermeisjes was de kans op ontevredenheid ruim 2× zo groot."
    }
   ]
  },
  {
   "groep": "Zelfbeeld en seks",
   "onderdeel": "Zin in seks",
   "perFase": [
    {
     "fase": "men",
     "niveau": 3,
     "woord": "wisselend",
     "uitleg": "Hier is weinig eenduidig onderzoek naar; het verschilt sterk per vrouw."
    },
    {
     "fase": "fol",
     "niveau": 4,
     "woord": "stijgt",
     "uitleg": "Stijgend oestrogeen hangt samen met meer verlangen, met ongeveer twee dagen vertraging."
    },
    {
     "fase": "eis",
     "niveau": 5,
     "woord": "piek",
     "uitleg": "Verlangen piekt halverwege de cyclus: het best onderbouwde effect van allemaal."
    },
    {
     "fase": "lut",
     "niveau": 3,
     "woord": "daalt",
     "uitleg": "Progesteron hangt samen met minder verlangen."
    },
    {
     "fase": "pre",
     "niveau": 2,
     "woord": "vaak minder",
     "uitleg": "Minder zin in seks is een veelgenoemde premenstruele klacht."
    }
   ]
  },
  {
   "groep": "Lichaam",
   "onderdeel": "Slaap",
   "perFase": [
    {
     "fase": "men",
     "niveau": 2,
     "woord": "soms slechter",
     "uitleg": "Pijn en krampen kunnen de nachtrust verstoren."
    },
    {
     "fase": "fol",
     "niveau": 4,
     "woord": "goed",
     "uitleg": "Lagere lichaamstemperatuur ’s nachts en rustigere slaap."
    },
    {
     "fase": "eis",
     "niveau": 4,
     "woord": "meestal goed",
     "uitleg": "Meestal geen opvallende veranderingen."
    },
    {
     "fase": "lut",
     "niveau": 3,
     "woord": "iets onrustiger",
     "uitleg": "Minder droomslaap, meer lichte slaap, hogere hartslag en een warmer lichaam."
    },
    {
     "fase": "pre",
     "niveau": 2,
     "woord": "vaak slechter",
     "uitleg": "Slecht slapen hoort bij de typische premenstruele klachten."
    }
   ]
  },
  {
   "groep": "Lichaam",
   "onderdeel": "Pijn en krampen",
   "perFase": [
    {
     "fase": "men",
     "niveau": 1,
     "woord": "krampen",
     "uitleg": "Bij 45 tot 95% van de vrouwen, 2 tot 29% ernstig. Door prostaglandinen; ibuprofen helpt vaak goed."
    },
    {
     "fase": "fol",
     "niveau": 5,
     "woord": "weinig",
     "uitleg": "Voor de meeste vrouwen een klachtenvrije fase."
    },
    {
     "fase": "eis",
     "niveau": 4,
     "woord": "soms middenpijn",
     "uitleg": "Korte, eenzijdige pijn in de onderbuik rond de eisprong."
    },
    {
     "fase": "lut",
     "niveau": 4,
     "woord": "weinig",
     "uitleg": "Meestal weinig pijn."
    },
    {
     "fase": "pre",
     "niveau": 2,
     "woord": "hoofdpijn, borsten",
     "uitleg": "Hoofdpijn, spierpijn en gespannen of pijnlijke borsten."
    }
   ]
  },
  {
   "groep": "Lichaam",
   "onderdeel": "Buik en vocht",
   "perFase": [
    {
     "fase": "men",
     "niveau": 2,
     "woord": "opgeblazen",
     "uitleg": "De eerste dagen voelen veel vrouwen zich nog opgeblazen."
    },
    {
     "fase": "fol",
     "niveau": 5,
     "woord": "weinig vocht",
     "uitleg": "Minder vocht vasthouden."
    },
    {
     "fase": "eis",
     "niveau": 4,
     "woord": "meestal rustig",
     "uitleg": "Meestal geen opvallende veranderingen."
    },
    {
     "fase": "lut",
     "niveau": 3,
     "woord": "neutraal",
     "uitleg": "Bij sommigen begint het vocht vasthouden aan het eind van deze fase."
    },
    {
     "fase": "pre",
     "niveau": 2,
     "woord": "vocht vasthouden",
     "uitleg": "Opgeblazen buik en vocht vasthouden; minder zout kan helpen."
    }
   ]
  },
  {
   "groep": "Lichaam",
   "onderdeel": "Huid",
   "perFase": [
    {
     "fase": "men",
     "niveau": 3,
     "woord": "soms puistjes",
     "uitleg": "Opvlammingen van vlak vóór de menstruatie trekken langzaam weg."
    },
    {
     "fase": "fol",
     "niveau": 4,
     "woord": "vaak rustiger",
     "uitleg": "De huid is in deze fase vaak rustiger."
    },
    {
     "fase": "eis",
     "niveau": 4,
     "woord": "meestal rustig",
     "uitleg": "Meestal geen opvallende veranderingen."
    },
    {
     "fase": "lut",
     "niveau": 3,
     "woord": "neutraal",
     "uitleg": "Meestal geen opvallende veranderingen."
    },
    {
     "fase": "pre",
     "niveau": 2,
     "woord": "vaker puistjes",
     "uitleg": "44% van 400 vrouwen meldde een acne-opvlamming vóór de menstruatie, vaker boven de 33."
    }
   ]
  },
  {
   "groep": "Ter informatie: niet goed of slecht",
   "onderdeel": "Eetlust en trek",
   "perFase": [
    {
     "fase": "men",
     "niveau": null,
     "woord": "wisselend",
     "uitleg": "Verschilt per vrouw."
    },
    {
     "fase": "fol",
     "niveau": null,
     "woord": "gemiddeld",
     "uitleg": "Geen opvallende veranderingen."
    },
    {
     "fase": "eis",
     "niveau": null,
     "woord": "iets minder",
     "uitleg": "Oestrogeen hangt samen met iets minder eten."
    },
    {
     "fase": "lut",
     "niveau": null,
     "woord": "meer trek",
     "uitleg": "Je lichaam verbruikt ongeveer 7% meer energie; meer trek is normaal."
    },
    {
     "fase": "pre",
     "niveau": null,
     "woord": "trek in zoet",
     "uitleg": "Eetbuien of trek in zoet komen vaak voor."
    }
   ]
  },
  {
   "groep": "Ter informatie: niet goed of slecht",
   "onderdeel": "Lichaamstemperatuur",
   "perFase": [
    {
     "fase": "men",
     "niveau": null,
     "woord": "laag",
     "uitleg": "Na de daling van progesteron zakt de temperatuur."
    },
    {
     "fase": "fol",
     "niveau": null,
     "woord": "laag",
     "uitleg": "Lager dan in de tweede helft van de cyclus."
    },
    {
     "fase": "eis",
     "niveau": null,
     "woord": "stijgt na de eisprong",
     "uitleg": "Progesteron laat de temperatuur na de eisprong oplopen."
    },
    {
     "fase": "lut",
     "niveau": null,
     "woord": "± 0,3 °C hoger",
     "uitleg": "’s Nachts gemiddeld ongeveer 0,3 °C warmer dan in de eerste helft."
    },
    {
     "fase": "pre",
     "niveau": null,
     "woord": "nog verhoogd",
     "uitleg": "Blijft hoger tot de menstruatie begint."
    }
   ]
  }
 ],
 "vierWeken": [
  {
   "fase": "men",
   "titel": "Week 1 · dag 1–7",
   "tekst": "De menstruatie begint. Krampen, moeheid en je opgeblazen of minder mooi voelen; de stemming kan wat lager zijn. Na een paar dagen trekt dat meestal weg."
  },
  {
   "fase": "fol",
   "titel": "Week 2 · dag 8–14",
   "tekst": "Oestrogeen stijgt. Meer energie, goede slaap, meer zelfvertrouwen. Rond de eisprong voelen veel vrouwen zich het aantrekkelijkst en hebben ze meer zin in seks."
  },
  {
   "fase": "lut",
   "titel": "Week 3 · dag 15–21",
   "tekst": "Progesteron neemt het over. Iets warmer, meer trek, wat onrustiger slapen. Voor de meeste vrouwen een stabiele, neutrale week."
  },
  {
   "fase": "pre",
   "titel": "Week 4 · dag 22–28",
   "tekst": "De hormonen dalen. Gevoelige borsten, opgeblazen buik, onrustiger huid. Een deel wordt prikkelbaarder of somberder en voelt zich minder mooi; meestal mild."
  }
 ],
 "dagboekBlokken": [
  {
   "t": "Menstruatie",
   "c": "men",
   "o": 1,
   "h": "Hoeveel bloed je vandaag verliest. Vul je op een nieuwe dag bloedverlies in, dan start er automatisch een nieuwe cyclus.",
   "q": [
    {
     "t": "s",
     "k": "bl",
     "l": "Bloedverlies",
     "v": [
      "Geen",
      "Licht",
      "Gemiddeld",
      "Veel"
     ],
     "a": 0,
     "d": 0
    }
   ]
  },
  {
   "t": "Lichaam",
   "c": "lut",
   "o": 1,
   "h": "Pijn en lichamelijke klachten. Tik alles aan wat je vandaag merkt.",
   "q": [
    {
     "t": "s",
     "k": "pijn",
     "l": "Pijn",
     "v": [
      "Geen",
      "Licht",
      "Matig",
      "Veel"
     ],
     "a": 0,
     "d": 0
    },
    {
     "t": "c",
     "k": "sym",
     "l": "Klachten",
     "v": [
      "Krampen",
      "Rugpijn",
      "Hoofdpijn",
      "Gevoelige borsten",
      "Opgeblazen buik",
      "Puistjes",
      "Trek in zoet",
      "Misselijk",
      "Moe",
      "Middenpijn"
     ]
    },
    {
     "t": "b",
     "k": "pil",
     "l": "Pijnstiller genomen"
    }
   ]
  },
  {
   "t": "Gevoel",
   "c": "fol",
   "o": 1,
   "h": "Hoe je je vandaag voelt. Kies bij gevoelens alles wat past.",
   "q": [
    {
     "t": "s",
     "k": "st",
     "l": "Stemming",
     "v": [
      "Heel slecht",
      "Slecht",
      "Oké",
      "Goed",
      "Heel goed"
     ],
     "a": 1,
     "d": 3
    },
    {
     "t": "s",
     "k": "en",
     "l": "Energie",
     "v": [
      "Heel laag",
      "Laag",
      "Normaal",
      "Hoog",
      "Heel hoog"
     ],
     "a": 1,
     "d": 3
    },
    {
     "t": "c",
     "k": "mood",
     "l": "Gevoelens",
     "v": [
      "Vrolijk",
      "Rustig",
      "Energiek",
      "Zelfverzekerd",
      "Prikkelbaar",
      "Gespannen",
      "Somber",
      "Huilerig",
      "Onzeker"
     ]
    }
   ]
  },
  {
   "t": "Zelfbeeld",
   "c": "eis",
   "o": 1,
   "h": "Hoe je je vandaag over je uiterlijk en je lichaam voelt.",
   "q": [
    {
     "t": "s",
     "k": "zb",
     "l": "Je mooi voelen",
     "v": [
      "Niet mooi",
      "Matig",
      "Neutraal",
      "Mooi",
      "Heel mooi"
     ],
     "a": 1,
     "d": 3
    },
    {
     "t": "s",
     "k": "seks",
     "l": "Zin in seks",
     "v": [
      "Weinig",
      "Gemiddeld",
      "Veel"
     ],
     "a": 1,
     "d": null
    }
   ]
  },
  {
   "t": "Slaap",
   "c": "p4",
   "o": 1,
   "h": "Hoe je afgelopen nacht hebt geslapen.",
   "q": [
    {
     "t": "s",
     "k": "sl",
     "l": "Slaapkwaliteit",
     "v": [
      "Heel slecht",
      "Slecht",
      "Oké",
      "Goed",
      "Heel goed"
     ],
     "a": 1,
     "d": 3
    },
    {
     "t": "h",
     "k": "uur",
     "l": "Uren geslapen"
    }
   ]
  },
  {
   "t": "Leefstijl",
   "c": "fsh",
   "o": 0,
   "h": "Optioneel. Beweging en stress kunnen je stemming en klachten beïnvloeden.",
   "q": [
    {
     "t": "s",
     "k": "bew",
     "l": "Beweging",
     "v": [
      "Geen",
      "Licht",
      "Gesport"
     ],
     "a": 0,
     "d": null
    },
    {
     "t": "s",
     "k": "stress",
     "l": "Stress",
     "v": [
      "Weinig",
      "Gemiddeld",
      "Veel"
     ],
     "a": 1,
     "d": null
    }
   ]
  },
  {
   "t": "Notities",
   "c": "pre",
   "o": 0,
   "h": "Gebeurtenissen, medicijnen of vragen voor de huisarts.",
   "q": [
    {
     "t": "n",
     "k": "n"
    }
   ]
  }
 ],
 "typischPerFase": {
  "men": [
   "Krampen",
   "Moe",
   "Rugpijn",
   "Opgeblazen buik"
  ],
  "fol": [
   "Energiek",
   "Vrolijk",
   "Zelfverzekerd"
  ],
  "eis": [
   "Middenpijn",
   "Zelfverzekerd",
   "Energiek"
  ],
  "lut": [
   "Trek in zoet",
   "Rustig",
   "Moe"
  ],
  "pre": [
   "Gevoelige borsten",
   "Opgeblazen buik",
   "Puistjes",
   "Hoofdpijn",
   "Moe",
   "Trek in zoet",
   "Prikkelbaar",
   "Gespannen",
   "Somber"
  ]
 }
}
```

---

## 9. Teksten voor het tabblad Weten

### Je mooi of minder mooi voelen

Het gevoel er goed of juist slecht uit te zien schommelt bij veel vrouwen mee met de cyclus. Het gaat om hoe je je *voelt*, niet om hoe je er *uitziet*: je lichaam verandert in een maand nauwelijks, je beleving ervan wel.

**Rond de eisprong: je aantrekkelijker voelen**

In de grootste studie tot nu toe hielden 872 vrouwen (580 zonder hormonale anticonceptie) 70 dagen een dagboek bij. Vrouwen met een natuurlijke cyclus voelden zich vóór en rond de eisprong duidelijk aantrekkelijker en begeerlijker.

- Sterkste effect: je seksueel begeerlijk voelen, daarna je aantrekkelijk voelen.
- Zelfvertrouwen en positieve stemming stegen ook, maar minder.
- Hoeveel moeite vrouwen voor hun uiterlijk deden, veranderde niet duidelijk.
- Bij gebruiksters van de pil of een ander hormonaal middel bleef de stijging uit.

In een kledingonderzoek met 30 vrouwen kozen beoordelaars in 59,5% van de gevallen de foto uit de vruchtbare fase als die waarop de vrouw ‘meer haar best deed’. Meer dan toeval (50%), maar subtiel, en de groep was klein.

**Wat níét overeind bleef.** Claims dat vrouwen rond de eisprong bijvoorbeeld ‘mannelijkere’ mannen verkiezen, hielden in grote herhalingsstudies grotendeels geen stand. Meer zin in seks en je aantrekkelijker voelen zijn veel beter onderbouwd.

**Rond de menstruatie: je minder mooi voelen**

Vóór en tijdens de menstruatie is de ontevredenheid over het lichaam het grootst. Oorzaken die elkaar versterken:

- **Vocht vasthouden en een opgeblazen buik**: kleding zit strakker.
- **Huid**: 44% van 400 vrouwen meldde een acne-opvlamming vóór de menstruatie.
- **Stemming en moeheid**: wie zich somber voelt, kijkt kritischer naar zichzelf.
- **Minder zin in sporten en meer trek**, wat bij sommigen schaamte geeft.

Belangrijk: in onderzoek veranderde de **waargenomen lichaamsomvang niet**. Het gevoel is echt, maar tijdelijk, en zegt weinig over hoe je er werkelijk uitziet. Neem op die dagen geen grote beslissingen over je uiterlijk of eetpatroon. Ben je gevoelig voor eetproblemen, let dan juist dan extra op compenseren (maaltijden overslaan, overmatig sporten) en bespreek het met je huisarts of behandelaar.

### Vrolijk of somber

Het populaire beeld is dat ‘alle vrouwen’ vóór de menstruatie slechter gehumeurd zijn. Een review uit 2012 bekeek 47 studies waarin vrouwen hun stemming dagelijks bijhielden:

Toon dit als horizontale balken (47 studies = 100%):

| Uitkomst | Aandeel |
|---|---|
| Geen verband tussen stemming en cyclusfase | 38,3% (18 studies) |
| Somberder premenstrueel én in een andere fase (vaak de menstruatie) | 38,3% (18 studies) |
| Somberder alléén premenstrueel | 14,9% (7 studies) |
| Somberder alleen in een andere fase | 8,5% (4 studies) |

De onderzoekers vonden geen duidelijk bewijs voor een premenstrueel ‘slecht humeur’ dat voor alle vrouwen geldt. Als deelnemers wisten dat het onderzoek over de cyclus ging, werd vaker een premenstrueel patroon gevonden. Nieuwere dagboekstudies zien wel vaker iets meer negatieve stemming vóór en tijdens de menstruatie, en iets meer positieve stemming halverwege. Samengevat: **gemiddeld een kleine schommeling, met grote verschillen tussen vrouwen.**

**De kracht van verwachtingen**

In een experiment uit 1977 zaten alle vrouwen midden in hun cyclus. Wie te horen kreeg dat ze premenstrueel was, meldde toch meer klachten. Vrouwen schrijven een slechte bui in de premenstruele week ook vaker aan hun cyclus toe, en een goede bui aan hun omstandigheden. Dat maakt klachten niet minder echt, maar een dagboek helpt om te zien wat er werkelijk speelt.

**Wie is extra gevoelig?**

- Vrouwen met PMS of PMDD.
- Vrouwen met een bestaande depressie of angststoornis, bij wie klachten vóór de menstruatie vaak verergeren.
- Vrouwen in de perimenopauze, als de hormonen grilliger gaan schommelen.

### PMS, PMDD en premenstruele verergering

Toon dit als vergelijkingstabel (op de telefoon gestapeld per rij):

| | Normale schommeling | PMS | PMDD | Verergering (PME) |
|---|---|---|---|---|
| Wat is het? | Lichte veranderingen in stemming, energie en lichaam | Lichamelijke en/of psychische klachten die elke cyclus terugkomen | Ernstige, vooral psychische vorm; erkende diagnose (DSM-5, ICD-11) | Een bestaande aandoening, zoals depressie, wordt vóór de menstruatie erger |
| Wanneer? | Rond de menstruatie en de dagen ervoor | Na de eisprong; weg kort na het begin van de menstruatie, daarna minstens een week klachtenvrij | Laatste week vóór de menstruatie, in de meeste cycli | Klachten zijn er óók buiten de premenstruele fase |
| Hoe ernstig? | Hinderlijk, maar het leven gaat door | Zo erg dat je gewone dagelijkse dingen niet lukken | Duidelijke lijdensdruk, hinder in werk en relaties | Hangt af van de onderliggende aandoening |
| Hoe vaak? | Heel veel vrouwen | Ongeveer 5 op de 100 vrouwen van 15–45 jaar | Ongeveer 1,6 tot 3,2 op de 100 | 58% van vrouwen met een actuele depressie (één onderzoek) |
| Wat helpt? | Leefstijl, rust inplannen, dagboek | Leefstijl, cognitieve gedragstherapie, pil, SSRI | SSRI, pil; bij ernstige klachten soms GnRH-analogen | Behandeling van de aandoening, met aandacht voor de cyclus |


**PMS**

Volgens Thuisarts spreek je van PMS als de klachten zo erg zijn dat je je gewone dagelijkse dingen niet kunt doen. Milde klachten hebben veel vrouwen; echte PMS veel minder. Wereldwijde schattingen lopen uiteen van 12% tot 98% door verschillende definities. Er is geen bloedtest: de diagnose wordt gesteld met een klachtendagboek.

**PMDD**

- Minstens vijf symptomen in de laatste week vóór de menstruatie, in de meeste cycli van het afgelopen jaar.
- Minstens één kernsymptoom: duidelijke stemmingswisselingen, prikkelbaarheid of boosheid, somberheid of hopeloosheid, of angst en gespannenheid.
- Verder bijvoorbeeld minder interesse, concentratieproblemen, moeheid, eetbuien, slaapproblemen, overweldigd voelen, lichamelijke klachten.
- Duidelijke hinder in werk en relaties, bevestigd door twee cycli dagelijks bijhouden.

Hoe vaak, volgens een meta-analyse van 44 studies (2024):

**1,6%**algemene bevolking, strikte criteria
**3,2%**bevestigde diagnose met dagboek
**7,7%**voorlopige diagnose zonder dagboek

**PMDD is ernstig.** In een onderzoek onder 110 vrouwen met PMDD had 39% op dat moment gedachten aan zelfdoding. Herken je dit? Praat met je huisarts, of neem contact op met 113 Zelfmoordpreventie: **0800-0113** (gratis) of [113.nl](https://www.113.nl).

**Premenstruele verergering (PME)**

Hier verergert een bestaande aandoening vóór de menstruatie: duidelijk aangetoond bij depressie, paniekstoornis, eetstoornissen, psychotische stoornissen en borderline. Het verschil met PMS en PMDD: er zijn ook buiten de premenstruele dagen klachten. Psychische klachten op bijna elke dag van de maand wijzen niet op PMS; ga dan naar de huisarts.

**Behandeling**

- **SSRI’s** verminderen de klachten waarschijnlijk beter dan een placebo, dagelijks of alleen in de tweede helft van de cyclus (Cochrane, 2024). Ze nemen ze meestal niet helemaal weg.
- **De pil**, eventueel doorgeslikt, vlakt de schommelingen af. Helpt niet bij iedereen; evalueer na drie maanden.
- **Cognitieve gedragstherapie** helpt om beter met de klachten om te gaan.
- Bij ernstige klachten soms **GnRH-analogen** via de gynaecoloog.

### Belangrijke nuances

**De verschillen tussen vrouwen zijn groter dan het gemiddelde**

Slechts 13% van ruim 600.000 cycli duurde precies 28 dagen; gemiddeld was dat 29,3 dagen. Het verschil zit vooral in het moment van de eisprong; de periode daarna is vrij constant (12 à 14 dagen). Sommige vrouwen merken vrijwel niets van hun cyclus, anderen voelen elke fase.

**Hormonale anticonceptie verandert het patroon**

Combinatiepillen onderdrukken de eisprong en houden de hormonen vlak; de pieken verdwijnen grotendeels. Bloedverlies in de stopweek is geen echte menstruatie. Bij de meeste vrouwen heeft de pil geen negatief effect op de stemming. In een Deens onderzoek onder ruim een miljoen vrouwen hing hormonale anticonceptie samen met iets vaker een eerste antidepressivum (2,2 tegenover 1,7 per 100 vrouwen per jaar), vooral bij tieners. Dat bewijst geen oorzaak. Merk je dat je stemming achteruitgaat, bespreek het met je huisarts.

**Perimenopauze**

Vanaf ongeveer 40 jaar schommelen de hormonen grilliger. De kans op depressieve klachten is dan ongeveer 1,5 tot 2 keer zo groot als ervoor, vooral bij vrouwen die eerder PMDD of stemmingsklachten hadden.

**Wees kritisch op apps en sociale media**

Veel onderzoek is klein. De beste studies bevestigen kleine effecten op zin in seks en je aantrekkelijk voelen; spectaculairdere claims zijn grotendeels niet herhaald. Geen app weet hoe jij je ‘zou moeten’ voelen, ook deze niet.

### Omgaan met de mindere dagen

- **Houd je cyclus bij**, minstens twee à drie cycli. Dit is de belangrijkste stap.

- **Plan rond je patroon**: belangrijke afspraken in je betere dagen, rust vóór en aan het begin van de menstruatie. Leg je omgeving uit wat er speelt.

- **Beweeg regelmatig**: minstens acht weken trainen verminderde in onderzoek de PMS-klachten duidelijk, en helpt ook tegen menstruatiepijn en slecht slapen.

- **Slaap**: vaste tijden, en een koele slaapkamer in de tweede helft van de cyclus.

- **Eet regelmatig en gevarieerd**. Minder zout, cafeïne en alcohol kan helpen. Calcium is het best onderzochte supplement; bespreek het eerst met je huisarts.

- **Pijn**: neem een ontstekingsremmende pijnstiller (zoals ibuprofen) vroeg, bij de eerste pijn. Warmte helpt ook.

- **Psychologische hulp**: cognitieve gedragstherapie wordt in Nederlandse richtlijnen genoemd.

### Wanneer naar de huisarts?

- Klachten verstoren elke maand je werk, studie, relaties of dagelijks leven.

- Je bent (bijna) de hele maand somber, angstig of prikkelbaar.

- Ernstige menstruatiepijn die niet reageert op pijnstillers of steeds erger wordt (denk ook aan endometriose).

- Zeer hevig bloedverlies, bloedingen tussendoor, of een sterk onregelmatige of uitblijvende menstruatie (doe bij uitblijven eerst een zwangerschapstest).

- Je stemming verandert duidelijk na het starten van een anticonceptiemiddel.

- Je bent 40 tot 50 en krijgt nieuwe of heftigere stemmingsklachten.

- Je slikt de pil tegen PMS en merkt na drie maanden geen verbetering.

**Denk je aan zelfdoding?** Zoek direct hulp: bel je huisarts of de huisartsenpost, of 113 Zelfmoordpreventie: **0800-0113** (gratis, dag en nacht) of chat via [113.nl](https://www.113.nl). Bij direct gevaar: **112**.

### Bronnen

- Thuisarts: [Premenstrueel syndroom](https://www.thuisarts.nl/premenstrueel-syndroom/ik-heb-premenstrueel-syndroom) en [PMS en de pil](https://www.thuisarts.nl/premenstrueel-syndroom/ik-heb-pms-en-ga-daarvoor-pil-slikken)

- De Gynaecoloog (NVOG): [Premenstrueel syndroom](https://www.degynaecoloog.nl/onderwerpen/premenstrueel-syndroom-pms/); [Richtlijn PMS](https://www.nvog.nl/wp-content/uploads/2020/06/Richtlijn-Premenstrueel-Syndroom.pdf)

- Bull e.a. (2019), ruim 600.000 cycli, [npj Digital Medicine](https://www.nature.com/articles/s41746-019-0152-7)

- Schleifenbaum e.a. (2021), dagboekstudie 872 vrouwen, [Evolutionary Human Sciences](https://www.cambridge.org/core/journals/evolutionary-human-sciences/article/women-feel-more-attractive-before-ovulation-evidence-from-a-largescale-online-diary-study/1E25337DA0BD0AECFA976F7A736AF6B4)

- Jappe & Gardner (2009), lichaamsbeeld per cyclusfase, [Perceptual and Motor Skills](https://doi.org/10.2466/pms.108.1.74-80)

- Roney & Simmons (2013), hormonen en seksueel verlangen, [Hormones and Behavior](https://labs.psych.ucsb.edu/roney/james/roneysimmons.published.pdf)

- Haselton e.a. (2007), kleding rond de eisprong, [PubMed](https://pubmed.ncbi.nlm.nih.gov/17045994/)

- Romans e.a. (2012), stemming en de cyclus, [review van 47 studies](https://www.researchgate.net/publication/233404666_Mood_and_the_Menstrual_Cycle)

- Nolan & Hughes (2022), [premenstruele verergering](https://www.researchgate.net/publication/362201784_Premenstrual_exacerbation_of_mental_health_disorders_a_systematic_review_of_prospective_studies)

- Cochrane (2024), [SSRI’s bij PMS en PMDD](https://www.cochranelibrary.com/cdsr/doi/10.1002/14651858.CD001396.pub4/full)

- Skovlund e.a. (2016), [anticonceptie en depressie](https://jamanetwork.com/journals/jamapsychiatry/fullarticle/2552796)

- Pearce e.a. (2020), [beweging bij PMS](https://research.birmingham.ac.uk/en/publications/exercise-for-premenstrual-syndrome-a-systematic-review-and-meta-a/)

- Thys-Jacobs e.a. (1998), [calcium en PMS](https://pubmed.ncbi.nlm.nih.gov/9731851/)

- FIGO (Munro e.a., 2018), [normale waarden voor de menstruatiecyclus](https://www.msdmanuals.com/professional/multimedia/table/normal-menstrual-parameters)

- Reilly e.a. (2024), vóórkomen van PMDD, [nieuwsbericht](https://medicalxpress.com/news/2024-01-prevalence-premenstrual-dysphoric-disorder.html)

---

## 10. Veiligheid en zorgvuldigheid

- Toon onderaan elk tabblad: "Deze app beschrijft gemiddelde patronen bij een natuurlijke cyclus en vervangt geen persoonlijk medisch advies."
- Toon bij voorspellingen altijd dat het een schatting is en **niet geschikt als anticonceptie**.
- Het 113-blok (0800-0113, 113.nl, bij direct gevaar 112) staat in de secties PMS/PMDD en Naar de huisarts. Maak het telefoonnummer tikbaar (`tel:08000113`).
- Geen account, geen tracking, geen gegevens naar een server. Zeg dat in de app ("Je gegevens blijven op dit apparaat.").
- Geen beweringen over gewicht of diëten; formuleer zelfbeeld als "hoe je je voelt", niet als hoe je eruitziet.

---

## 11. Extra's voor de telefoon-app (optioneel)

- **Herinneringen** met `expo-notifications`: dagelijks "Vul je dagboek in" (tijd instelbaar, standaard 21:00) en twee dagen voor de verwachte menstruatie "Je menstruatie komt waarschijnlijk over 2 dagen". Standaard uit; vraag eerst toestemming.
- **App-vergrendeling** met `expo-local-authentication` (vingerafdruk/gezicht), standaard uit.
- **Import** van een eerder gedeelde CSV (zelfde kolommen) om naar een nieuwe telefoon over te zetten.

---

## 12. Controlelijst voor oplevering

- [ ] Vier tabbladen werken in licht en donker thema, ook op een klein scherm (360 px breed) zonder afgekapte teksten.
- [ ] Bij 28 dagen kloppen de fasen precies met 6.2; bij andere lengtes schuift de eisprong mee (L − 14).
- [ ] Startdag toevoegen, vervangen (binnen 14 dagen) en verwijderen werkt met "Ongedaan maken".
- [ ] Dagboek: optionele schalen zijn te wissen; samenvattingen per blok kloppen; automatische nieuwe cyclus werkt.
- [ ] Voorbeeldgegevens verdwijnen na de eerste eigen opgeslagen dag.
- [ ] Gemiddelden per fase en de "Jij"-regel verschijnen pas bij ≥ 3 dagen in een fase.
- [ ] Alle teksten in het Nederlands, getallen met een komma (3,8).
- [ ] Afbeeldingen lokaal in `assets/images/` (`hero.jpg`, `fase-*.jpg`, `blok-*.jpg`); app-icoon en splash met `icon.png` (zon op #502039).
