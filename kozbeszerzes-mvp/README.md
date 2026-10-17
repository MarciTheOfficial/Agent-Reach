# Tenderpilóta – közbeszerzés-figyelő MVP

Kattintható prototípus magyar közbeszerzésekhez, **kitalált példaadatokkal**. Arra készült, hogy leendő ügyfeleknek (pályázó cégeknek és közbeszerzési tanácsadóknak) meg lehessen mutatni, és ki lehessen próbálni velük, hogy fizetnének-e érte.

Egyetlen önálló fájl: [`index.html`](index.html). Nincs build, nincs backend.

## Mit tud

| Fül | Funkció |
|---|---|
| **Cégkereső** | Adószám vagy cégnév alapján azonosítja a céget (az adószámot az ellenőrző számjeggyel validálja), megmutatja a cégadatlapot, a főtevékenységből (TEÁOR) és a korábbi nyertes eljárásokból közbeszerzési profilt képez, és kilistázza, hol lenne az első a várható ajánlattevők közül. Ismeretlen cégnél kézzel vagy AI-besorolással (egy mondatos leírásból) is működik. Az eredmény ideiglenes profil, ami egy kattintással ügyfélként menthető. |
| **Tenderek** | Nyitott kiírások listája 0–100 pontos illeszkedési pontszámmal. Szűrés kulcsszóra, ágazatra (CPV), helyszínre, eljárásrendre, határidőre és értékre. |
| **Tender részletei** | Pontszám-bontás (CPV, helyszín, méret, alkalmassági feltételek), alkalmassági feltételenkénti automatikus ellenőrzés, értékelési szempontok, és hogy kik nyertek korábban ennél az ajánlatkérőnél. |
| **AI-elemzés** | AI-összefoglaló a kiírásról, valamint „Megfelelünk?” értékelés (indulj / konzorciummal / ne indulj, feltételenkénti indoklással). |
| **Ajánlat-tervezet** | A cégprofilból személyre szabott tervezet: kísérőlevél, megvalósítási koncepció, alkalmasság igazolása, vállalások, dokumentumlista, kockázatok. Készülhet AI-jal vagy AI nélkül, sablonból. Másolható és letölthető (.md). |
| **Versenytársak** | Piaci mutatók (egyajánlatos arány, átlagos ajánlattevőszám, ár a becsült érték arányában), elnyert érték cégenként, figyelőlista automatikus megfigyelésekkel. |
| **Napi riport** | A reggeli e-mail előnézete: új illeszkedő kiírások, lejáró határidők, versenytársak eredményei. A küldés még nincs bekötve. |
| **Cégprofil** | CPV-kódok, célterület, tanúsítványok, szakemberek, referenciák szerkesztése. Ebből számol a pontozás és ezt kapja az AI. |
| **Ügyfelek** | Tanácsadói nézet: az összes cégprofil egy helyen, köztük hány kiírásban áll az első helyen. Új ügyfél adószám alapján vehető fel. |

## Hogyan működik

- **Pontozás (szabályalapú, AI nélkül):** CPV-egyezés (max. 40), helyszín (max. 20), a becsült érték és az árbevétel aránya (max. 15), alkalmassági feltételek (max. 25). A feltételeket típusonként ellenőrzi: referencia (CPV-előtag, minimális érték, évek száma), szakember, tanúsítvány és árbevétel. A többit „ellenőrizendőnek” jelöli.
- **Nyerési esély:** minden kiírásnál a cég és a példa-cégadatbázis többi cége közül azok kerülnek a „várható ajánlattevők” közé, akiknek az illeszkedése legalább 50 pont. A versenytársak pontszáma nyilvános adatokból becsült (tevékenység, székhely, árbevétel, korábbi nyertes eljárások); a szakember- és tanúsítványfeltételeiket semlegesnek veszi. A helyezés pontszáma: illeszkedés × 0,9, +6 pont annak, aki már nyert ennél az ajánlatkérőnél, és +4 pont az előző hasonló (azonos CPV-osztályú) szerződés nyertesének. Ha a cégnek önállóan hiányzik egy alkalmassági feltétel, a rendszer partnerjelöltet javasol azok közül, akik teljesítik.
- **AI:** a claude.ai-on megnyitva az oldal a néző saját Claude-fiókján keresztül hív AI-t. Az első használatkor engedélyt kér. AI nélkül (például helyben megnyitva) a pontozás és a sablon-tervezet működik.
- **Mentés:** a profilok, a figyelőlista, a riport-beállítások és a tervezetek a böngésző helyi tárolójában maradnak. Nem szinkronizálódnak, és mások nem látják őket.

## Példaadatok

3 cégprofil (útépítő, IT-cég, közétkeztető), egy 17 cégből álló példa-cégadatbázis, 17 nyitott kiírás, 36 korábbi eredményhirdetés. **Minden cég, ajánlatkérő és kiírás kitalált**, a CPV- és NUTS-kódok viszont valódiak. A példa-adószámok 9999-cel kezdődnek, és átmennek az ellenőrző számjegy vizsgálatán. A TEÁOR-besorolás szemléltető. A dátumok a megnyitás napjához képest számolódnak, így a demó nem „avul el”.

## Következő lépések

1. **Valódi cégadatok a Cégkeresőhöz:** alapadatok és tevékenység az ingyenes e-cégjegyzékből (név, adószám vagy cégjegyzékszám alapján), árbevétel és létszám az e-beszámoló portálon közzétett beszámolókból, vagy mindez egy helyen az Opten előfizetéses szolgáltatásával (az API-hozzáférés feltételeit velük kell egyeztetni). Az Opten weboldalának automatikus letöltése a felhasználási feltételeikbe ütközhet, ezért az csak hivatalos hozzáféréssel jöhet szóba.
2. **Valódi közbeszerzési adatok:** az EU-s TED adatbázis lekérdezése magyar kiírásokra (hivatalos API) és az EKR közzétett hirdetményeinek feldolgozása. Ezekből ugyanilyen szerkezetű `TENDERS` és `AWARDS` lista készül.
3. **Az alkalmassági feltételek kinyerése** a kiírások szövegéből AI-jal, hogy a pontozás valódi kiírásokon is működjön.
4. **Felhasználói fiókok és szerveroldali tárolás**, valamint valódi e-mail-küldés a napi riporthoz.
5. **Validálás:** a prototípus bemutatása 10 közbeszerzési tanácsadónak vagy pályázó cégnek, mielőtt további fejlesztés indul.
