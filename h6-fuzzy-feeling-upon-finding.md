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

Ffuf on Kalissa valmiiksi asennettuna, mutta vanhalla versiolla. Katsoin Ffuf:in [GitHubista](https://github.com/ffuf/ffuf) ohjeet päivitykseen.

Asensin ensiksi golangin komennolla: ```sudo apt install gccgo-go```, jonka jälkeen latasin uusimman ffuf-version komennolla: ```go install github.com/ffuf/ffuf/v2@latest```

<img width="639" height="247" alt="image" src="https://github.com/user-attachments/assets/045551d3-7233-46d6-8302-64d345acb9f8" />

- Ffuf ei kuitenkaan päivittynyt ja install uudelleen herjasi jotain versioista, joten päätin kokeilla ffuf uudelleenasennusta.

Poistin ffuf:in komennolla: ```sudo apt remove ffuf```

<img width="568" height="237" alt="image" src="https://github.com/user-attachments/assets/923e8f1e-89a6-4da3-ac54-aeb2d9b7a4af" />

Latasin pre-built binäärin ffuf GitHubin "[Releases](https://github.com/ffuf/ffuf/releases/tag/v2.3.0)"-osiosta.

<img width="810" height="95" alt="image" src="https://github.com/user-attachments/assets/1a129485-97c1-4917-8a72-0579298abadd" />

Purin sen komennolla: ```tar -xzf ffuf_2.3.0_linux_amd64.tar.gz```

<img width="827" height="160" alt="image" src="https://github.com/user-attachments/assets/ffe31d09-452a-4ed6-952e-408416faab3b" />


Siirsin ffuf kansion latauskansiosta bin-kansioon, jotta sitä voi ajaa mistä tahansa hakemistosta komennolla: ```sudo mv ffuf /usr/local/bin/```

Ajoin komennon: ```ffuf -V``` tarkastaakseni, onnistuinko.

<img width="197" height="60" alt="image" src="https://github.com/user-attachments/assets/471ad45c-4069-47a3-8439-b54004897161" />

- Tadaa! Aikaisempina viikkoina .tar-tiedostojen purkaminen tuli hyödyksi!


## c1) Content discovery


