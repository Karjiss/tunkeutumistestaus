# h1 - Kybertappoketju

- Kurssi: [Tunkeutumistestaus](https://terokarvinen.com/tunkeutumistestaus/) (Karvinen 2026)
- Opettaja: Tero Karvinen
- Raportin kirjoittaja: Jani Karjalainen


 ## x) Lue/katso/kuuntele ja tiivistä

 ### Dark Net Diaries - Episode 151: Chris Rock ([Rhysider, J. 2024](https://darknetdiaries.com/episode/151/))

- **Chris Rock kertoo jaksossa "black hat"-töistään, tässä tapauksessa hacking-for-hire työstä lähi-idän asiakkaalle, joka epäili jonkun ryöstävän häneltä rahaa.**
  - Valitsin tämän jakson, sillä tällainen "mercenary" tyylinen hakkerointi kuulosti mielenkiintoiselta.
 
- **Hakkeroidessa Chris ja hänen tiiminsä reititti liikenteensä useiden eri maiden väliltä, pääpointtina jokaisen hypyn välisen maan huonot välit, koska he eivät halua jakaa tietoja toisilleen kysyttäessä.**
  - Tämä oli mielestäni erittäin mielenkiintoinen tapa toimia, sillä yrittäessä pysyä piilossa joudut piilottelemaan jälkiäsi. Jos hypit esimerkiksi sodassa olevien maiden välillä ennen kohdetta, tiedonjako on todennäköisesti erittäin huonoa. Lisäksi tämä vaikuttaa todella työläältä, sillä joudut tunkeutumaan eri laitteisiin useassa eri maassa, vain voidaksesi reitittää hyökkäyksesi niiden läpi.
  - Haastattelu saa minut myös miettimään, luokitellaanko Chris black-hatiksi vai gray-hatiksi? Sillä kyllä, hän rikkoo lakia tehdäkseen rahaa, mutta samaan aikaan hän tekee tätä rikollisille, jotka esimerkiksi ovat varastaneet rahaa.

### Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains ([Hutchins ym, 2011](https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf))

- **Tutkimus esittää menetelmän, jossa estetään hyökkäyksiä ennakoivasti, eikä vanhanaikaisesti reaktiivisesti.**
  - Olen samaa mieltä, että reaktiivinen puolustus on vanhanaikaista, hyökkäys on jo tapahtunut jos joudut reagoimaan siihen jälkijunassa. Hyökkääjien analysointi myös vahvistaa omaa puolustustasi, sillä voit ennakoida mahdollisia tapoja ja keinoja tulevaisuudessa paremmin.

- **Hyökkäyksellä on 7 vaihetta, joista kaikkien pitää toteutua, jotta hyökkäys onnistuu. Puolustuksen täytyy katkaista ketju vain kerran, jotta hyökkäys estetään:**

```

1. Reconnaissance

2. Weaponization

3. Delivery

4. Exploitation

5. Installation

6. Command and Control (C2)

7. Actions on Objectives

```

### The Art of Hacking: 4.3 Surveying Essential Tools for Active Reconnaissance ([Santos ym, 2019](https://www.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/)) 

- **Passiivisessa tiedustelussa keräät tietoa ilman, että jätät jälkiä itsestäsi, kun taas aktiivisessa esimerkiksi skannaat portteja, sekä jätät jälkiä/lokeja. Toisinsanoen tiedusteltava ei tiedä passiivisesta tiedustelusta, mutta kykenee huomaamaan jos keräät tietoa aktiivisesti.**

- **Monipuolisin työkalu porttien skannaukseen on ```nmap```. Sitä voidaan käyttää tunnistamaan palveluita, käyttöjärjestelmiä tai jopa heikkouksia.**
  - Erittäin tärkeä työkalu hakkeroinnissa!


## a) Asenna Kali virtuaalikoneeseen.

Kali Linux oli jo asennettuna molemmille tietokoneilleni. Pöytäkoneellani käytän uusinta **Kali Rolling 2026.2**-versiota. Läppärilläni käytän **Kali Rolling 2025.2**-versiota. Molemmilla tietokoneilla virtuaalikoneet pyörivät käyttäen VMWare Workstation Pro:ta. Molemmille koneille Kali asennettiin lataamalla "Pre-built VM" [Kalin verkkosivuilta](https://www.kali.org/get-kali/#kali-virtual-machines).

## b) Irrota Kali-virtuaalikone verkosta.

Aloitin tarkistamalla verkkoadapterin ID:n, sekä sen statuksen komennolla: ```$ ip a```

<img width="719" height="537" alt="image" src="https://github.com/user-attachments/assets/565e4507-ad4c-46f9-a135-2eea4d2f0448" />

- eth0 on verkkoadapterini, jonka sammutan testien ajaksi.

Irrotin virtuaalikoneeni verkosta komennolla: ```$ sudo nmcli device disconnect eth0```

<img width="1274" height="798" alt="image" src="https://github.com/user-attachments/assets/520ce14a-fd90-4f14-a12f-f01ff7e7019d" />

Pingasin vielä Cloudflaren DNS-palvelinta tarkistaakseni, etten ole vielä verkossa:

<img width="319" height="76" alt="image" src="https://github.com/user-attachments/assets/9e3ace8c-0d13-41a5-968c-ee797346d1ac" />

- Ei verkkoa

Seuraavaksi kokeilin porttiskannata oman verkkoni komennolla: ```$ nmap -T4 -A 127.0.0.1```

<img width="641" height="298" alt="image" src="https://github.com/user-attachments/assets/5c4a27b2-9e3b-4513-84ba-a6ce3b12fcbd" />

- Skannaus ei tuota hirveästi tulosta, johtuen verkon tilasta. Tietokone ei ole missään yhteydessä "ulkomaailmaan", jonka takia se ei anna juurikaan tietoa.
- Olisin käyttänyt komennossa localhostia 127.0.0.1 sijaan, mutta verkon ollessa pois päältä myöskään DNS-palvelu ei ole käytössä.
- Network distance antaa tuloksen "0", sillä pakettien ei tarvitse liikkua minnekään, sillä skannaan omaa verkkoani.

Komennon: ```$ nmap -T4 -A 127.0.0.1``` parametrit selitettynä:

- **-T4 = Määrittää ajankäytön työkalussa, tässä tapauksessa toisiksi nopein template.**

- **-A = Tämä parametri ottaa käyttöön käyttöjärjestelmän tunnistamisen, versiontunnistuksen, skriptiskannauksen sekä tracerouten, jolla voidaan selvittää ip-pakettien "hyppypolku".** 
