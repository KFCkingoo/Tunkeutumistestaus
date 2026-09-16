## h4 Täysin Laillinen Sertifikaatti
Raporttitehtävät Tero Karvisen kurssille - Tunkeutumistestaus ICI005AS3A-3007.

Syksy 2026.

### Ympäristö

VBox Kali GNU/Linux

Ryzen 5

---
## x) Lue/katso ja tiivistä.
#### OWASP Top 10:2021 [A01:2021 – Broken Access Control](https://top10.owasp.org/2021/A01_2021-Broken_Access_Control/)
- 94% testatuista sovelluksista ollaan todettu haavoittuvaisista pääsynhallinnasta.
- Pääsynhallinta asettaa käyttäjille asetetut käyttöoikeudet.
 
#### PortSwigger Academy
#### [Insecure direct object references (IDOR)](https://portswigger.net/web-security/access-control/idor)
- Pääsynhallinnan haavoittuvuus, missä sovellus käyttää käyttäjän syöttöä objektin pääsyyn.
- Esimerkissä muutetaan parametreja. 


#### [Path traversal](https://portswigger.net/web-security/file-path-traversal)
- Directory traversal, hakemiston kulku(?)
- Haavoittuvuus antaa pääsyn hyökkääjän etsimään tietoon hakemistoa navigoimalla URL-syötteellä.


#### [Cross-site scripting](https://portswigger.net/web-security/cross-site-scripting) 
- Tietoturva haavoittuvuus, hyökkääjä tekee sovelluksesta haitallisen.
- Yleisesti saa käyttäjän käyttöoikeudet


| XSS | Kuvaus |
|---|---|
| **Reflected XSS** | Haitallinen skripti tulee nykyisestä HTTP-pyynnöstä. |
| **Stored XSS** | Haitallinen skripti tulee sivuston tietokannasta. |
| **DOM-based XSS** | Haavoittuvuus sijaitsee asiakaspuolen koodissa eikä palvelinpuolen koodissa. |



---

## a) Totally Legit Sertificate. 
#### Asenna OWASP ZAP ja käynnistä

    sudo apt install zaproxy
    zaproxy
    
#### Generoi CA-sertifikaatti
Generoitiin CA-sertifikaatti **`Tool -> Options -> Network -> Server Certificates`.** Tallennettiin CA-sertifikaatti.

<img width="741" height="580" alt="image" src="https://github.com/user-attachments/assets/463e35dd-c9a5-4704-a52c-59d90669b3bb" />

#### Asenna se selaimeen
Avattiin Firefox ja lisättiin CA-sertifikaatti. **`Firefox Settings -> Search "certificates" -> View Certificates... -> Import`**

<img width="792" height="310" alt="image" src="https://github.com/user-attachments/assets/b647dc40-8721-4f45-bb58-04e44a988ce6" />

<br>
Tarkistettiin, että sertifikaatti on asennettu.
<br>
<img width="632" height="77" alt="Näyttökuva 2026-09-15 161908" src="https://github.com/user-attachments/assets/0e337a39-e139-46cc-a30c-9dfdc55d0a9f" />


#### Laita ZAP proxyksi selaimeen
ZAP:illa pystyy suoraan avata selain konfiguroituna Manual Explore:sta.

<img width="748" height="816" alt="image" src="https://github.com/user-attachments/assets/ae200f03-b82d-492c-9e12-7596a1d2c948" />

Selain avattiin ZAP:illa suoraan ja haulla "proxy" näkyy, että selain on jo valmiiksi konfiguroitu proxyna. Konfiguroitiin kuitenkin manuaalisesti selaimeen samalla mallilla tehtävää varten.

`localhost` ei toiminut, joten piti starttaa apache.

     sudo systemctl start apache2.service


<img width="1217" height="181" alt="image" src="https://github.com/user-attachments/assets/56779181-bca5-424a-ac48-59f98e41de21" />

ZAP toimii selaimen proxyna.

#### Laita ZAP sieppaamaan myös kuvat
Laitettiin asetus päälle **`Tools -> Options -> Display -> Process images in HTTP requests/responses`**.

Response kohdan alapalkista näkyy myös pyydetty kuva _(kuvassa oikea alakulma)_.

<img width="847" height="412" alt="image" src="https://github.com/user-attachments/assets/a0a6029d-5d79-4686-a96c-58bbd308eaa0" />

<br>

Request kohdassa **`GET pyyntö openlogo-75.png-kuvasta.`**

<img width="1201" height="187" alt="image" src="https://github.com/user-attachments/assets/98eefbe6-69ae-4eb8-8344-85258d5ffc47" />


---
## b) Kettumaista.  

#### Asenna "FoxyProxy Standard" Firefox Addon, ja lisää ZAP proxyksi siihen.
Asennettu Firefoxin laajennusten kautta ja lisätty ZAP proxyna.

    Title: ZAP
    Hostname: localhost
    Port: 8080


#### Käytä FoxyProxyn "Patterns" -toimintoa, niin että vain valitsemasi weppisivut ohjataan Proxyyn. (Läksyssä ohjataan varmaankin PortSwigger Labs ja localhost.)
Lisättiin filtterit **`Proxy by Patterns`** painikkeesta ja lisättiin sinne:

    http://localhost/*               #localhost
    *.web-secutity-academy.net/*     #portswigger labs

<img width="956" height="502" alt="image" src="https://github.com/user-attachments/assets/2543be0f-5374-4e3c-962f-45d232916984" />

Testattiin, että filtteri toimii.

<img width="760" height="975" alt="image" src="https://github.com/user-attachments/assets/1e6769d9-2df4-4f37-a116-fef2d683b240" />

Proxy sieppaa vain filtteröidyt kohteet **localhost** ja Portswiggerin labit **.web-secutity-academy.net/**.

---
## PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu. ratkaisu ja haavoittuvuuden etsiminen on selitettävä ja perusteltava.

## Cross Site Scripting (XSS)

## c) [Reflected XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)
    This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.
    To solve the lab, perform a cross-site scripting attack that calls the alert function.

Koska hakukentän syötettä ei enkoodata eikä mitenkään prosessoida, se tulostaa syötteen takaisin käyttäjälle.

Selain ottaa hyökkääjän skriptin ja suorittaa sen käyttäjän selaimessa.

Tehtiin skripti Labin hakukenttään ja se suoritti koodin.

    <script>alert(1)</script>

<img width="480" height="135" alt="image" src="https://github.com/user-attachments/assets/383191a8-9b67-4748-8300-0f69bb916eb7" />
<br>
<br>
<img width="942" height="55" alt="image" src="https://github.com/user-attachments/assets/79f51fb1-aaca-4241-bc9f-c1b8849e892f" />

Toimii.

---
## d) [Stored XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)
    This lab contains a stored cross-site scripting vulnerability in the comment functionality.
    To solve this lab, submit a comment that calls the alert function when the blog post is viewed. 

Koska sivusto ei enkoodaa dataa, se tallentaa syötteen palvelimen tietokantaan. Kun avaa blogin, niin se suorittaa skriptikoodin selaimessa.

Syötettiin kommenttikenttään skriptikoodi ja postattiin se.

    <script>alert(1)</script>

Kun avaa blogin niin ponnahtaa ilmoitus.

<img width="490" height="132" alt="image" src="https://github.com/user-attachments/assets/f50c90c8-0210-4d30-87ea-4acdf0e2d015" />

Toimii.

---
## e) Selitä esimerkin avulla, mitä hyökkääjä hyötyy XSS-hyökkäyksestä. Alert("Hei Tero!") ei vielä tarjoa kummoista pääsyä. (Tässä alakohdassa ei tarvitse tehdä testejä tietokoneella, pelkkä lyhyt ja selkeä selitys riittää.)

Jos esim. otetaan Stored XSS-hyökkäys sivustoon missä on kommentteja ja syötteitä ei enkoodata kuten d) tehtävässä. Hyökkääjä voi suorittaa haitalliset koodit käyttäjien selaimessa, huijata käyttäjiä ja muuttaa sivustoa.

## Path traversal

## f) [File path traversal, simple case. Laita tarvittaessa Zapissa kuvien sieppaus päälle.](https://portswigger.net/web-security/file-path-traversal/lab-simple)

    This lab contains a path traversal vulnerability in the display of product images.
    To solve the lab, retrieve the contents of the /etc/passwd file

Labin kuvissa on path traversal haavoittuvuus, eli voidaan yrittää päästä **`/etc/passwd`** tiedostoon navigoimalla hakemistoa URL:issa.

Haavoittuvuus löytyi tuotteen URLissa ja ZAP:ista mentiin muokkaamaan sitä **`/image?filename=20.jpg`**. Portswiggerin mukaan kuvat säilytetään **`/var/www/images`** hakemistossa.

Sieltä piti navigoida root-hakemistoon ja sieltä pääsyä haluttuun tietoon. Muokattiin Requesterissa URL:ia hakemaan **`/etc/passwd`** tiedostoa.

    filename=../../../etc/passwd    #navigoitiin /var/www/images hakemistosta root-hakemistoon ja sieltä /etc/passwd tiedostoon.

Piti muuttaa Responsen outputtia tekstimuotoon **`Body: Text`**, jotta tiedot tulostui.
    
<img width="842" height="614" alt="image" src="https://github.com/user-attachments/assets/379e5e15-6552-4c51-84c2-81319ed0a804" />

<br>

<img width="930" height="101" alt="image" src="https://github.com/user-attachments/assets/b5f85103-811c-46d6-b189-a303fc8481f5" />


---

## g) [File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)
    This lab contains a path traversal vulnerability in the display of product images.
    The application blocks traversal sequences but treats the supplied filename as being relative to a default working directory.
    To solve the lab, retrieve the contents of the /etc/passwd file. 

Labin kuvissa on path traversal haavoittuvuus, eli voidaan yrittää päästä **`/etc/passwd`** tiedostoon navigoimalla hakemistoa URL:issa.

Labi estää hakemiston kulkua, mutta **`filename`** toimii oletushakemistona.

Portswiggerin videossa he lisäsivät tiedoston sijainnin suoraan **`filename`** jälkeen. Testattiin sillä.

    filename=/etc/passwd    #navigoitiin suoraan /etc/passwd tiedostoon, absolute path

<img width="836" height="613" alt="image" src="https://github.com/user-attachments/assets/840b4412-d227-4579-a226-d126cd089b7b" />

<br>

<img width="927" height="66" alt="image" src="https://github.com/user-attachments/assets/9445d267-a5da-498d-9c51-7e858facc124" />

---
## h) [File path traversal, traversal sequences stripped non-recursively](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)
    This lab contains a path traversal vulnerability in the display of product images.
    The application strips path traversal sequences from the user-supplied filename before using it.
    To solve the lab, retrieve the contents of the /etc/passwd file. 

Samaa hommelia kuin aikaisemmat, mutta ne ratkaisut eivät todennäköisesti toimi.

Testattiin kummatkin ratkaisut Requesterissa, ei toiminut.

    filename=../../../etc/passwd
    filename=/etc/passwd HTTP/1.1
    
    "No such file"

Portswiggerissa on myös muu oikaisu kun sekvenssiä yritetään estää, syöttämällä sisennetyt sekvenssit **`....//`**.

Testattuaan oikaisua, **`....//`** kulkee kuin **`../`**.

    filename=....//....//....//etc/passwd

<img width="831" height="614" alt="image" src="https://github.com/user-attachments/assets/304c62e1-4173-46f2-948f-7edd02a976ea" />

<br>

<img width="926" height="57" alt="image" src="https://github.com/user-attachments/assets/131d2ab5-5ae7-49f3-8183-c632c513fcbe" />

Toimii!

---

## i) [Insecure Direct Object References (IDOR)](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)
    This lab stores user chat logs directly on the server's file system, and retrieves them using static URLs.
    Solve the lab by finding the password for the user carlos, and logging into their account. 

Yritin aluksi saada jotakin irti kun latasin transcriptin **`Live Chat -> View transcript`**.

Sitten yritin etsiä ZAP:ista tietoa kuten carlos, login, POST-pyynnöt, vielä yritettiin kirjautua sisään carlos-käyttäjätunnuksilla ilman salasanaa.

Tehtävänratkaisussa ei oikein edetty, joten katsottiin Portswaggerista ratkaisu. Ei edes oltu huomattu, että **`View transcript`** oli skipannut **`1.txt`** ja mennyt suoraan 2.txt!

Mentiin ZAP:iin ja etsittiin sieltä **download-transcript -> GET:3.txt** ja muutettiin se **1.txt**.

<img width="1257" height="257" alt="image" src="https://github.com/user-attachments/assets/1902b9a9-3d47-453d-9333-59b4ed732f97" />

<br>

<img width="831" height="542" alt="image" src="https://github.com/user-attachments/assets/da0934df-8e0f-43c5-8f4a-a4d4947ea48b" />

<br>

Testattiin salasana.

<img width="1255" height="562" alt="image" src="https://github.com/user-attachments/assets/7ba3d905-c1d9-4ebc-b37e-cac8989b8331" />

Päästiin sisään.

---
## Lähteet
FoxyProxy. s.a. [URL Patterns](https://help.getfoxyproxy.org/index.php/knowledge-base/url-patterns/)

Karvinen, T. 2026. [Tunkeutumistestaus h4](https://terokarvinen.com/tunkeutumistestaus/#h4-taysin-laillinen-sertifikaatti). Luettu: 15.9.2026.

OWASP Top 10 Team. 2021. [A01:2021 – Broken Access Control](https://top10.owasp.org/2021/A01_2021-Broken_Access_Control/). Luettu: 15.9.2026

PortSwigger s.a. [Cross-site scripting](https://portswigger.net/web-security/cross-site-scripting). Luettu: 16.9.2026.

PortSwigger s.a. [Insecure direct object references (IDOR)](https://portswigger.net/web-security/access-control/idor). Luettu: 16.9.2026.

PortSwigger s.a. [Path traversal](https://portswigger.net/web-security/file-path-traversal). Luettu: 15.9.2026.

PortSwigger s.a. [Reflected XSS](https://portswigger.net/web-security/cross-site-scripting/reflected). Luettu: 15.9.2026.

PortSwigger s.a. [Stored XSS](https://portswigger.net/web-security/cross-site-scripting/stored). Luettu: 15.9.2026.






