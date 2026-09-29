## h6 Fuzzy

Raporttitehtävä Tero Karvisen kurssille - Tunkeutumistestaus ICI005AS3A-3007.

Syksy 2026.

### Ympäristö

VBox Kali GNU/Linux

Ryzen 5

## x) Lue ja tiivistä
**[Hoikkala 2026: Fuzzing with Fuff](https://terokarvinen.com/tunkeutumistestaus/hoikkala-2026-fuzzing-with-ffuf.pdf)**
- HTTP bruteforce-työkalu
- Fuzzaus kaikkiin HTTP osiin


---
## a) Tallenna itsellesi kopio säännöistä.

**Scope. Mikä on kohde?**

Kohteena on ffuf-host.
    
**Rules of engagement. Mitä sille saa tehdä, eli mitä tai millaisia menetelmiä saa käyttää?**

Kohdetta saa fuzzata harjoitusta varten.

**Mihin oikeutesi tehdä tietoturvatestausta tähän kohteeseen perustuu?**

Kohteen omistajan antama lupa.

**Riskit ja mitigointi. Tuo palvelin on Internetissä. Tunnista lyhyesti riskit ja niiden mitigointi ennen käytännön harjoittelua.**

Riskinä on palvelimen kuormitus suurilla pyyntömäärillä, joten niitä pitää rajoittaa.

Missä harjoituksessa kannattaa rajoittaa rate-komennolla on kuitenkin subjektiivista, ellei halua olla ns. piilossa. Oletus rate on 0 = unlimited.

---
## b) Asenna ffuf versio, joka tukee aivan uutta preflight-ominaisuutta.

**Asennettiin uusin ffuf**

```bash
wget https://github.com/ffuf/ffuf/releases/download/v2.3.0/ffuf_2.3.0_linux_amd64.tar.gz
tar -xf ffuf_2.3.0_linux_amd64.tar.gz
rm ffuf_2.3.0_linux_amd64.tar.gz
```

**Tarkistettiin versio ja preflight**

<img width="276" height="102" alt="kuva" src="https://github.com/user-attachments/assets/5b9b1e93-c7a8-4735-a194-2f1f8350b865" />

**Ladattiin sanakirjat harjoitusta varten**

    curl -O https://ffuf.io.fi/wordlists/content.txt
    curl -O https://ffuf.io.fi/wordlists/passwords.txt

**Testattiin toimivuus**

    ffuf -w content.txt -u https://ffuf.io.fi/FUZZ

Tuli suuri määrä HTTP-pyyntöjä.

---
## c1) Content discovery (Vaultline https://ffuf.io.fi/play tehtävät on numeroitu näin, käytetään tässä samoja.).

**Goal: Find the paths that exist but are not linked from anywhere.**

Kokeiltiin **`-ac`**-lippua, mikä autokalibroi filttereitä.

<img width="745" height="600" alt="kuva" src="https://github.com/user-attachments/assets/7b857d66-1b59-4cff-9a32-7471af1d83e3" />

Komento tuotti HTTP-pyynnöt kuten **admin, login ja files.**

>_Huomattiinkin myöhemmin, että on käytetty vanhempaa ffuf-versiota kuin äsken asennettu. Syynä oli unohdettu käyttää **`./ffuf`**-komentoa, mutta harjoituksessa ei vielä vaadittu preflightia._


---
## c2) The interesting non-200

**Goal: Two planted paths do not answer 200. One of them a default run will not even consider.**

Ajettiin ensin **`-mc all`**-matcher (Match status codes). Se johti suureen määrään tuloksia.

    ffuf -w content.txt -u https://ffuf.io.fi/FUZZ -mc all

Filtteröitiin Status Code 200 match **`-fc 200`**.

    ffuf -w content.txt -u https://ffuf.io.fi/FUZZ -mc all -fc 200

<img width="747" height="125" alt="kuva" src="https://github.com/user-attachments/assets/72a96c78-275c-4e6a-a25b-ae295c346ee3" />

Saatiin haluttu lopputulos.

---
## c3) Recursion

**Goal: The wordlist holds names, not paths, so C1 found you 13 things and none of them nested. Descending finds more.**

Ajettiin komento ffuf [Recursion-ohjeiden](https://github.com/ffuf/ffuf/wiki/Recursion#depth) mukaan.

    ./ffuf -w content.txt -u https://ffuf.io.fi/FUZZ -mc all -fw 135 -recursion -recursion depth 2

**`-fw`** = Filtteröidään sanamäärä.

**`-recursion`** = Hakemistot fuzzataan myös.

**`-recursion depth 2`** = Hakemiston syvyyden rajoitus 2 tasoon.

<img width="719" height="272" alt="kuva" src="https://github.com/user-attachments/assets/0461688d-b0d2-461c-ba4f-a2baed3c78ec" />

<img width="687" height="120" alt="kuva" src="https://github.com/user-attachments/assets/929099af-d6d6-4e37-a44b-3fca2f6a2cdb" />

**[Info]-tulosteissa** näkyy kun ffuf lisäsi jonoon fuzzaukset **`Adding a new job...`** ja **`Starting queued job...`**.

---
## c4) Virtual hosts

**Goal: Three hostnames under ffuf.io.fi serve different content from this same address. Find all three.**

Ajettiin ensin normaalisti.

    ./ffuf -w content.txt -u https://ffuf.io.fi/ -H "Host: FUZZ.ffuf.io.fi"

Saatiin suuri määrä tuloksia, jossa samat arvot.

<img width="737" height="212" alt="kuva" src="https://github.com/user-attachments/assets/d563b219-f8d1-4ddf-bb3e-0ce85d86fdde" />

Filtteröitiin ohjeen mukaan **`-fw 377`** ja kokeiltiin **`-rate`**-rajoitusta.

    ./ffuf -w content.txt -u https://ffuf.io.fi/ -H "Host: FUZZ.ffuf.io.fi" -fw 377 -rate 50

<img width="736" height="51" alt="kuva" src="https://github.com/user-attachments/assets/26386b58-bc06-4a8e-9da5-020355522ce4" />

Saatiin vain admin osoite. Kokeiltu eri filttereillä ja jopa per-host calibration **`-ach`** eikä saatu muut 2 virtual hostia.

    ./ffuf -w content.txt -u https://ffuf.io.fi/ -H "Host: FUZZ.ffuf.io.fi" -fw 377 -ach -rate 500
    

---
## c9) The login you cannot replay (Has preflight! Has CSRF token!)

**Goal: Get into the admin account. A plain password fuzz returns 403 forever, however long you run it.**

Tässä emme oikeen edennyt, joten suuntauduttiin Vaulline-ohjeeseen **C9), spelled out**.

```bash

cat > login.raw <<'EOF'
GET /login HTTP/1.1
Host: ffuf.io.fi
Accept: text/html

EOF

ffuf -w passwords.txt -u https://ffuf.io.fi/login -X POST \
 -H "Content-Type: application/x-www-form-urlencoded" \
 -d "csrf_token=CSRFTOKEN&username=admin&password=FUZZ" \
 -preflight login.raw \
 -preflight-var 'CSRFTOKEN:name="csrf_token" value="([a-f0-9]+)"' \
 -preflight-mode per-request \
 -mc 302

```
Saatiin salasana.

<img width="737" height="45" alt="kuva" src="https://github.com/user-attachments/assets/d03c4179-1ced-4b8e-bfae-012a91e04602" />

Tässä läpikäynnissä oli hieman epäselvyyksiä, joten kysyimme ChatGPT:ltä tarkempaa selvennystä.

Lyhyesti ffuf hakee ensin CSRF-tokenin ennen jokaisen **`passwords.txt`** salasanojen yritystä ja palauttaa Status Coden 302 onnistuttua.

**`login.raw`** tiedosto = sen sisältö on ffufin esipyyntö eli preflight-request, mikä hakee **/login**-sivulta CSRF-tokenin ja käyttää sitä POST-pyynnössä.

**`-X POST`** = tekee fuzzauksen POST-pyyntöinä, kirjautuminen.

**`-H "Content-Type: application/x-www-form-urlencoded"`** = HTTP-headerin lisääminen.

**`-d "csrf_token=CSRFTOKEN&username=admin&password=FUZZ"`** = HTTP request body, POST-pyynnöllä lähetetty data. Salasanaa fuzzataan.

**`-preflight login.raw`** = Suoritetaan **login.raw** ennen fuzzaus-pyyntöjä.

**`-preflight-var 'CSRFTOKEN:name="csrf_token" value="([a-f0-9]+)"'`** = Etsitään **/login**-sivun HTML:ästä **name="csrf_token"** ja **value** a-f tai 0-9 otetaan talteen. Ffuf sitten antaa arvolle nimen **CSRFTOKEN**.

**`-preflight-mode per-request`** = Suoritetaan preflight jokaisen yrityksen jälkeen, jos CSRF-token vaihtuu.

**`-mc 302`** = Match Status Code 302.

---
## Lähteet

[ChatGPT](chatgpt.com). 2026. Hyödynnetty tehtävän c9 selvennyksessä.

Hoikkala, J. 2026. [CLI flags](https://github.com/ffuf/ffuf/wiki/CLI-flags). GitHub. Luettu: 30.9.2026.

Hoikkala, J. 2026. [Fuzzing with Fuff](https://terokarvinen.com/tunkeutumistestaus/hoikkala-2026-fuzzing-with-ffuf.pdf). Luettu: 29.9.2026.

Hoikkala, J. 2026. [Performance and rate](https://github.com/ffuf/ffuf/wiki/Performance-and-rate#rate-limiting). GitHub. Luettu: 29.9.2026.

Hoikkala, J. 2026. [Recursion](https://github.com/ffuf/ffuf/wiki/Recursion#depth). GitHub. Luettu: 29.9.2026.

Vaultline Oy. s.a. [How to play](https://ffuf.io.fi/play). Harjoitukset tehty 29.9.2026.
