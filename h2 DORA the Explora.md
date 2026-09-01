## h2 DORA the Explora
Raporttitehtävä Tero Karvisen kurssille - Tunkeutumistestaus ICI005AS3A-3007.

Syksy 2026.

### Ympäristö
VBox Kali GNU/Linux 

Ryzen 5

---
## x) Lue/katso/kuuntele ja tiivistä.


---
## a) Asenna Metasploitable 2 virtuaalikoneeseen. Jos säätelet VirtualBoxista

    Kali saa yhteyden Internettiin, mutta sen voi laittaa pois päältä
    Kalin ja Metasploitablen välillä on host-only network, niin että porttiskannatessa ym. koneet on eristetty intenetistä, mutta ne saavat yhteyden toisiinsa



---
## b) Tee Kalin ja Metasploitablen välille virtuaaliverkko.



---
## c) Harjoittelemme omassa virtuaaliverkossa, jossa on Kali ja Metaspoitable. Osoita testein, että 1) koneet eivät saa yhteyttä Internetiin 2) Koneet saavat yhteyden toisiinsa.



---
## d) Etsi Metasploitable porttiskannaamalla (nmap -sn). Tarkista selaimella, että löysit oikean IP:n - Metasploitablen weppipalvelimen etusivulla lukee Metasploitable.



---
## e) Porttiskannaa Metasploitable huolellisesti ja kaikki portit (nmap -A -T4 -p-). Poimi 2-3 hyökkääjälle kiinnostavinta porttia. Analysoi ja selitä tulokset näiden porttien osalta. Voit hakea analyysin tueksi tietoa verkosta, muista merkitä lähteet.







---
## Lähteet
Buuri 2026: [DORA and TLPT testing - Lecture for Haaga-Helia on 31 March 2026](https://terokarvinen.com/buuri-2026-dora-and-threat-lead-penetration-testing/buuri-2026-dora-and-threat-lead-penetration-testing--teros-pentest-course.pdf)

DORA [(Regulation ... on digital operational resilience for the financial sector)](https://eur-lex.europa.eu/eli/reg/2022/2554/oj/eng)
- Article 26 "Advanced testing of ICT tools, systems and processes based on TLPT"
- Article 27 "Requirements for testers for the carrying out of TLPT"

Karvinen Tero 2026. [Tunkeutumistestaus h2](https://terokarvinen.com/tunkeutumistestaus/#h2-dora-the-explora)

[TIBER-FI procedures and guidelines](https://www.suomenpankki.fi/globalassets/bof/en/money-and-payments/the-bank-of-finland-as-catalyst-payments-council/tiber-fi/tiber-fi-2.0-procedures-and-guidelines.pdf)

- 5.4 Testing phase: Red team testing (johdantokappale suoraan 5.4 alta, "5.4.1 Red team test plan creation" alkuun asti)


