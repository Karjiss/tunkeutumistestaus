<img width="385" height="55" alt="image" src="https://github.com/user-attachments/assets/d1fe1f6f-2704-4f37-a3b2-e8ca8fc874c2" /># h5 - Elokuu2026!

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

Tässä tehtävässä halusin murtaa salasanat, jotka kaappasin tehtävässä [h3-EternalHomework](https://github.com/Karjiss/tunkeutumistestaus/blob/main/h3-EternalHomework.md).

Kopioin salasanatiedoston testihash hakemistoon komennolla: ```cp ~/SUPERHAKKEROINTI/shadow ~/testihash```

<img width="674" height="159" alt="image" src="https://github.com/user-attachments/assets/7955e514-c13d-479c-be16-4e5294e186c4" />

**Tiedoston tuloste:**

<img width="502" height="586" alt="image" src="https://github.com/user-attachments/assets/f86b1905-e6d3-427b-9bf5-adc0ce81bb44" />

- Salasanat on hashatty.

Kopioin yhden tiivisteen tiedostosta ja yritin tunnistaa sen käyttäen komentoa: ```hashid -m '$1$f2ZVMS4K$R9XkI.CmLdHhdUE3X9jqP0:14742:0:99999:7:::'```

<img width="566" height="115" alt="image" src="https://github.com/user-attachments/assets/cdbd422b-0cfe-4871-896d-16911cc420fa" />

- Todennäköisin on MD5-crypt.
- MD5-crypt on MD5, johon on lisätty suola ja avaimen venytys, jotta brute-force olisi haasteellisempaa ([Vidarholen 2011](https://www.vidarholen.net/contents/blog/?p=32))
- Moodi on ```-m 500```


Seuraavaksi kokeilin, onnistuuko hashcat murtamaan "parempaa" MD5-tiivistettä.

Komentona käytin: ```hashcat -m 500 -a 0 shadow rockyou.txt --force```

Annoin komennon ajaa vain pari minuuttia, sillä virtuaalikoneeni ei voi hyödyntää näytönohjainta laskemiseen.

Tulostin löydetyt salasanat komennolla: ```hashcat -m 500 --show shadow```

Vertasin salasanoja käyttäen greppiä:

<img width="1260" height="239" alt="image" src="https://github.com/user-attachments/assets/c7fb3a50-69a3-49cc-8934-645c92925818" />

- Koko rockyou.txt läpikäynti hashcatilla prosessorilla olisi vienyt noin 40 minuuttia.
- Löysin kuitenkin 3 osumaa!

## g) Sanakirja. Oman sanakirjan teko parantaa onnistumismahdollisuuksia. Demonstroi, kuinka teet oman sanakirjan hashcat:n tai john:iin

Loin ensin demotettavan md5 tiivisteen komennolla: ```echo -n "jani" | md5sum```

<img width="295" height="66" alt="image" src="https://github.com/user-attachments/assets/62883038-0528-4b9d-9f1f-a44095422b61" />

Sitten loin sanakirjan microlla komennolla: ```micro dict```, jonne laitoin eri sanoja.

<img width="577" height="273" alt="image" src="https://github.com/user-attachments/assets/30cd081e-327f-43cb-bf40-0b0816d78074" />

Tallensin käyttäen ```CTRL + S``` ja suljin micron ```CTRL + Q```.

Ajoin jälleen hashcatin komennolla: ```hashcat -m 0 -a 0 d5d51a2d88cda585e37315067891381f dict --force```

<img width="672" height="436" alt="image" src="https://github.com/user-attachments/assets/7bd0211e-9749-4fa5-8c9a-39e69a1c1e4d" />

- Pam!

## h) Hash rules. Näytä esimerkki HashCatin sääntöjen käytöstä (rules)

Aiemmissa tehtävissä käytin sääntöä ```-a 0```, joka käyttää sanalistoja. Hashcatissa on kuitenkin useita sääntöjä! Sääntöjä löytyy [Hashcatin wikistä](https://hashcat.net/wiki/doku.php?id=rule_based_attack).

Loin uuden version "jani"-tiedostosta, muuttaen alkukirjaimen isoksi:

<img width="293" height="70" alt="image" src="https://github.com/user-attachments/assets/bcd98d17-1e7e-4e21-ab48-ff80eedb5359" />

Loin tekstitiedoston "jani.rule" komennolla: ```micro jani.rule```

Lisäsin tekstitiedoston sisälle vain "c", joka kertoo hashcatille, että muuttaa alkukirjaimen isoksi ja loput pieneksi.

En tehnyt muutoksia sanalistaan, sillä sääntö hoitaa tämän puolestani. Ajoin sitten hashcat komennon ja säännön päälle komennolla: ```hashcat -m 0 4d6f618f683c460286d04611a1a18d6d dict -r jani.rule --force```

<img width="631" height="453" alt="image" src="https://github.com/user-attachments/assets/85ed7ed5-1f4c-463a-8df4-8f7b5642d0a2" />

- Cracked!
- Sääntö toimii, sillä syntax näyttää hashcatin kokeilevan salasanoja isolla alkukirjaimella, vaikka sanakirjassani kaikki oli pienellä.

Sitten on vielä valmiit säännöt, mitä tulee hashcatin mukana, esimerkiksi "best66", joka kokeilee sanoja väärinpäin.

Tein jälleen uuden variaation janista, tällä kertaa: ```echo -n "inaj" | md5sum```

<img width="297" height="68" alt="image" src="https://github.com/user-attachments/assets/490cd430-e264-4f14-afdc-ed551d744761" />

Ajoin hashcatin käyttäen sanalistaani ja sääntöä, joka kaivetaan hashcatin tiedostoista komennolla: ```hashcat -m 0 cd380509265cfe613fd866cde4822fa3 dict -r /usr/share/hashcat/rules/best66.rule```

<img width="647" height="450" alt="image" src="https://github.com/user-attachments/assets/01000aae-96a2-44c4-ba8f-7879dac82a3b" />

- Cracked!

Sääntöjä on monenlaisia. Voit käyttää valmiiksi rakennettuja, tai rakentaa niitä itse. Erittäin hyödyllistä, jos ideasi ovat loppu murtautuessa!


## Lähteet



