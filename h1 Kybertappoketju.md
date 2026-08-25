# h1 Kybertappoketju
Raporttitehtävä Tero Karvisen kurssille - Tunkeutumistestaus ICI005AS3A-3007.

Syksy 2026.

### Ympäristö
VBox Kali GNU/Linux 

Ryzen 5

## x) Lue/katso/kuuntele ja tiivistä.
#### Darknet Diaries, Episode 171: Ubiquiti
Follows the case of Nickolas Sharp who worked in Ubiquiti.

Had administrator access to confidential data and tried to exploit the company.

Showcased the process of cyber kill chain and especially the risk of insider threats and excessive privileges.

#### Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain.
Using a kill chain model reduces the success of threat actors.

Intrusion Kill Chain is used to describe the process of how attackers begin their attack and how they reach it.

Stopping any stage in the Intrusion Kill Chain prevents the attackers from success or restricting their ability.

#### € Santos et al: The Art of Hacking (Video Collection): 4.3 Surveying Essential Tools for Active Reconnaissance.
Active Recon = sending packets to target. **Portscanning -> Web Service Review -> Vulnerability scanning**

| Port Scanner             | Description          |
| ------------------------ | -------------------- |
| **Nmap**                 | Versatile and stable |
| **Masscan**              | Very fast            |
| **UDP Protocol Scanner** | Fast UDP scanning    |

Tool on prioritizing Web Applications
- EyeWitness - gives screenshots and an overview of all the web applications.
  
#### KKO 2003:36.
Osuuspankkikeskus-OPK osuuskuntaan tehty porttiskannaus.

## a) Asenna Kali
Asennettu.

Kali GNU/Linux

Version 2026.2
## b) Irrota Kali-virtuaalikone verkosta. Todista testein, että kone ei saa yhteyttä Internetiin (esim. 'ping 8.8.8.8')
Irrotettu yhteys nettiin Kalista ja Vboxista. Pingaus testattu.

<p>
<img width="395" height="110" alt="Näyttökuva 2026-08-25 221306" src="https://github.com/user-attachments/assets/e00668ae-7df2-45e4-911d-d71585858ea0" />
<img width="312" height="62" alt="Näyttökuva 2026-08-25 221335" src="https://github.com/user-attachments/assets/b8e7b827-7651-43a0-bd0c-d6836e68cdf4" />
</p>


## c) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (nmap -T4 -A localhost). Selitä komennon paramterit. Analysoi ja selitä tulokset.
Suoritettu porttiskannaus sudolla.
<img width="1031" height="231" alt="kuva" src="https://github.com/user-attachments/assets/8443b07c-7220-4dbf-803a-7b1f079749be" />

**-T4** - nmap-skannauksen ajoitus.

**-A** - OS ja version tunnistus, skriptiskannaus ja traceroute.

## d) Asenna kaksi vapaavalintaista demonia ja skannaa uudelleen. Analysoi ja selitä erot.
Kalissa asennettu ssh ja apache. Käynnistetty demonit ja porttiskannattu uudelleen.

<img width="1031" height="342" alt="kuva" src="https://github.com/user-attachments/assets/a70ec874-3727-4d60-88c9-9a074afc4547" />

Palvelujen tiedot kuten nimi, versio, os ja status tulostui.

TCP portit 22 ja 80 oli auki.

## e) Ratkaise vapaavalintainen kone HackTheBoxista. Omalle tasolle sopiva, useimmille varmaan Starting Pointista. Valitse kone, jota et ole ratkaissut vielä. Ei tunnilla näytetty Meow. (Propellihatuille: jos teet vaikeampia ei-starting-point koneita, niin retired tai vastaava kone, josta saa julkaista writeupin).
Ongelmia HTB koneen kanssa. Tehdään myöhemmin, ennen seuraavaa tuntia. Palautetaan nyt kuitenkin raportti ennen deadlinea.


## Lähteet
Darknet Diarias 2026. [Ubiquiti.](https://darknetdiaries.com/episode/178/)

Finlex 2003. [KKO:2003:36](https://www.finlex.fi/fi/oikeuskaytanto/korkein-oikeus/ennakkopaatokset/2003/36)

Hutchins et al 2011: [Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain.](https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf)

Santos et al: [The Art of Hacking (Video Collection): 4.3 Surveying Essential Tools for Active Reconnaissance.](https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00)

Tero Karvinen 2026. [Tunkeutumistestaus h1](https://terokarvinen.com/tunkeutumistestaus/)

