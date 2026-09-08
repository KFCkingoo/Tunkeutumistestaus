## h3 EternalHomework
Raporttitehtävät Tero Karvisen kurssille - Tunkeutumistestaus ICI005AS3A-3007.

Syksy 2026.
#### Ympäristö

VBox Kali GNU/Linux

Ryzen 5

---
#### x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

    € Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit (kohdasta Conducting a penetration test with Metasploit luvun loppuun eli "Summary" loppuun)
    Mitä 'nmap -sn' tekee? Älä arvaa, vaan perustele lähteillä. Mistä tiedät, että käyttämäsi lähde on luotettava?

---
#### b) Tallenna porttiskannauksen tuloksia Metasploitin tietokantoihin. Skannaa niin, että Metasploitable tulee mukaan. Kannattaa ottaa mukaan ainakin versioskannaus -sV (joka on banner grabbing plus).
Alustettiin tietokanta ja avattiin msfconsole.

  
    sudo msfdb init
    sudo msfdbconsole

<img width="597" height="491" alt="image" src="https://github.com/user-attachments/assets/079c3261-9f80-4c8f-8d0d-82ffdc5f4531" />


Porttiskannattiin localhost ja saatiin 0 palvelutulosta.

    db_nmap -sV localhost  #'Failed to resolve "localhost"
    
Tehtiin uusi porttiskannaus Metasploitablen IP:llä ja palvelut tulostui raporttiin.

    db_nmap -T4 -sV 192.168.56.102

<img width="727" height="517" alt="image" src="https://github.com/user-attachments/assets/ddf64985-3be5-4e18-8347-b673691b5ea0" />

---
#### c) Tarkastele Metasploitin tietokantoihin tallennettuja tietoja komennoilla "hosts" ja "services". Kokeile suodattaa näitä listoja tai hakea niistä.
Syötettiin komennot ja kokeiltiin suodattaa listat.

    hosts
    hosts -S linux

<img width="760" height="292" alt="image" src="https://github.com/user-attachments/assets/4e5355a2-1361-428a-8b9c-29d73683bdf5" />

    services
    services -p 21

<img width="776" height="537" alt="image" src="https://github.com/user-attachments/assets/66c53032-ba1b-4acf-8e0b-e10e7ca3601d" />

<br>

<img width="592" height="120" alt="image" src="https://github.com/user-attachments/assets/c74e5775-c39a-430f-9974-3b799fe2c87f" />

---
#### d) Internet famous. Etsi Metasploitablen mukana tulevista hyökkäyksistä (en: exploits; search) sellainen, joka on ollut julkisuudessa.
---
#### e) Vertaile nmap:n omaa tiedostoon tallennusta (-oA foo) ja db_nmap:n tallennusta tietokantoihin. Mitkä ovat eri tiedostomuotojen ja Metasploitin tietokannan hyvät puolet?
---
#### f) Murtaudu Metasploitablen vsftpd-palveluun
---
#### g) Kerää levittäytymisessä (lateral movement) tarvittavaa tietoa metasploitablesta. Analysoi tiedot. Selitä, miten niitä voisi hyödyntää.
---
#### h) Murtaudu Metasploitableen jollain toisella tavalla. (Jos tämä kohta on vaikea, voit tarvittaessa turvautua verkosta löytyviin läpikävelyohjeisiin. Merkitse silloin raporttiin, missä määrin tarvitsit niitä).
---
#### i) Demonstroi Meterpretrin ominaisuuksia.
---
#### j) Tallenna shell-sessio tekstitiedostoon script-työkalulla (script -fa log001.txt) tai tmux:lla.
---
#### k) Pivot point. Laita kaikki harjoituksen tiedostot (script -fa, nmap -oA...) samaan kansioon. Hae sopiva pivot point (sovellus, versio, osoite, MAC-numero) 'grep -r' -komennolla. Keksi uskottava esimerkkikysymys, johon haet vastausta.
---
#### l) Attaaack! Mitä Mitre Attack taktiikoita ja tekniikoita käytit tässä harjoituksessa? (Tässä alakohdassa "Attaack!" ei tarvitse tehdä lisää testejä koneella, koska testit on jo tehty.)
