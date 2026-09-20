# h5 - Elokuu2026!

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

### Cracking Passwords with Hashcat ([Karvinen 2022](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/))

- Järjestelmät eivät säilytä salasanoja selkeänä tekstinä, vaan tiivisteinä.
- "Hashays" on yksisuuntainen funktio, eli et voi muuttaa tiivistettyä tiedostoa takaisin selkeäksi.
- Salasanojen murtamisessa voit käyttää "sanakirjoja", jotka sisältävät tunnettuja tai vuodettuja salasanoja.
- Hashcat katsoo sanakirjaa ja vertaa sen sisältämiä sanoja kohdetiivisteeseen.


### Crack File Password With John ([Karvinen 2023](https://terokarvinen.com/2023/crack-file-password-with-john/))

-  JohnTheRipper on samankaltainen, kuin Hashcat, mutta monimutkaisempi.
-  Voit käyttää Johnia murtamaan esimerkiksi salasanasuojatun ZIP-tiedoston.
-  JohnTheRipper voi murtaa todella monia eri tiedostomuotoja.


## a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana

