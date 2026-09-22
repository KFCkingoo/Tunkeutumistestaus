# h5 Elokuu2026!
Raporttitehtävät Tero Karvisen kurssille - Tunkeutumistestaus ICI005AS3A-3007.

Syksy 2026.
### Ympäristö

VBox Kali GNU/Linux

Ryzen 5

---

## x) Lue/katso ja tiivistä.
**Karvinen 2022: [Cracking Passwords with Hashcat](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/)**

- Hashit ovat yksisuuntaisia, mutta niitä voidaan verrata sanalistaan, jos löytyy osuma.
- **`hashid`**-työkalulla yritetään tunnistaa hashin formaatti
-  **`hashcat`**-työkalulla murretaan hash ja verrataan sanakirjaan

**Karvinen 2023: [Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/)**

- Tiedosto esim. zip voi olla lukittu salasanalla, sen voi murtaa **`John the Ripper`**-työkalulla
- John the Ripper voi murtaa monta eri formaattia

---
## a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana.
Kalissa valmiiksi asennettu. 

**Tehtiin hakemisto tehtävälle**

mkdir hashed
cd hashed

**Ladattiin sanakirja rockyou.txt**

    wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz
    tar xf rockyou.txt.tar.gz
    rm rockyou.txt.tar.gz
    
**Tunnistettiin minkätyyppinen hash**
  
    └─$ hashid -m 6b1628b016dff46e6fa35684be6acc96                                                                                                             
    Analyzing '6b1628b016dff46e6fa35684be6acc96'
    [+] MD2 
    [+] MD5 [Hashcat Mode: 0]
    [+] MD4 [Hashcat Mode: 900]
    ...

**Crack the hash**
Virhetilanne, puuttuu yhteensopivia ajureita:

    hashcat (v7.1.2) starting
    
    clGetPlatformIDs(): CL_PLATFORM_NOT_FOUND_KHR
    
    ATTENTION! No OpenCL, HIP or CUDA compatible platform found.

Asennettiin yhteensopiva ajuri.

    sudo apt install pocl-opencl-icd

Ajettiin hashcat uudelleen.

    hashcat -m 0 '6b1628b016dff46e6fa35684be6acc96' rockyou.txt -o solved

**-m** on mode.

**0** on MD5 formaatti.

**-o solved** lisää murretun salasanan 'solved'-tiedostoon.

    └─$ cat solved 
    6b1628b016dff46e6fa35684be6acc96:summer
---
## c) Asenna John the Ripper ja testaa sen toiminta murtamalla jonkin esimerkkitiedoston salasana.
**Asennetaan Karvisen ohjeiden mukaan paketit**

    └─$ sudo apt -y install micro bash-completion git build-essential libssl-dev zlib1g zlib1g-dev zlib-gst libbz2-1.0 libbz2-dev atool zip
    Error: Unable to locate package zlib-gst

Karvisen ohjeessa näkyy komennossa **`zlib-gst`**, mutta pakettien taulukossa **`zlib1g-gst`**. Katsottiin Kalin paketeista ja löytyi **`zlib1g-dev`**. Kuitenkin jätettiin paketti lataamatta.

**Asennetaan John the Ripper, Jumbo versio**
```bash
# Kopioidaan git repo
git clone --depth=1 https://github.com/openwall/john.git

# ./configure tunnistaa ympäristön ja tekee Makefile 'make'-komennolle
cd john/src/
./configure

# Compile
make -s clean && make -sj4
```
Asennettu ja käännetty.

**Ladattiin esimerkkitiedosto ja purattiin se**

    wget https://TeroKarvinen.com/2023/crack-file-password-with-john/tero.zip

Purkaus epäonnistui, tiedosto vaatii salasanan


**Crack zip password**
```bash
# Otetaan hash talteen
zip2john tero.zip > tero.zip.hash

# Sanakirja hyökkäys
john tero.zip.hash
```

Saatiin salasana.

<img width="767" height="195" alt="kuva" src="https://github.com/user-attachments/assets/68f3203e-08ba-44f7-9d2c-3ba6e2df49ff" />

**Purataan tiedosto uudelleen**

Käyttämällä **butterfly** salasanaa, saatiin purattua tiedosto.

Tässä vielä purattu sisältö.

<img width="602" height="157" alt="kuva" src="https://github.com/user-attachments/assets/af9bb5e5-52bc-42c9-9c82-6a753f10618d" />

---
## e) Tiedosto. Tee itse tai etsi verkosta jokin salakirjoitettu tiedosto, jonka saat auki. Murra sen salaus. (Jokin muu formaatti kuin aiemmissa alakohdissa kokeilemasi).
**Tiedoston luonti**

    nano namnamA.txt

**Tiedoston salaus 7z**

```bash
    7z a -p "pizza" secret.7z namnamA.txt

    # Poistettiin tiedosto
    rm namnamA.txt

    # Epäonnistunut purkaustesti
    7z e pizza.7z
```

**a** on tiedoston lisäys.

**-p** on salasana.

**e** purkaus.

**Murretaan 7z-tiedosto**
```bash
    # Otetaan hash talteen
    7z2john pizza.7z > pizza.7z.hash

    # Selvitetään salasana
    john pizza.7z.hash
```

<img width="767" height="227" alt="kuva" src="https://github.com/user-attachments/assets/5fbf1000-7fde-4474-b929-535f51d8c52c" />

Saatiin salasana **pizza**, puretaan 7z-tiedosto.

<img width="575" height="455" alt="kuva" src="https://github.com/user-attachments/assets/5e1a0c23-f839-4909-977f-cb3d6c6a24bb" />

Saatiin purettua tiedosto ja tässä purettu tiedosto.

    └─$ cat namnamA.txt                  
    burgir

---
## f) Tiiviste. Tee itse tai etsi verkosta salasanan tiiviste, jonka saat auki. Murra sen salaus. (Jokin muu formaatti kuin aiemmissa alakohdissa kokeilemasi. Voit esim. tehdä käyttäjän Linuxiin ja murtaa sen salasanan.)
**Tehtiin SHA-256 hash sivustolla https://hashgenerator.co/**

**Lisättiin se tiedostoon**

    └─$ cat namnamB.txt
    d241bd2a4c7ef7c695230258b80c66b955a9af10f804a989a1dd84fdbf3ce228

**Selvitettiin hash-formaatti hashid ja hash-identifier työkalulla**

<img width="635" height="742" alt="kuva" src="https://github.com/user-attachments/assets/11c51e35-c529-4408-9149-ffdeb03f1692" />

Tunnistivat formaatin.

**Murretaan tiedosto**

```bash
# Etsittiin moodi numero SHA-256
hashcat -hh

# Ajettiin moodi 1400
hashcat -m 1400 namnamB.txt rockyou.txt -o solved
```

Ei saatu osumaa.

    ...
    Status...........: Exhausted
    Hash.Mode........: 1400 (SHA2-256)
    ...

Luotiin tiedosto, jossa on varsinainen salasana ja testattiin uudelleen.

    echo pizzapolloburgir > testi.txt
    hashcat -m 1400 namnamB.txt testi.txt -o solved

Murtautuminen onnistui.

    └─$ cat solved 
    6b1628b016dff46e6fa35684be6acc96:summer
    d241bd2a4c7ef7c695230258b80c66b955a9af10f804a989a1dd84fdbf3ce228:pizzapolloburgir

---
## g) Sanakirja. Oman sanakirjan teko parantaa onnistumismahdollisuuksia. Demonstroi, kuinka teet oman sanakirjan hashcat:n tai john:iin.
Edellisessä tehtävässä tehty sanakirja **`testi.txt`**, lisätty uusia sanoja.


**hashcat demonstroitu edellisessä tehtävässä**

    └─$ hashcat -m 1400 namnamB.txt testi.txt --show
    d241bd2a4c7ef7c695230258b80c66b955a9af10f804a989a1dd84fdbf3ce228:pizzapolloburgir

**john demonstroiminen**

```bash
# Luettiin sanakirja ja verrattiin hashiin
└─$ john --wordlist=testi.txt namnamB.txt

# Ei osumaa
└─$ john --show namnamB.txt
0 password hashes cracked, 1 left
```

Tässä kysyttiin ChatGPT:ltä apua.

<img width="762" height="180" alt="kuva" src="https://github.com/user-attachments/assets/329954bb-dabf-4d78-bb28-68e7edf429be" />

Piti lisätä SHA-256 formaatti, jotta John tietää hashin sisältävän SHA-256 enkryptauksen.

Tuloste myös kertoo komennosta "--show --format=Raw-SHA256". Syötettiin.
    
    └─$ john --show --format=Raw-SHA256 namnamB.txt 
    ?:pizzapolloburgir
    
    1 password hash cracked, 0 left

---
## h) Hash rules. Näytä esimerkki HashCatin sääntöjen käytöstä (rules).


## Lähteet
Karvinen, T. 2023. [Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/). Luettu: 22.9.2026.

Karvinen, T. 2023. [Cracking Passwords with Hashcat](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/). Luettu: 22.9.2026.
