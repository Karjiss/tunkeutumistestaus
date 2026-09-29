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

**_NOTE:_**  Käytin kaikkien tulevien tehtävien parametrien selvitykseen vain ffuf GitHubin [CLI-Flags](https://github.com/ffuf/ffuf/wiki/CLI-flags) osiota.

Aloitin tehtäväsivun ohjeiden mukaisesti lataamalla valmiit wordlistit komennoilla:

```
curl -O https://ffuf.io.fi/wordlists/content.txt
curl -O https://ffuf.io.fi/wordlists/passwords.txt
```

Ajoin testiajon komennolla: ```ffuf -w content.txt -u https://ffuf.io.fi/FUZZ -rate 100```

- Parametrillä: ```-rate 100``` rajoitan pyyntöjä sekunnissa, jotta en kuormita palvelinta sekoilullani.

<img width="376" height="272" alt="image" src="https://github.com/user-attachments/assets/2fdbe738-810b-4458-b785-c28bcda69183" />


- Ffuf antoi tulosteena koko wordlistin takaisin, false-positive.

Tehtävänannossa näytetään parametrejä, joilla flagia tavoitellaan. Tuloksissa yhtenäistä on sanojen määrä, joten kokeilin sanojen filtteröimistä. Vihjeissä myös lukee, että tehtävän piilotetut/tuntemattomat sivut vastaavat 200 eli "OK".

Ajoin komennon: ```ffuf -w content.txt -u https://ffuf.io.fi/FUZZ -ac -rate 400```

- ```-ac``` = Automaattinen kalibrointi, joka automaattisesti filtteröi false-positive vastauksia.
- Nostin ratea vähän.
- Ffuf parametrien opastus löytyy Ffuf:in [CLI-flag](https://github.com/ffuf/ffuf/wiki/CLI-flags) osiosta, sekä ffuf GitHubin README:stä.

<img width="380" height="359" alt="image" src="https://github.com/user-attachments/assets/27e70a44-2ac2-41ac-bd2c-3c66efae2a2d" />

- Tulokset paljon paremmat.
- Listassa useita sivuja, jotka ovat "piilossa".
- Parametrillä: ```-mc 200´´´ olisin voinut matchata tulosteeseen vain statuskoodin 200 omaavat polut, mutta näin sain enemmän tuloksia.

Esimerkkinä ```ffuf.io.fi/.env```-URL:in takana on "arkaluontoista" sisältöä:

<img width="380" height="76" alt="image" src="https://github.com/user-attachments/assets/00cd259b-e4f6-48c0-bd93-6f8e0713e85d" />

- Salasanoja, API-keytä yms.

Toivottavasti olen oikealla polulla tehtävien suhteen, sillä mitään varsinaista ilmoitusta onnistuneesta tehtävästä ei ole.

## c2) The interesting non-200

"**Find the paths that exist but are not linked from anywhere.**"

Vihjeenä oli : "**ffuf matches 200,204,301,302,307,401,403,405,500 by default. Anything outside that list is invisible and nothing tells you it was skipped. Match everything, then filter down.**"

Tehtävänannossa on myös kohta "**flags in play**", jossa on parametrit: ```-mc all``` ja ```-fc```.

```-mc all``` = Match status codes all, eli palauttaa kaikkien statuskoodien tulokset.
```-fc``` = Filter status codes. Tämä taas filtteröi statuskoodeja pois tuloksista.

Ajoin ensin ffuf:in komennolla: ```ffuf -w content.txt -u https://ffuf.io.fi/FUZZ -mc all -rate 400```

<img width="366" height="272" alt="image" src="https://github.com/user-attachments/assets/8733113a-39b1-466b-980a-1d37d830ebdd" />


- Valtava määrä taas false-positivea, sillä koko wordlist palautti matcheja.

Seuraavaksi kokeilin lisäämällä statuskoodien filtteröinnin koodiin 200.

Ajoin komennon: ```ffuf -w content.txt -u https://ffuf.io.fi/FUZZ -mc all -fc 200 -rate 400```

<img width="373" height="254" alt="image" src="https://github.com/user-attachments/assets/d005f77a-9ca6-4f49-9e8c-2cb090a02ba2" />

- Nämä tulosteet näkyvät C1 kohdassa, mutta nyt 200 statuskoodit filtteröitynä ulos.
- Istutetut polut ovat varmaankin ```/.git/``` ja ```/server-status/```.


## c3) Recursion

"**The wordlist holds names, not paths, so C1 found you 13 things and none of them nested. Descending finds more.**"

Vihje: "**Directories redirect to their trailing-slash form, which is the signal ffuf's default strategy keys off. Recursion reuses the same wordlist, so it can only descend into a name the list contains.**"

Käytettävät flagit: ```-recursion, -recursion-depth```

- ```-recursion``` = Scan discovered directories recursively. Ffuf ajaa sanalistoja sisäkkäin poluissa, eli kokeilee niitä polusta polkuun.

- ```-recursion-depth``` = Maximum recursion depth (0 = unlimited). Määrittää, kuinka "syvälle" ffuf yrittää maksimissaan.


Ajoin komennon: ```ffuf -w content.txt -u 'https://ffuf.io.fi/FUZZ' -ac -recursion -recursion-depth 0 -v -rate 400```

- ```-v``` = Verbose output. Yksinkertaistaa tulostusta, en ole varma onko parempaa tapaa saada ffuf tulostetta yksinkertaisemmaksi.

<img width="822" height="372" alt="image" src="https://github.com/user-attachments/assets/96e6518e-4833-4b1e-8243-e40d57d0be7c" />

- Tuloksia oli taas paljon, joten summaan mielestäni oleelliset alle.

Tuloksia:

<img width="1168" height="178" alt="image" src="https://github.com/user-attachments/assets/e63d1fa4-e9c5-435a-9838-f24a50bbf42a" />

- Tämä on ainakin hyvä löydös hyökkääjälle, käyttäjätunnuksia ja hashattyjä salasanoja.

<img width="510" height="87" alt="image" src="https://github.com/user-attachments/assets/370561f5-cbf4-4e5b-82b3-76ad594d1c05" />

- Kvartaalittain olevat vuoden 2026 raportit.

## c4) Virtual hosts

Ffuf.io.fi alla on 3 hostnamea, jotka antavat erilaista sisältöä samalla nimellä. Tehtävässä täytyy löytää ne kolme.

Vihje: "**The keyword does not have to go in the URL. Point -u at the bare domain and put FUZZ in a Host header. Do not point -u at the subdomain: the certificate only covers the bare name, so the handshake is refused and you get no HTTP at all. Everything that is not one of the three falls through to the default site, so you need a filter.**"

Tehtävänannossa on myös käytettävät parametrit:

- ```-H "Host: FUZZ.ffuf.io.fi"``` = Header value. Tähän asetetaan header.

Käytin tässä komentoa: ```ffuf -u https://ffuf.io.fi/ -w content.txt -H "Host: FUZZ.ffuf.io.fi"  -rate 400```

<img width="771" height="551" alt="image" src="https://github.com/user-attachments/assets/8f7e6c8b-843c-4c30-b74f-5ca88bdcb0df" />

- Tulosteena taas false-positivea, poissuljen sen käyttämällä filtteröintiä sanamäärään 377.

```-fw``` = Filter words. Tällä voin filtteröidä sanamäärän mukaan osumia pois.

Uusi komento: ```ffuf -u https://ffuf.io.fi/ -w content.txt -H "Host: FUZZ.ffuf.io.fi" -fw 377  -rate 400```

<img width="819" height="496" alt="image" src="https://github.com/user-attachments/assets/51640407-d552-4a37-af89-028a6fed84ae" />

- Löysin vain yhden, voisiko jollain toisella sanakirjalla löytyä jotain?

Kokeilin eri sanakirjalla, joka löytyy Kalin wordlisteistä polusta: ```/usr/share/wordlists/dirb/big.txt```

Komento: ```ffuf -u https://ffuf.io.fi/ -w /usr/share/wordlists/dirb/big.txt -H "Host: FUZZ.ffuf.io.fi" -fw 3```

<img width="832" height="511" alt="image" src="https://github.com/user-attachments/assets/83889701-7dc5-4b25-ae01-aefac8a7a114" />

- Löysin tällä sanakirjalla enemmän tuloksia, toivottavasti oikeita.
- admin, dev, staging.

## c9) The login you cannot replay (Has preflight! Has CSRF token!)

Tämä vaikutti mielenkiintoisimmalta, sekä haastavimmalta näistä.

Tehtävänä oli päästä admin käyttäjälle sisälle, verkkoselain palauttaa aina 403, riippumatta ajokerroista.

Vihje: "**Every form carries a CSRF token that works exactly once. Fetch a fresh one before each attempt instead of replaying a stale one.**"

Käytin tässä tehtäväsivulla olevia ohjeita, sillä tämä on todella edistynyttä ainakin omasta mielestä!

Loin ```login.raw```-tiedoston komennolla: 

```
cat > login.raw <<'EOF'
GET /login HTTP/1.1
Host: ffuf.io.fi
Accept: text/html

EOF
```

<img width="721" height="217" alt="image" src="https://github.com/user-attachments/assets/9253f053-6a0c-4bc8-9b62-96e308547cc8" />

- Login.raw hakee aina uuden tokenin yritysten jälkeen.

Seuraavaksi syötin ohjeiden mukaisesti terminaaliin komennon: 

```
ffuf -w passwords.txt -u https://ffuf.io.fi/login -X POST \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "csrf_token=CSRFTOKEN&username=admin&password=FUZZ" \
  -preflight login.raw \
  -preflight-var 'CSRFTOKEN:name="csrf_token" value="([a-f0-9]+)"' \
  -preflight-mode per-request \
  -mc 302
```

- ```-preflight login.raw``` = Pyytää aina uuden CRTF-tokenin yrityksen jälkeen.
- ```-preflight-var 'CSRFTOKEN:name="csrf_token" value="([a-f0-9]+)"'``` = Poimii uuden CRTF-tokenin, minkä ```-preflight``` pyytää.
- ```-preflight-mode per-request``` = Ajaa koko komennon uudestaan joka yrityksellä, eli: **-preflight login.raw** --> **preflight-var xxxxxxx** --> **YRITYS** ja sama uudestaan.

-  Nämä parametrien selitykset löytyivät sivulta: "[Preflight and postflight](https://github.com/ffuf/ffuf/wiki/Preflight-and-postflight)".

<img width="736" height="526" alt="image" src="https://github.com/user-attachments/assets/5951e52f-24d1-4360-a8aa-d416e35a8d93" />

- Cracked!

- Tämä on ainakin selvä onnistuminen.

Kirjauduin vielä sisälle tunnuksilla: "admin:vaultline2026".

<img width="765" height="337" alt="image" src="https://github.com/user-attachments/assets/ffbbfe05-dfc2-47a4-83c8-af55e0e17bc6" />




## Lähteet 

ffuf. 2026. ffuf uusin versio. Luettavissa: https://github.com/ffuf/ffuf/releases/tag/v2.3.0

ffuf. 2026. ffuf wiki. Luettavissa: https://github.com/ffuf/ffuf/wiki

ffuf. 2026. ffuf wiki, komentorivi manuaali. Luettavissa: https://github.com/ffuf/ffuf/wiki/CLI-flags

ffuf. 2026. ffuf wiki. Preflight and postflight. Luettavissa: https://github.com/ffuf/ffuf/wiki/Preflight-and-postflight

ffuf. 2026. GitHub-repositorio. Luettavissa: https://github.com/ffuf/ffuf

Hoikkala, J. 2026. Luentokalvot kurssin tunnilta 24.9.2026. Luettavissa: https://io.fi/fuzzing_with_ffuf.pdf

Karvinen, T. 2026. Tunkeutumistestaus kurssisivu. Luettavissa: https://terokarvinen.com/tunkeutumistestaus/

Vaultline Oy. s.a. Tehtävän sanalista. Saatavilla: https://ffuf.io.fi/wordlists/content.txt

Vaultline Oy. s.a Tehtävän salasanalista. Saatavilla: https://ffuf.io.fi/wordlists/passwords.txt

Vaultline Oy. s.a. How to play, ffuf haasteet. luettavissa: https://ffuf.io.fi/play
