## h6 Fuzzy

Raporttitehtävät Tero Karvisen kurssille - Tunkeutumistestaus ICI005AS3A-3007.

Syksy 2026.

### Ympäristö

VBox Kali GNU/Linux

Ryzen 5

## x) Lue ja tiivistä
**[Hoikkala 2026: Fuzzing with Fuff](https://terokarvinen.com/tunkeutumistestaus/hoikkala-2026-fuzzing-with-ffuf.pdf)**
- HTTP bruteforce-työkalu
- Fuzzaus kaikkiin HTTP osiin


---
## a) Tallenna itsellesi kopio säännöistä. Kirjoita omin sanoin,

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
$ wget https://github.com/ffuf/ffuf/releases/download/v2.3.0/ffuf_2.3.0_linux_amd64.tar.gz
$ tar -xf ffuf_2.3.0_linux_amd64.tar.gz
$ rm ffuf_2.3.0_linux_amd64.tar.gz
```

**Tarkistettiin versio ja preflight vaatimus**

<img width="276" height="102" alt="kuva" src="https://github.com/user-attachments/assets/5b9b1e93-c7a8-4735-a194-2f1f8350b865" />


---
## Vaultline

**Ladattiin sanakirjat harjoitusta varten**

    curl -O https://ffuf.io.fi/wordlists/content.txt
    curl -O https://ffuf.io.fi/wordlists/passwords.txt

**Testattiin toimivuus**

    ffuf -w content.txt -u https://ffuf.io.fi/FUZZ

--
## c1) Content discovery (Vaultline https://ffuf.io.fi/play tehtävät on numeroitu näin, käytetään tässä samoja.).

**Goal: Find the paths that exist but are not linked from anywhere.**

Kokeiltiin **`-ac`**-lippua, mikä autokalibroi filttereitä.

<img width="745" height="600" alt="kuva" src="https://github.com/user-attachments/assets/7b857d66-1b59-4cff-9a32-7471af1d83e3" />

Komento tuotti HTTP-pyynnöt kuten **admin, login ja files.**

>_Huomattiinkin myöhemmin, että on käytetty vanhempaa ffuf-versiota kuin äsken asennettu. Syynä oli unohdettu käyttää **`./ffuf`**-komentoa, mutta harjoituksessa ei vielä vaadittu preflightia._


---
## c2) The interesting non-200

**Goal: Two planted paths do not answer 200. One of them a default run will not even consider.**

Ajettiin ensin **`-mc all`**-matcheriä (Match status codes). Se johti suureen määrään tulokseen.

    $ ffuf -w content.txt -u https://ffuf.io.fi/FUZZ -mc all

Filtteröitiin Status Code 200 matchit **`-fc 200`**.

    $ ffuf -w content.txt -u https://ffuf.io.fi/FUZZ -mc all -fc 200

<img width="747" height="125" alt="kuva" src="https://github.com/user-attachments/assets/72a96c78-275c-4e6a-a25b-ae295c346ee3" />


---
## c3) Recursion

**Goal: The wordlist holds names, not paths, so C1 found you 13 things and none of them nested. Descending finds more.**

Ajettiin komento ffuf [Recursion-ohjeiden](https://github.com/ffuf/ffuf/wiki/Recursion#depth) mukaan.

    $ ./ffuf -w content.txt -u https://ffuf.io.fi/FUZZ -mc all -fw 135 -recursion -recursion depth 2

<img width="719" height="272" alt="kuva" src="https://github.com/user-attachments/assets/0461688d-b0d2-461c-ba4f-a2baed3c78ec" />

**[Info]-tulosteissa** näkyy kun ffuf lisäsi jonoon fuzzaukset **`Adding a new job...`** ja **`Starting queued job...`**.

<img width="687" height="120" alt="kuva" src="https://github.com/user-attachments/assets/929099af-d6d6-4e37-a44b-3fca2f6a2cdb" />


---
## c4) Virtual hosts

**Goal: Three hostnames under ffuf.io.fi serve different content from this same address. Find all three.**




---
## c9) The login you cannot replay (Has preflight! Has CSRF token!)


---
## Lähteet

[CLI flags](https://github.com/ffuf/ffuf/wiki/CLI-flags).

[Harjoitukset](https://ffuf.io.fi/play).

[Hoikkala 2026: Fuzzing with Fuff](https://terokarvinen.com/tunkeutumistestaus/hoikkala-2026-fuzzing-with-ffuf.pdf).

[Performance and rate](https://github.com/ffuf/ffuf/wiki/Performance-and-rate#rate-limiting).

[Recursion](https://github.com/ffuf/ffuf/wiki/Recursion#depth).
