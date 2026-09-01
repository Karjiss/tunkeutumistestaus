# h1 - Dora The Explora

- Kurssi: [Tunkeutumistestaus](https://terokarvinen.com/tunkeutumistestaus/) (Karvinen 2026)
- Opettaja: Tero Karvinen
- Raportin kirjoittaja: Jani Karjalainen


 ## x) Lue/katso/kuuntele ja tiivistä

### **DORA and TLPT testing - Lecture for Haaga-Helia on 31 March 2026** ([Buuri, M. 2026](https://terokarvinen.com/buuri-2026-dora-and-threat-lead-penetration-testing/buuri-2026-dora-and-threat-lead-penetration-testing--teros-pentest-course.pdf))

- Red teamaus kyberturvallisuudessa virallistui ja sai suosiota esimerkiksi rahoitusalalla 2010-luvulla.
- Finanssialan testausprosessi kestää 1-1,5 vuotta.
- Tämä oli todella hyvä luento kalvoineen. Avasi tunkeutumistestausta rahoitusalalla hyvin.

### **DORA - Regulation ... on digital operational resilience for the financial sector ([2022/2554](https://eur-lex.europa.eu/eli/reg/2022/2554/oj/eng))** 

**Artikla 26: Advanced testing of ICT tools, systems and processes based on TLPT**

- Finanssialan yritysten tulee tehdä edistyneet tunkeutumistestit vähintään kolmen vuoden välein.
- Jokaisen uhkaperusteisen tunkeutumistestin täytyy kattaa useita, tai kaikki finanssialan yrityksen kriittisistä tai tärkeistä toiminnoista. Testit myös täytyy suorittaa toimivissa tuotantojärjestelmissä.

**Artikla 27: Requirements for testers for the carrying out of TLPT**

- Finanssialan yritysten tulee käyttää uhkaperusteisiin tunkeutumistesteihin testaajia, jotka ovat:
  - Luotettavimpia ja sopivimpia.
  - Uhkatiedustelun, tunkeutumistestauksen ja red teamauksen asiantuntijoita.


### **TIBER-FI procedures and guidelines([Suomen Pankki. 2025.](https://www.suomenpankki.fi/globalassets/bof/en/money-and-payments/the-bank-of-finland-as-catalyst-payments-council/tiber-fi/tiber-fi-2.0-procedures-and-guidelines.pdf))**

- Red teamin testausvaihe koostuu kahdesta prosessivaiheesta:
  - RTTP (Red Team Test Plan) laatiminen, kesto 2-3 viikkoa.
  - Testaussuunnitelman hyväksymisen jälkeen siirrytään aktiiviseen vaiheeseen, joka kestää vähintään 12 viikkoa.
- Testausvaiheessa voidaan käyttää erilaisia menetelmiä, esimerkiksi:
  - Vaihe 1. Tiedustelu
  - Vaihe 2. Aseistus
  - Vaihe 3. Toimitus
  - Vaihe 4. Hyväksikäyttö
  - Vaihe 5. Hallinta
  - Vaihe 6. Toiminta kohteessa

## a) Asenna Metasploitable 2 virtuaalikoneeseen

Aloitin lataamalla Metasploitable 2 virtuaalikoneen [Sourceforgesta](https://sourceforge.net/projects/metasploitable/files/latest/download),
Latauksen jälkeen avasin **VMWare Workstation Pro:n**, johon asennan virtuaalikoneen.

Workstationissa klikkasin vasemmasta yläkulmasta: ``` File -> Open -> (METASPLOITABLE2) ```

<img width="268" height="276" alt="image" src="https://github.com/user-attachments/assets/d64cca07-f5d0-4fb0-a745-e92dc8cbee5c" />

Käynnistin Metasploitablen Workstationista:

<img width="723" height="523" alt="image" src="https://github.com/user-attachments/assets/259f44b1-696a-48a0-b5ab-857d18d4c5c6" />

- Kone on käynnissä!

## b) Tee Kalin ja Metasploitablen välille virtuaaliverkko

Verkon halutut määritelmät:
  - Kali saa yhteyden Internettiin, mutta sen voi laittaa pois päältä.
  - Kalin ja Metasploitablen välillä on host-only network, niin että porttiskannatessa ym. koneet on eristetty intenetistä, mutta ne saavat yhteyden toisiinsa.

Aloitin sammuttamalla Kalin ja Metasploitablen. Klikkasin Metasploitablen Workstation välilehdellä kohtaa ```Network Adapter```:

<img width="300" height="332" alt="image" src="https://github.com/user-attachments/assets/f9523d30-48ee-4ed0-a46d-62fcd0d6dfce" />

Eteen aukesi välilehti verkkoasetuksille, mistä muutin kohdan network connection: ```NAT```-->```Host-only```

<img width="421" height="252" alt="image" src="https://github.com/user-attachments/assets/31f7001a-d46f-4fb0-bd7d-b9619bf413b6" />

- OK klikkauksella eteenpäin!

Seuraavaksi konfiguroin Kalin verkon. Avasin Kalin verkkoasetukset samalla tavalla, kuin edellisessä vaiheessa.
Kaliin minun pitää lisätä yksi uusi verkkokortti, se onnistui klikkaamalla asetusten alaosassa kohtaa ```Add```.

<img width="345" height="730" alt="image" src="https://github.com/user-attachments/assets/288f5ebc-ddbf-4b20-9393-7baef5ec0924" />

Sitten valitsin: ```Network Adapter``` ja hyväksyin muutoksen painamalla ```Finish```.

<img width="439" height="431" alt="image" src="https://github.com/user-attachments/assets/c00b3e63-0683-4e1b-b448-25628bcb7c0a" />

Molemmat verkkokortit olivat NAT-yhteydessä. Muutin ```Network Adapter 2``` tilan Host-Onlyyn samalla tavalla, kuin Metasploitablen kanssa.

## c) Harjoittelemme omassa virtuaaliverkossa, jossa on Kali ja Metaspoitable. Osoita testein, että 1) koneet eivät saa yhteyttä Internetiin 2) Koneet saavat yhteyden toisiinsa.

Testasin toimivuuden vielä avaamalla molemmat virtuaalikoneet, sekä pingaamalla toisiinsa ja julkiseen DNS-palveluun.
Kalissa irrotin itseni ensin verkosta: 

<img width="329" height="276" alt="image" src="https://github.com/user-attachments/assets/3744b8a8-b967-46d5-ad18-0856684c41c4" />

- Disconnected!

Kokeilin kalilla pingata ensin 1.1.1.1 DNS-palveluun, ja seuraavaksi Metasploitableen:

<img width="537" height="314" alt="image" src="https://github.com/user-attachments/assets/744e8937-3009-4913-8f81-261115584a79" />

- Ping-komento ei edes mene läpi ulkomaailmaan, sillä verkkoyhteyttä ei ole.

Kokeilin samaa vielä Metasploitablessa, jolta ei pitäisi saada missään vaiheessa yhteyttä Kalia pidemmälle:

<img width="612" height="295" alt="image" src="https://github.com/user-attachments/assets/918b31ce-1a53-4c14-8948-8a20efd0e6d2" />

- Tulokset ovat samoja, joten konfigurointi onnistui.

## d) Etsi Metasploitable porttiskannaamalla (nmap -sn). Tarkista selaimella, että löysit oikean IP:n.

Tässä tehtävässä suoritin porttiskannauksen Metasploitableen Kalilta.

Käytin tehtävänannon mukaisesti parametriä ```-sn```, joka on [Nmapin](https://nmap.org/book/man-host-discovery.html) oman dokumentaation mukaan "Host Discovery" asetus.
Dokumentoinnin mukaan tässä komennossa käytetään IP rangea, joka tässä tapauksessa olisi: ```192.168.32.0/24```, se käy läpi kaikki tämän rangen sisällä olevat osoitteet läpi.

- Erittäin kätevä työkalu etsimään kaikki isännät verkkoalueellasi!

<img width="724" height="319" alt="image" src="https://github.com/user-attachments/assets/8b1c8f43-73a8-4a89-ab13-16eab062fdfc" />

- Tiesin jo aikaisempien verkkoyhteys testien perusteella, että kuvassa maalattu IP-osoite oli Metasploitablen.
- 192.168.32.254 on oma tietokoneeni, 192.168.32.129 on Kali.

Syötin Metasploitablen IP-osoitteen firefoxiin, tarkastaakseni weppipalvelimen olemassaolon:

<img width="574" height="525" alt="image" src="https://github.com/user-attachments/assets/d1fbbf7b-12d8-433c-bcd4-fcf48a0e3f7b" />

- Toimii!

## e) Porttiskannaa Metasploitable huolellisesti ja kaikki portit (nmap -A -T4 -p-). Poimi 2-3 hyökkääjälle kiinnostavinta porttia. Analysoi ja selitä tulokset näiden porttien osalta

Porttiskannasin Metasploitablen komennolla: ```nmap -A -T4 -p- 192.168.32.128```

<img width="619" height="704" alt="image" src="https://github.com/user-attachments/assets/9d683ec2-882b-4f25-b906-4919adc169c5" />

- Valtavan paljon tietoa!

Heti ensimmäiseksi kiinnitin huomiota FTP-palvelimeen. FTP (EI FTPS) on haavoittuva palvelu. FTP ei salaa liikennettä, joten hyökkääjä voi napata välistä dataa miten haluaa. Myös käyttäjientodennus on haavoittuvaa, esimerkiksi voit huoletta brute-forcettaa todennuksen läpi käyttäen automatisoituja työkaluja ([SecurityScorecard. 2025.](https://securityscorecard.com/blog/ftp-security-risks/)). 
Tässä skannaustulokset FTP osalta:
```
| FTP server status:
|      Connected to 192.168.32.129
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
```
- Voit kirjautua tiedostopalvelimelle anonyyminä.
- Kaikki tieto kulkee luettavana tekstinä.

Toinen mielenkiintoinen oli MySQL, sillä nmap tunnisti versioksi: ```5.0.51a-3ubuntu5```, joka pikaisen googletuksen jälkeen paljastui erittäin haavoittuvaksi versioksi.

<img width="627" height="136" alt="image" src="https://github.com/user-attachments/assets/00a4b8e1-84ae-40f9-934e-86b62da0c765" />

Googlettamalla MySQL version, löysin [Ubuntun](https://ubuntu.com/security/notices/USN-897-1) tietoturvatiedotteen, jossa kerrotaan version haavoittuvuuksista.
Haavoittuvuuksia oli esimerkiksi käyttöoikeuksien ohitus & olemassa olevien tiedostojen ylikirjoitus. Myös palvelunesto, sekä cross-site scripting tuli esille.
Nämä haavoittuvuudet olivat todella vakavia, sillä mahdolliset hyökkääjät ovat voineet kaataa palveluita ja kirjoittaa tietokantaan mitä tahansa.

Myös Telnetillä on auki oleva portti koneella. Telnetissä ei ole minkäänlaista salausta, joten hyökkääjä vain kävelee sisälle.


## Lähteet


Buuri, M. 2026. DORA and TLPT testing. Luentomateriaali Haaga-Helia ammattikorkeakoulussa 31.3.2026. Luettavissa: https://terokarvinen.com/buuri-2026-dora-and-threat-lead-penetration-testing/buuri-2026-dora-and-threat-lead-penetration-testing--teros-pentest-course.pdf.

Ubuntu. 2010. USN-897-1: MySQL vulnerabilities. Ubuntu Security Notices. Luettavissa: https://ubuntu.com/security/notices/USN-897-1.

2022/2554. Luettavissa: https://eur-lex.europa.eu/eli/reg/2022/2554/oj/eng.

Karvinen, T. s.a. Tunkeutumistestaus. Kurssisivusto. URL: https://terokarvinen.com/tunkeutumistestaus/. Luettu: 1.9.2026.

Lyon, G. 2009. Host Discovery.Nmap Network Scanning: The Official Nmap Project Guide to Network Discovery and Vulnerability Scanning. Nmap Project. Luettavissa: https://nmap.org/book/man-host-discovery.html.

Rapid7 2012. Metasploitable 2. Intentionally vulnerable Linux virtual machine. SourceForge. URL: https://sourceforge.net/projects/metasploitable/files/latest/download. Luettu: 1.9.2026.

SecurityScorecard 2025. FTP Security Risks, Vulnerabilities & Best Practices Guide. SecurityScorecard Blog. URL: https://securityscorecard.com/blog/ftp-security-risks/. Luettu: 1.9.2026.

Suomen Pankki 2023. TIBER-FI 2.0: Threat Intelligence-based Ethical Red Teaming – Procedures and Guidelines. Suomen Pankki. Luettavissa: https://www.suomenpankki.fi/globalassets/bof/en/money-and-payments/the-bank-of-finland-as-catalyst-payments-council/tiber-fi/tiber-fi-2.0-procedures-and-guidelines.pdf.


