## h4 Täysin Laillinen Sertifikaatti
Raporttitehtävät Tero Karvisen kurssille - Tunkeutumistestaus ICI005AS3A-3007.

Syksy 2026.

### Ympäristö

VBox Kali GNU/Linux

Ryzen 5

---
## x) Lue/katso ja tiivistä.
**OWASP 2021: OWASP Top 10:2021**

[A01:2021 – Broken Access Control (IDOR ja path traversal ovat osa tätä)](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

    
**PortSwigget Academy**

[Insecure direct object references (IDOR)](https://portswigger.net/web-security/access-control/idor)

[Path traversal](https://portswigger.net/web-security/file-path-traversal)
[Cross-site scripting](https://portswigger.net/web-security/cross-site-scripting) 


---

## a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Laita ZAP sieppaamaan myös kuvat, niitä tarvitaan tämän kerran kotitehtävissä. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään. (Voi vaatia Firefox about:config network.proxy.allow_hijacking_localhost. Foxyproxy laittoi tämän aiemmin päälle itse. Kalin Firefox ESR oli viimeksi ongelmia Foxyproxyn kanssa - vaihtoehtona on asettaa Proxy käsin Settings, hakusana "proxy")




---
## b) Kettumaista. Asenna "FoxyProxy Standard" Firefox Addon, ja lisää ZAP proxyksi siihen. Käytä FoxyProxyn "Patterns" -toimintoa, niin että vain valitsemasi weppisivut ohjataan Proxyyn. (Läksyssä ohjataan varmaankin PortSwigger Labs ja localhost.)





---
## PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu. ratkaisu ja haavoittuvuuden etsiminen on selitettävä ja perusteltava.

**Cross Site Scripting (XSS)**

c) [Reflected XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)

d) [Stored XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)

e) Selitä esimerkin avulla, mitä hyökkääjä hyötyy XSS-hyökkäyksestä. Alert("Hei Tero!") ei vielä tarjoa kummoista pääsyä. (Tässä alakohdassa ei tarvitse tehdä testejä tietokoneella, pelkkä lyhyt ja selkeä selitys riittää.)



**Path traversal**

f) [File path traversal, simple case. Laita tarvittaessa Zapissa kuvien sieppaus päälle.](https://portswigger.net/web-security/file-path-traversal/lab-simple)

g) [File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)

h) [File path traversal, traversal sequences stripped non-recursively](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)




**Insecure Direct Object Reference (IDOR)**

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




