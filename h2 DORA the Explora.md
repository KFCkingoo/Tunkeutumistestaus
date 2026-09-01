## h2 DORA the Explora
Raporttitehtävä Tero Karvisen kurssille - Tunkeutumistestaus ICI005AS3A-3007.

Syksy 2026.

### Ympäristö
VBox Kali GNU/Linux 

Ryzen 5

---
## x) Lue/katso/kuuntele ja tiivistä.
#### Buuri 2026: [DORA and TLPT testing - Lecture for Haaga-Helia on 31 March 2026](https://terokarvinen.com/buuri-2026-dora-and-threat-lead-penetration-testing/buuri-2026-dora-and-threat-lead-penetration-testing--teros-pentest-course.pdf)
- Dora on EU-laajuinen sääntely finanssialan toiminnalliseen sietokykyyn.
- TLPT (Threat-Led Penetration Testing) on testausvaatimus merkittäville yksilöille, jonka ovat määritellyt viranomaiset.
- TIBER-EU/TIBER-FI (Threat Intelligence-Based Ethical Red Teaming) on kehys mikä tarjoaa red team-testeille yhteisen referenssimallin.

#### DORA [(Regulation ... on digital operational resilience for the financial sector)](https://eur-lex.europa.eu/eli/reg/2022/2554/oj/eng). Article 26-27.
- TLPT testauksen regulaatiota
- Havainnot ilmoitetaan viranomaisille

#### [TIBER-FI procedures and guidelines](https://www.suomenpankki.fi/globalassets/bof/en/money-and-payments/the-bank-of-finland-as-catalyst-payments-council/tiber-fi/tiber-fi-2.0-procedures-and-guidelines.pdf). 5.4 Testing phase: Red team testing

| Vaihe | TIBER-FI prosessi |
|---|---|
| **Recon** | Tiedonkeruu |
| **Weaponisation** | Analysointi ja suunnittelu |
| **Delivery** | Hyökkäyksen toimittaminen kohteeseen |
| **Exploitation** | Yritys murtautua sisään |
| **Control and Movement** | Siirtyminen muihin haavoittuviin tai korkea-arvoisempiin järjestelmiin |
| **Actions on Target** | Pääsy tavoiteltuihin tietoihin on saavutettu. Testi pyritään suorittamaan loppuun ja liput kerätään. |


---
## a) Asenna Metasploitable 2 virtuaalikoneeseen. 
Ladattu sivulta https://sourceforge.net/projects/metasploitable/.

Asennettu sillä uusi VM VBoxiin.

---
## b) Tee Kalin ja Metasploitablen välille virtuaaliverkko.
VBoxin Network asetuksissa asetettiin koneille sama virtuaaliverkko.

Kalilla oletuksena NAT, lisättiin **Host-only Adapter** tehtävää varten.

Metasploitablella **Host-only Adapter**.


---
## c) Harjoittelemme omassa virtuaaliverkossa, jossa on Kali ja Metaspoitable. Osoita testein, että 1) koneet eivät saa yhteyttä Internetiin 2) Koneet saavat yhteyden toisiinsa.
Varmistettu koneista yhteys pois internetistä.

<img width="306" height="253" alt="Näyttökuva 2026-09-01 195757" src="https://github.com/user-attachments/assets/e009e819-676c-48a0-af21-e6bd38a57139" />

<br>
<img width="310" height="55" alt="image" src="https://github.com/user-attachments/assets/def70db5-77df-4bc3-8361-99ea6f5b5fb4" />

<br>
<img width="357" height="35" alt="image" src="https://github.com/user-attachments/assets/0f0d1656-47cc-4f51-931a-bc6a4e3c7e25" />

<br>
<br>

Otettu IP kummastakin VM, jotta voidaan ottaa yhteyttä toisiinsa.

    ifconfig
    192.168.56.101 #kali VM
    192.168.56.102 #metasploitable 2 VM

<img width="512" height="182" alt="image" src="https://github.com/user-attachments/assets/20cc4eab-f4cf-493a-bbf9-5f797784d30a" />

<img width="566" height="162" alt="image" src="https://github.com/user-attachments/assets/f77036ae-d6b8-47f1-8ca9-03527dc82efb" />

Yhteys luotu.

---
## d) Etsi Metasploitable porttiskannaamalla (nmap -sn). Tarkista selaimella, että löysit oikean IP:n - Metasploitablen weppipalvelimen etusivulla lukee Metasploitable.
Tehty porttiskannaus.

    ┌──(abc㉿kali)-[~]
    └─$ sudo nmap -sn 192.168.56.0/24
    [sudo] password for abc: 
    Starting Nmap 7.99 ( https://nmap.org ) at 2026-09-01 13:21 -0400
    mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
    Nmap scan report for 192.168.56.1
    Host is up (0.00015s latency).
    MAC Address: 0A:00:27:00:00:17 (Unknown)
    Nmap scan report for 192.168.56.100
    Host is up (0.00034s latency).
    MAC Address: 08:00:27:67:BB:1A (Oracle VirtualBox virtual NIC)
    Nmap scan report for 192.168.56.102
    Host is up (0.00025s latency).
    MAC Address: 08:00:27:75:E9:9C (Oracle VirtualBox virtual NIC)
    Nmap scan report for 192.168.56.101
    Host is up.
    Nmap done: 256 IP addresses (4 hosts up) scanned in 1.81 seconds

<br>

Oikea IP : `192.168.56.102`


<img width="562" height="362" alt="image" src="https://github.com/user-attachments/assets/423406c0-8b46-454d-9cd2-15ef03330670" />




---
## e) Porttiskannaa Metasploitable huolellisesti ja kaikki portit (nmap -A -T4 -p-). Poimi 2-3 hyökkääjälle kiinnostavinta porttia. Analysoi ja selitä tulokset näiden porttien osalta. Voit hakea analyysin tueksi tietoa verkosta, muista merkitä lähteet.
Tehty porttiskannaus `sudo nmap -A -T4 -p- 192.168.56.102`.

    PORT      STATE SERVICE     VERSION
    21/tcp    open  ftp         vsftpd 2.3.4
    |_ftp-anon: Anonymous FTP login allowed (FTP code 230)

    23/tcp    open  telnet      Linux telnetd

- 23/tcp FTP - ftp-anon: Anonymous FTP login allowed (FTP code 230) - pääsy ilman käyttätunnusta ja salasanaa.

- 23/tcp telnet - ei salaa tietoja etäyhteyden käyttäessä. Vanha protokolla ja haavoittuvainen.


---
## Lähteet
Buuri 2026: [DORA and TLPT testing - Lecture for Haaga-Helia on 31 March 2026](https://terokarvinen.com/buuri-2026-dora-and-threat-lead-penetration-testing/buuri-2026-dora-and-threat-lead-penetration-testing--teros-pentest-course.pdf)

DORA [(Regulation ... on digital operational resilience for the financial sector)](https://eur-lex.europa.eu/eli/reg/2022/2554/oj/eng)
- Article 26 "Advanced testing of ICT tools, systems and processes based on TLPT"
- Article 27 "Requirements for testers for the carrying out of TLPT"

Karvinen Tero 2026. [Tunkeutumistestaus h2](https://terokarvinen.com/tunkeutumistestaus/#h2-dora-the-explora)

[TIBER-FI procedures and guidelines](https://www.suomenpankki.fi/globalassets/bof/en/money-and-payments/the-bank-of-finland-as-catalyst-payments-council/tiber-fi/tiber-fi-2.0-procedures-and-guidelines.pdf)
- 5.4 Testing phase: Red team testing (johdantokappale suoraan 5.4 alta, "5.4.1 Red team test plan creation" alkuun asti)


