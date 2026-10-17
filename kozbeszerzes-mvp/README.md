# Tenderpilóta – közbeszerzés-figyelő MVP

Kattintható prototípus magyar közbeszerzésekhez, **kitalált példaadatokkal**. Arra készült, hogy leendő ügyfeleknek (pályázó cégeknek és közbeszerzési tanácsadóknak) meg lehessen mutatni, és ki lehessen próbálni velük, hogy fizetnének-e érte.

Egyetlen önálló fájl: [`index.html`](index.html). Nincs build, nincs backend.

## Mit tud

| Fül | Funkció |
|---|---|
| **Tenderek** | Nyitott kiírások listája 0–100 pontos illeszkedési pontszámmal. Szűrés kulcsszóra, ágazatra (CPV), helyszínre, eljárásrendre, határidőre és értékre. |
| **Tender részletei** | Pontszám-bontás (CPV, helyszín, méret, alkalmassági feltételek), alkalmassági feltételenkénti automatikus ellenőrzés, értékelési szempontok, és hogy kik nyertek korábban ennél az ajánlatkérőnél. |
| **AI-elemzés** | AI-összefoglaló a kiírásról, valamint „Megfelelünk?” értékelés (indulj / konzorciummal / ne indulj, feltételenkénti indoklással). |
| **Ajánlat-tervezet** | A cégprofilból személyre szabott tervezet: kísérőlevél, megvalósítási koncepció, alkalmasság igazolása, vállalások, dokumentumlista, kockázatok. Készülhet AI-jal vagy AI nélkül, sablonból. Másolható és letölthető (.md). |
| **Versenytársak** | Piaci mutatók (egyajánlatos arány, átlagos ajánlattevőszám, ár a becsült érték arányában), elnyert érték cégenként, figyelőlista automatikus megfigyelésekkel. |
| **Napi riport** | A reggeli e-mail előnézete: új illeszkedő kiírások, lejáró határidők, versenytársak eredményei. A küldés még nincs bekötve. |
| **Cégprofil** | CPV-kódok, célterület, tanúsítványok, szakemberek, referenciák szerkesztése. Ebből számol a pontozás és ezt kapja az AI. |
| **Ügyfelek** | Tanácsadói nézet: az összes cégprofil egy helyen. |

## Hogyan működik

- **Pontozás (szabályalapú, AI nélkül):** CPV-egyezés (max. 40), helyszín (max. 20), a becsült érték és az árbevétel aránya (max. 15), alkalmassági feltételek (max. 25). A feltételeket típusonként ellenőrzi: referencia (CPV-előtag, minimális érték, évek száma), szakember, tanúsítvány és árbevétel. A többit „ellenőrizendőnek” jelöli.
- **AI:** a claude.ai-on megnyitva az oldal a néző saját Claude-fiókján keresztül hív AI-t. Az első használatkor engedélyt kér. AI nélkül (például helyben megnyitva) a pontozás és a sablon-tervezet működik.
- **Mentés:** a profilok, a figyelőlista, a riport-beállítások és a tervezetek a böngésző helyi tárolójában maradnak. Nem szinkronizálódnak, és mások nem látják őket.

## Példaadatok

3 cégprofil (útépítő, IT-cég, közétkeztető), 17 nyitott kiírás, 36 korábbi eredményhirdetés. **Minden cég, ajánlatkérő és kiírás kitalált**, a CPV- és NUTS-kódok viszont valódiak. A dátumok a megnyitás napjához képest számolódnak, így a demó nem „avul el”.

## Következő lépések

1. **Valódi adatok:** az EU-s TED adatbázis lekérdezése magyar kiírásokra (hivatalos API) és az EKR közzétett hirdetményeinek feldolgozása. Ezekből ugyanilyen szerkezetű `TENDERS` és `AWARDS` lista készül.
2. **Az alkalmassági feltételek kinyerése** a kiírások szövegéből AI-jal, hogy a pontozás valódi kiírásokon is működjön.
3. **Felhasználói fiókok és szerveroldali tárolás**, valamint valódi e-mail-küldés a napi riporthoz.
4. **Validálás:** a prototípus bemutatása 10 közbeszerzési tanácsadónak vagy pályázó cégnek, mielőtt további fejlesztés indul.
