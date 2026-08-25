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

Aloitin tarkistamalla verkkoadapterin ID:n, sekä sen statuksen komennolla: ```ip a```

<img width="630" height="288" alt="image" src="https://github.com/user-attachments/assets/e494b670-7a37-4332-a19b-bc3aa73089ae" />

- eth0 on verkkoadapterini, jonka sammutan testien ajaksi.

Irrotin virtuaalikoneeni verkosta komennolla: ```sudo nmcli device disconnect eth0```

<img width="1274" height="798" alt="image" src="https://github.com/user-attachments/assets/520ce14a-fd90-4f14-a12f-f01ff7e7019d" />

Pingasin vielä Cloudflaren DNS-palvelinta tarkistaakseni, etten ole vielä verkossa:

<img width="319" height="76" alt="image" src="https://github.com/user-attachments/assets/9e3ace8c-0d13-41a5-968c-ee797346d1ac" />

- Ei verkkoa

Seuraavaksi kokeilin porttiskannata oman verkkoni komennolla: ```nmap -T4 -A 127.0.0.1```

<img width="641" height="298" alt="image" src="https://github.com/user-attachments/assets/5c4a27b2-9e3b-4513-84ba-a6ce3b12fcbd" />

- Skannaus ei tuota hirveästi tulosta, johtuen verkon tilasta. Tietokone ei ole missään yhteydessä "ulkomaailmaan", jonka takia se ei anna juurikaan tietoa.
- Olisin käyttänyt komennossa localhostia 127.0.0.1 sijaan, mutta verkon ollessa pois päältä myöskään DNS-palvelu ei ole käytössä.
- Network distance antaa tuloksen "0", koska pakettien ei tarvitse liikkua minnekään, sillä skannaan omaa verkkoani.

Komennon: ```nmap -T4 -A 127.0.0.1``` parametrit selitettynä:

- **-T4 = Määrittää ajankäytön eli skannauksen nopeuden työkalussa.**

- **-A = Tämä parametri ottaa käyttöön käyttöjärjestelmän tunnistamisen, versiontunnistuksen, skriptiskannauksen sekä tracerouten, jolla voidaan selvittää ip-pakettien "hyppypolku".**

## d) Asenna kaksi vapaavalintaista demonia ja skannaa uudelleen. Analysoi ja selitä erot.

Asensin daemonit ```apache2``` ja ```openssh-server``` komennolla: ```sudo apt install apache2 openssh-server -y```

Tämän jälkeen käynnistin daemonit komennoilla:

```
sudo systemctl start apache2
sudo systemctl start ssh

```
- Muuttamalla komennosta "start" --> "status" voit tarkastaa daemonien tilan.

<img width="485" height="293" alt="image" src="https://github.com/user-attachments/assets/f1da6acd-7095-491d-b723-442a0fc5e08a" />

- Molemmat ovat aktiivisia!

Seuraavaksi ajan aikaisemmin suoritetun porttiskannin uudestaan samoilla parametreillä:

<img width="718" height="437" alt="image" src="https://github.com/user-attachments/assets/213bdfc4-b268-4569-9a58-b3cd5ba185c2" />

- Nyt nmap löysi kaksi avointa porttia! Ne ovat äsken käynnistetyt daemonit.
- Myös käyttöjärjestelmän tunnistus sai dataa auki olevien porttien ansiosta.
- Vaikka laitteella ei pääse verkkoon, nämä daemonit käynnissä ollessa avaavat portin. Siksi sain eri tuloksia kuin aikaisemmin.

## e) **Ratkaise vapaavalintainen kone HackTheBoxista. Omalle tasolle sopiva, useimmille varmaan Starting Pointista. Valitse kone, jota et ole ratkaissut vielä. Ei tunnilla näytetty Meow.**

### HTB lab: "Appointment"

- Hack The Boxin OpenVPN olin jo asentanut valmiiksi. Ohjeet siihen löytää kurssin sivuilta (Karvinen 2026). 


Aloitin tekemällä ensimmäiset tehtävät:

<img width="1259" height="584" alt="image" src="https://github.com/user-attachments/assets/6ad18fc1-1528-42d1-b3b3-d89a5e1aa594" />

Sitten alkaa tehtävät itse koneessa:

<img width="1236" height="178" alt="image" src="https://github.com/user-attachments/assets/5afb8217-9fa8-4ffc-b409-03a798d97e24" />

Yhdistin virtuaalikoneeni OpenVPN yhteyteen ja varmistin, että pingit eivät mene läpi kohteen ulkopuolelle:

<img width="524" height="115" alt="image" src="https://github.com/user-attachments/assets/1093b071-327d-49b7-94dc-0749c8878b83" />

- Paketit eivät mene läpi!

Suoritin nmapilla porttiskannin kohde IP:seen komennolla: ```nmap -T5 -A 10.129.169.133```.

<img width="704" height="443" alt="image" src="https://github.com/user-attachments/assets/bbd47d86-cc96-490f-bbb8-c6822e69f0fd" />

- Porttiskannaus tunnistaa apachen portista 80, sekä sen version.
- Tässä olisi voinut käyttää myös muita parametrejä, kuten: ```-sC -sV```.

<img width="1124" height="865" alt="image" src="https://github.com/user-attachments/assets/551f238c-efc5-4258-af71-e5897464be24" />

<img width="1247" height="310" alt="image" src="https://github.com/user-attachments/assets/842461d1-2b6f-445b-89f0-f92fa92f026d" />
 
Tässä kohtaa avaan verkkoselaimessa kohde IP:n sivun:

<img width="759" height="766" alt="image" src="https://github.com/user-attachments/assets/e7aad479-610f-4ba4-94f3-1908c020e18e" />

Kyseessä on kirjautumissivu, johon pitäisi päästä sisälle SQL-injektiolla.
Voin manipuloida SQL-kyselyä esimerkiksi siten, että käyttäjätunnuksen jälkeen lisään merkit ```'#```. Heittomerkki sulkee koodin merkkijonon ja risuaita tekee SQL-kyselyn loppupätkästä kommentin. Näin pystyisin kirjautumaan admin-tunnuksille ilman oikeaa salasanaa.

<img width="486" height="459" alt="image" src="https://github.com/user-attachments/assets/d43fd405-106b-4c0b-a5ec-26660e29e21a" />

<img width="743" height="228" alt="image" src="https://github.com/user-attachments/assets/eeb5ea0f-9885-42e1-9ca5-4bf1fd95baba" />

- Syöte toimi! Eli syötettyäni käyttäjätunnuksen "Admin", lisäsin sen perään heittomerkin ja risuaidan, joten loppuosa kyselystä jäi turhaksi ja pääsin sisälle!

<img width="1097" height="291" alt="image" src="https://github.com/user-attachments/assets/6afe8a71-f727-48ba-8e63-b589b70b7643" />

<img width="877" height="486" alt="image" src="https://github.com/user-attachments/assets/c1c97153-ede5-4d6f-afc4-2921426d8791" />

## Lähteet

Karvinen, T. 2026. Tunkeutumistestaus. Luettavissa: https://terokarvinen.com/tunkeutumistestaus/

Rhysider, J. 5.11.2024. EP 151: Chris Rock. Dark Net Diaries -podcast. Kuunneltavissa: https://darknetdiaries.com/episode/151/

Hutchins, E. 2011. Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains. Luettavissa: https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf

