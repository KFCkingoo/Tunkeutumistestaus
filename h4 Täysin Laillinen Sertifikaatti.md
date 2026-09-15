## h4 Täysin Laillinen Sertifikaatti
Raporttitehtävät Tero Karvisen kurssille - Tunkeutumistestaus ICI005AS3A-3007.

Syksy 2026.

### Ympäristö

VBox Kali GNU/Linux

Ryzen 5

---
## x) Lue/katso ja tiivistä.
#### OWASP 2021: OWASP Top 10:2021 [A01:2021 – Broken Access Control (IDOR ja path traversal ovat osa tätä)](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
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
-



---

## a) Totally Legit Sertificate. 
#### Asenna OWASP ZAP ja käynnistä

    sudo apt install zaproxy
    zaproxy
    
#### Generoi CA-sertifikaatti
Generoitiin CA-sertifikaatti Tool -> Options -> Network -> Server Certificates. Tallennettiin CA-sertifikaatti.

<img width="741" height="580" alt="image" src="https://github.com/user-attachments/assets/463e35dd-c9a5-4704-a52c-59d90669b3bb" />

#### Asenna se selaimeen
Avattiin Firefox ja lisättiin CA-sertifikaatti. **`Firefox Settings -> Search "certificates" -> View Certificates... -> Import`**

<img width="792" height="310" alt="image" src="https://github.com/user-attachments/assets/b647dc40-8721-4f45-bb58-04e44a988ce6" />

<br>
Tarkistettiin, että sertifikaatti on asennettu.

<img width="632" height="77" alt="Näyttökuva 2026-09-15 161908" src="https://github.com/user-attachments/assets/0e337a39-e139-46cc-a30c-9dfdc55d0a9f" />


#### Laita ZAP proxyksi selaimeen
ZAP:illa pystyy suoraan avata selain konfiguroituna Manual Explore:sta.

<img width="748" height="816" alt="image" src="https://github.com/user-attachments/assets/ae200f03-b82d-492c-9e12-7596a1d2c948" />

Selain avattiin ZAP:illa suoraan ja haulla "proxy" näkyy, että selain on jo valmiiksi konfiguroitu proxyna. Konfiguroitiin kuitenkin manuaalisesti selaimeen samalla mallilla tehtävää varten.

`localhost` ei toiminut, joten piti starttaa apache.

<img width="1217" height="181" alt="image" src="https://github.com/user-attachments/assets/56779181-bca5-424a-ac48-59f98e41de21" />

ZAP toimii selaimen proxyna.

#### Laita ZAP sieppaamaan myös kuvat
Laitettiin asetus päälle **`Tools -> Options -> Display -> Process images in HTTP requests/responses`**.

Response kohdan alapalkista näkyy myös pyydetty kuva.

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

Proxy sieppaa vain filtteröidyt kohteet.

---
## PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu. ratkaisu ja haavoittuvuuden etsiminen on selitettävä ja perusteltava.

#### Cross Site Scripting (XSS)

c) [Reflected XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)

d) [Stored XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)

e) Selitä esimerkin avulla, mitä hyökkääjä hyötyy XSS-hyökkäyksestä. Alert("Hei Tero!") ei vielä tarjoa kummoista pääsyä. (Tässä alakohdassa ei tarvitse tehdä testejä tietokoneella, pelkkä lyhyt ja selkeä selitys riittää.)



#### Path traversal

f) [File path traversal, simple case. Laita tarvittaessa Zapissa kuvien sieppaus päälle.](https://portswigger.net/web-security/file-path-traversal/lab-simple)

    This lab contains a path traversal vulnerability in the display of product images.
    To solve the lab, retrieve the contents of the /etc/passwd file

Labissa on path traversal haavoittuvuus, eli voidaan yrittää päästä **`/etc/passwd`** tiedostoon navigoimalla hakemistoa URL:issa.

Haavoittuvuus löytyi tuotteen URLissa ja ZAP:ista mentiin muokkaamaan **`/image?filename=20.jpg`**. Portswiggerin mukaan kuvat säilytetään **`/var/www/images`** hakemistossa.

Sieltä piti navigoida root-hakemistoon ja sieltä pääsyä haluttuun tietoon. Muokattiin Requesterissa URL:ia hakemaan **`/etc/passwd`** tiedostoa.

    filename=../../../etc/passwd    #navigoitiin root-hakemistoon ja sieltä /etc/passwd tiedostoon.

Piti muuttaa Responsen outputtia tekstimuotoon **`Body: Text`**, jotta tiedot tulostui.
    
<img width="842" height="614" alt="image" src="https://github.com/user-attachments/assets/379e5e15-6552-4c51-84c2-81319ed0a804" />

<br>

<img width="930" height="101" alt="image" src="https://github.com/user-attachments/assets/b5f85103-811c-46d6-b189-a303fc8481f5" />


---

g) [File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)

h) [File path traversal, traversal sequences stripped non-recursively](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)




#### Insecure Direct Object Reference (IDOR)

i) [Insecure direct object references](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)

---
## Lähteet
[A01:2021 – Broken Access Control (IDOR ja path traversal ovat osa tätä)](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

[Cross-site scripting](https://portswigger.net/web-security/cross-site-scripting) 

[File path traversal, simple case. Laita tarvittaessa Zapissa kuvien sieppaus päälle.](https://portswigger.net/web-security/file-path-traversal/lab-simple)

[File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)

[File path traversal, traversal sequences stripped non-recursively](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)

[Insecure direct object references](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)

[Insecure direct object references (IDOR)](https://portswigger.net/web-security/access-control/idor)

Karvinen Tero 2026. [Tunkeutumistestaus h4](https://terokarvinen.com/tunkeutumistestaus/#h4-taysin-laillinen-sertifikaatti)

[Path traversal](https://portswigger.net/web-security/file-path-traversal)

[Reflected XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)

[Stored XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)




