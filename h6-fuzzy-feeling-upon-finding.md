# h6 - Fuzzy Feeling Upon Finding

- Kurssi: [Tunkeutumistestaus](https://terokarvinen.com/tunkeutumistestaus/) (Karvinen 2026)
- Opettaja: Tero Karvinen
- Raportin kirjoittaja: Jani Karjalainen

## Käyttöympäristö

- Käyttöjärjestelmä: Microsoft Windows 10 Home
- Emolevy: Gigabyte Z170-Gaming K3
- Prosessori: Intel i5-6600K
- Näytönohjain: NVIDIA GeForce RTX 2060
- RAM: 16 GB DDR4
- Virtualisointiohjelmisto: **VMWare Workstation Pro**

## x) Lue ja tiivistä

### **Fuzzing with Ffuf** ([Hoikkala 2026](https://io.fi/fuzzing_with_ffuf.pdf))

- Ffuf on HTTP-bruteforce monitoimityökalu.
- "Avoid magic - stay idiomatic and understandable", vältetään turhaa automaatiota.
  - Mielestäni tämä auttaa myös käyttäjää oppimaan asioista paljon enemmän, kun sinun täytyy myös ymmärtää, eikä työkalu vain tee kaikkea puolestasi.
- Ffuf tunnistaa poikkeamia eri osa-alueilla.
  - Ffuf pitää sisällään niin paljon hyödyllisiä asioita, että sitä on mahdoton tiivistää tänne.


## [Vaultline](https://ffuf.io.fi/play)

## a) Tallenna itsellesi kopio säännöistä

## Säännöt

- **Scope**

  - Harjoituksen kohde/scope on vain ja ainoastaan ```https://ffuf.io.fi```-domain. Työkalujen käyttö ja "hyökkäys" verkkosivun ulkopuolelle on ulkona scopesta.

- **Rules of engagement**
  - Käytän kohteen testaamiseen vain fuzzaus työkalua "ffuf", enkä kohdista muita hyökkäyksiä kohteeseen.
  - Käytän vain taskien tekemiseen tarvittavia menetelmiä.

- **Mihin oikeutesi tehdä tietoturvatestausta tähän kohteeseen perustuu?**
  - Oikeuteni tehdä testausta perustuu Hoikkalan luentopuheeseen, opettajan antamaan tehtävänantoon sekä [demo-sivulla](https://ffuf.io.fi) olevaan tekstiin: "```FUZZING DEMO TARGET. This is not a real product. Every finding on this host is planted for a talk demo.```".

- **Riskit ja mitigointi**
  - Mahdollinen palvelunestohyökkäys on yksi, mitä voi tapahtua. Tämän mitigoimiseksi rajoitan ffuf:in pyyntömääriä parametreillä kuten: ```-p``` ja ```-r```.
  - Väärä kohde-URL on myös riski, josta voi aiheutua lain kannalta ongelmia. Tämän riskin poistamiseksi minun täytyy olla erittäin huolellinen syötettäessäni kohde-URLia ffuf:iin.

## b) Asenna ffuf versio, joka tukee aivan uutta preflight-ominaisuutta.

