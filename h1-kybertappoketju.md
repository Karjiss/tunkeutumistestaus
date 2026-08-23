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

Kali Linux oli jo asennettuna molemmille tietokoneilleni. Pöytäkoneellani käytän uusinta **Kali Rolling 2026.2**-versiota. Läppärilläni käytän **Kali Rolling 2025.2**-versiota. Molemmilla tietokoneilla virtuaalikoneet pyörivät käyttäen VMWare Workstation Pro:ta. Molemmille koneille Kali asennettiin lataamalla "Pre-built VM" [Kalin verkkosivuilta](https://www.kali.org/get-kali/#kali-virtual-machines)

## b) Irrota Kali-virtuaalikone verkosta.
