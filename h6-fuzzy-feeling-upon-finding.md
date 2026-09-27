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

### a) Tallenna itsellesi kopio säännöistä

**Scope**

Harjoituksen kohde on vain ja ainoastaan ffuf.io domain.
