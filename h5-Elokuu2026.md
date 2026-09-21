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

Hashcat oli jo valmiiksi asennettuna Kali-koneellani, mutta sen ja Hashid:n voi asentaa komennoilla:
```
sudo apt-get update
sudo apt-get -y install hashid hashcat wget
```

Loin testiä varten testihakemiston ja siirryin sinne komennoilla: ```mkdir testihash``` ja ```cd testihash```

<img width="248" height="105" alt="image" src="https://github.com/user-attachments/assets/68b04f37-295c-46c1-a80c-b04df7e84a31" />

Sanalistan saa ladattua komennolla: ```wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz```

Kali linuxilla kuitenkin voin käyttää "rockyou.txt"-tiedostoa komennolla: ```wordlists``` --> ```cp rockyou.txt ~/testihash```

- Tämä kopioi sanalistan ```wordlists```-hakemistosta kotihakemistooni.

<img width="536" height="380" alt="image" src="https://github.com/user-attachments/assets/2f49556c-ee08-4ee3-b8e9-31fbcb67689b" />

Loin testitiedoston ja hashasin sen komennolla: ```echo -n "secret" | md5sum```

<img width="310" height="58" alt="image" src="https://github.com/user-attachments/assets/33e6d3ba-0037-4360-a455-83c7a9aea4ab" />

- ```-n```-parametri ei tulosta loppuun oletuksena tulevaa rivinvaihtoa.

Käytin Hashid:tä antamaan arvion, minkä tyyppisestä hashista on kyse (Vaikka minähän sen hashin tein).

```hashid -m 5ebe2294ecd0e0f08eab7690d2a6ee69```

<img width="471" height="350" alt="image" src="https://github.com/user-attachments/assets/bbdc9e7c-a4cf-473b-9f71-96b64fe070e6" />

- Top 3 vaihtoehdot ovat : MD2, MD5 ja MD4.
- MD2 on todella harvinainen, joten mennään MD5!

Ajoin Hashcatin komennolla: ```hashcat -m 0 -a 0 5ebe2294ecd0e0f08eab7690d2a6ee69 rockyou.txt --force```

- ```-m``` = Valitaan tiivisteen formaatti.
- ```-a``` = Valitaan hyökkäystapa, vaihtoehto "0" käyttää sanalistan kaikki sanat.
- ```--force``` = Ohittaa kaikki varoitukset.

<img width="707" height="351" alt="image" src="https://github.com/user-attachments/assets/501fc7c2-c1f5-4614-8842-d6ae4a5c768d" />

- Salasana "secret" on aika yleinen, joten se löytyy rockyou.txt listalta.


## c) Asenna John the Ripper ja testaa sen toiminta murtamalla jonkin esimerkkitiedoston salasana

<img width="711" height="219" alt="image" src="https://github.com/user-attachments/assets/9e10f14e-2947-4a68-a8bd-406a9b7439ae" />

- John oli myös valmiiksi asennettuna Kalilla.

Latasin Teron testi ZIP-tiedoston koneelleni komennolla: ```wget https://TeroKarvinen.com/2023/crack-file-password-with-john/tero.zip```

<img width="715" height="303" alt="image" src="https://github.com/user-attachments/assets/04e7e702-cbc5-4f83-973e-b79fa52754d9" />

Kokeilin purkaa Teron tiedoston, mutta sehän oli salasanasuojattu!

<img width="482" height="146" alt="image" src="https://github.com/user-attachments/assets/acf81200-2cb5-426c-8d08-8f49e01f5531" />

Käytin Johnia purkamaan ```tero.zip``` --> ```tero.zip.hash``` komennolla: ```zip2john tero.zip > tero.zip.hash```

<img width="710" height="362" alt="image" src="https://github.com/user-attachments/assets/00b07ab3-edae-4e74-8e37-d642e7539486" />

Seuraavaksi kokeilin Johnin hyökkäystä tiedostoon komennolla: ```john tero.zip.hash```

<img width="709" height="244" alt="image" src="https://github.com/user-attachments/assets/d6f940bc-7e5e-4075-b6fb-e080ab774669" />

- Johnin mukaan salasana on "butterfly".
- Voin katsella kaikki hyökkäyksessä murretut salasanat komennolla: ```john tero.zip.hash --show```, vaikkakin tässä on vain 1.
- John käyttää oletus sanalistoja omista tiedostoistaan: ```/usr/share/john/password.lst```.

Kokeilen pääsyä Teron tiedostoon salasanalla "butterfly":

<img width="364" height="97" alt="image" src="https://github.com/user-attachments/assets/e6d54ef7-1dfd-4014-8cdd-7c3ce99b1b07" />

<img width="708" height="250" alt="image" src="https://github.com/user-attachments/assets/4c367de3-fb01-4555-b269-782c0561f3b5" />

- Jee!


## e) Tiedosto. Tee itse tai etsi verkosta jokin salakirjoitettu tiedosto, jonka saat auki. Murra sen salaus

Latasin [OpenWallista](https://openwall.info/wiki/john/sample-non-hashes) "sample pdfdump created using JtR's pdf2john tool" -[tiedoston](https://openwall.info/wiki/_media/john/pdfdump.tar).

- [OpenWall](https://openwall.info/wiki/john) verkkosivu on JohnTheRipperin yhteisö-resursseille.

Kopioin tiedoston /Downloads -hakemistosta testihakemistooni komennolla: ```cp pdfdump.tar ~/testihash```

<img width="801" height="84" alt="image" src="https://github.com/user-attachments/assets/64334bc5-8cef-45e4-9e50-5f22ee2a88bc" />

Purin tiedoston komennolla: ```tar -xvf pdfdump.tar```

<img width="596" height="132" alt="image" src="https://github.com/user-attachments/assets/e0ebfdfa-c9e2-4b8e-b90e-1fdc48888383" />

Löysin parametrit [tar-manuaalista](https://man7.org/linux/man-pages/man1/tar.1.html).

- ```x``` =  Purkaa tiedoston.
- ```v``` =  Aktivoi yksityiskohtaisen listauksen.
- ```f``` = Parametri, jonka jälkeen syötetään purettava tiedosto.

<img width="829" height="281" alt="image" src="https://github.com/user-attachments/assets/4cd8fcd9-3636-4441-b794-7afb38ba22d3" />

Tämän olisi varmasti voinut tehdä jotenkin myös käyttäen suoraan Johnia, mutta mennään näillä!

Seuraavaksi kokeilin murtaa salasanat Johnilla komennolla: ```john pdfdump```

<img width="741" height="263" alt="image" src="https://github.com/user-attachments/assets/e9821a93-b838-4c8f-b6ce-f5f809f609ff" />

- Sehän toimi.

## f) Tiiviste. Tee itse tai etsi verkosta salasanan tiiviste, jonka saat auki. Murra sen salaus


