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
- 

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

    # Kopioidaan git repo
    git clone --depth=1 https://github.com/openwall/john.git

    # ./configure tunnistaa ympäristön ja tekee Makefile 'make'-komennolle
    cd john/src/
    ./configure

    # Compile
    make -s clean && make -sj4

Asennettu ja käännetty.

**Ladattiin esimerkkitiedosto ja purattiin se**

    wget https://TeroKarvinen.com/2023/crack-file-password-with-john/tero.zip

Purkaus epäonnistui, tiedosto vaatii salasanan


**Crack zip password**

    # Otetaan hash talteen
    zip2john tero.zip > tero.zip.hash

    # Sanakirja hyökkäys
    john tero.zip.hash

Saatiin salasana.

<img width="767" height="195" alt="kuva" src="https://github.com/user-attachments/assets/68f3203e-08ba-44f7-9d2c-3ba6e2df49ff" />

**Purataan tiedosto uudelleen**

Käyttämällä **butterfly** salasanaa, saatiin purattua tiedosto.

Tässä vielä purattu sisältö.

<img width="602" height="157" alt="kuva" src="https://github.com/user-attachments/assets/af9bb5e5-52bc-42c9-9c82-6a753f10618d" />

---
## e) Tiedosto. Tee itse tai etsi verkosta jokin salakirjoitettu tiedosto, jonka saat auki. Murra sen salaus. (Jokin muu formaatti kuin aiemmissa alakohdissa kokeilemasi).



---
## f) Tiiviste. Tee itse tai etsi verkosta salasanan tiiviste, jonka saat auki. Murra sen salaus. (Jokin muu formaatti kuin aiemmissa alakohdissa kokeilemasi. Voit esim. tehdä käyttäjän Linuxiin ja murtaa sen salasanan.)



---
## g) Sanakirja. Oman sanakirjan teko parantaa onnistumismahdollisuuksia. Demonstroi, kuinka teet oman sanakirjan hashcat:n tai john:iin.



---
## h) Hash rules. Näytä esimerkki HashCatin sääntöjen käytöstä (rules).


## Lähteet
Karvinen, T. 2023. [Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/). Luettu: 22.9.2026.

Karvinen, T. 2023. [Cracking Passwords with Hashcat](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/). Luettu: 22.9.2026.
