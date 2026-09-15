# h4 - Täysin Laillinen Sertifikaatti

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

### OWASP Top 10, 2021. Broken Access Control ([OWASP Top 10 Team. 2021](https://top10.owasp.org/2021/A01_2021-Broken_Access_Control/))

- Broken Access Control (Rikkinäinen pääsynhallinta) nousi vuonna 2021 viidenneltä sijalta ensimmäiseksi.

- Pääsynhallinnan tarkoitus on estää käyttäjiä toimimasta niiden oikeuksien yläpuolella.

- Kehittäjien ja laadunvarmistajien tulisi testata pääsynhallintaa osana työtään.


### PortSwigger Academy ([PortSwigger LTD 2026](https://portswigger.net/web-security))

**[Insecure Direct Object Reference](https://portswigger.net/web-security/access-control/idor)**

- IDOR on pääsynhallinnan haavoittuvuus, jossa sovellus käyttää käyttäjän syötettä objektien käsittelyyn, vaikka käyttäjällä ei olisi oikeuksia.

- IDOR liitetään usein horisontaaliseen oikeuksien laajentamiseen (esim. Toisen samantasoisen käyttäjän tietoihin pääsy), mutta se voi esiintyä myös vertikaalisesti.

- Esimerkkinä URL = ```https://insecure-website.com/customer_account?customer_number=132355```, josta hyökkääjä voisi muuttaa kohtaa ```customer_number``` joksikin toiseksi päästäkseen toisen käyttäjän tilille.


**[Path traversal](https://portswigger.net/web-security/file-path-traversal)**

- Haavoittuvuus, jossa hyökkääjä saa mahdollisuuden lukea kohteen tiedostoja, joihin hyökkääjällä ei pitäisi olla pääsyä.

- Path traversal haavoittuvuus syntyy, kun verkkosovellus käyttää käyttäjän syötettä, esimerkiksi tiedostonimeä URL:issa.

- Hyökkääjä voi sitten hyödyntää haavoittuvuutta liikumalla ylöspäin hakemistossa käyttäen merkkejä, kuten ```../``` Linuxilla, tai ```..\``` Windowsilla.

**[Cross-Site Scripting](https://portswigger.net/web-security/cross-site-scripting)**

-  XSS (Cross-Site Scripting) on haavoittuvuus, joka antaa hyökkääjälle mahdollisuuden ajaa haitallista JavaScript koodia uhrien selaimessa.

 **Cross-Site Scripting päätyypit**:
  - **Reflected cross-site scripting** = Verkkosovellus vastaanottaa tietoa HTTP-pyynnöstä ja sisällyttää sen vastaukseensa.

  - **Stored cross-site scripting** = Verkkosovellus vastaanottaa tietoa ei luotetuista lähteistä, sitten sisällyttää sen tulevissa HTTP-vastauksissa.

  - **DOM-based cross-site scripting** = Verkkosovellus sisältää client-side JavaScript koodia, joka sisältää tietoa epäluotettavasta lähteestä, yleensä kirjoittaen dataa takaisin DOM-puuhun (Ohjelmointirajapinta).

  
## a) Totally Legit Sertificate

Tässä tehtävässä asensin OWASP ZAPin Kali Linuxille. 

Kali Linuxilla lataus hoitui helposti komennolla ```sudo apt-get install zaproxy```:

<img width="712" height="433" alt="image" src="https://github.com/user-attachments/assets/ac9b5c41-b243-4758-ad10-4f6ce8fd9d58" />

Käynnistin ZAPin ```zap```-komenolla. Sitten selvitinm miten luodaan CA-Sertifikaatti ZAP:issa. Löysin ohjeet ZAP:in sivuilta ([Zaproxy s.a](https://www.zaproxy.org/docs/desktop/addons/network/options/servercertificates/)).

ZAP:issä valitsin ylävalikosta ```Tools --> Options```.

<img width="793" height="594" alt="image" src="https://github.com/user-attachments/assets/2bfa6d34-4da4-4693-a5cf-efeb21a45568" />

Kirjoitin asetusten hakuun "CA Certificate":

<img width="732" height="334" alt="image" src="https://github.com/user-attachments/assets/12ae79f5-bb14-474c-97e8-56e20e797d51" />

- Polku = ```Network --> Server Certificates```
- Kohde löytynyt!

Tallensin sertifikaatin kotihakemistooni painamalla: ```Save```-nappia.

<img width="694" height="589" alt="image" src="https://github.com/user-attachments/assets/2bce4e7f-8b63-476c-a724-3a25b7972e77" />


CA-sertifikaatti piti asentaa vielä selaimelle, joten hommiin!

Hakkasin päätä näppäimistöön hetken aikaa, kunnes löysin tehtävien [vinkkiosiosta](https://terokarvinen.com/tunkeutumistestaus/) opastuksia.

Avasin Firefoxin "Settings" valikon, josta hain "Certificates".

<img width="1009" height="727" alt="image" src="https://github.com/user-attachments/assets/92986953-4869-4515-85c1-f4898d3c77e4" />

- Huokaisin helpotuksesta.

Seuraavaksi klikkasin: ```View Certificates``` --> ```Import```

<img width="656" height="461" alt="image" src="https://github.com/user-attachments/assets/970ce50c-973d-4758-9c05-322fbde5b897" />

Import välilehdeltä valitsin tallentamani CA-Sertifikaatin kotihakemistostani.

<img width="788" height="309" alt="image" src="https://github.com/user-attachments/assets/445d1193-0370-496a-8531-4f8432a54192" />

- Annoin ZAProxylle oikeudet tunnistaa nettisivuja, mutta en sähköposteja.

Painoin ```OK```, sitten tarkistin vielä sertifikaattilistalta CA-sertifikaatin olemassaolon.

<img width="665" height="470" alt="image" src="https://github.com/user-attachments/assets/edb14926-cccb-414e-ad90-a3ce50dbf43b" />

- Siellä lepää.

Nyt ZAP piti saada tallentamaan kuvia, sekä todistaa sovelluksen toimivuus.
Teron vinkeissä oli riittävät ohjeet kuvankaappauksia varten. 

ZAP asetuksissa: ```Tools --> Display```, ja ruksi ruutuun: ```Process Images in HTTP requests/responses```

<img width="748" height="582" alt="image" src="https://github.com/user-attachments/assets/54249612-f8dd-41ff-b91f-af17ea0af434" />

Kävin myös säätämässä vinkkien mukaisesti Firefoxin konfiguraatiosta kohdan: ```network.proxy.allow_hijacking_localhost``` "true"-vaihtoehtoon.

<img width="980" height="219" alt="image" src="https://github.com/user-attachments/assets/7ce4777e-31f8-483f-90ac-206f3e94b191" />

Aloitin uuden session klikkaamalla ```File --> New Session``` ja ```Ok```. Valitsin vaihtoehdon "No, I do not want to persist this session at this moment in time".

<img width="1269" height="720" alt="image" src="https://github.com/user-attachments/assets/93d2755c-e5bd-4eef-b0cd-2a12b1a213b7" />

Syötin myös Quick Startissa olevaan "URL to explore"-kohtaan Metasploitable2 IP-osoitteen ja painoin ```Launch Browser```.

Selailin vähän ja katsoin ZAPin tuloksia:

<img width="1758" height="840" alt="image" src="https://github.com/user-attachments/assets/ab5f845e-d54c-4ba6-aa8f-85aa22268e95" />

- Paljon HTTP-pyyntöjä.

## b) Kettumaista

Tässä tehtävässä asensin Firefoxiin lisäosan nimeltä: "[FoxyProxy Standard](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/) (Jung, E. 2026)".

Linkin takaa löytyy Firefoxin laajennuskaupan sivu, josta latasin FoxyProxyn klikkaamalla kohdasta: ```Add to Firefox```.

<img width="477" height="172" alt="image" src="https://github.com/user-attachments/assets/ce7dfbef-bd43-42f5-986b-b8ca543712da" />

Tehtävänä oli myös lisätä ZAP FoxyProxyn proxyksi, joten menin muuttamaan FoxyProxyn asetuksia:

Firefoxin oikeasta yläkulmasta klikkaamalla FoxyProxyn kuvaketta ja sitten ```Options``` päästään asetuksiin.

Tässä vaiheessa kaipasin jonkin verran apua, sillä aivot löivät tyhjää "Proxy by Patterns" osalta. Löysin kuitenkin viimevuoden toteutuksen [raportin](https://github.com/veitim/tunkeutumistestaus/blob/main/h2_t%C3%A4ysin_laillinen_sertifikaatti.md) (Veijalainen 2026) josta löysin asetukset FoxyProxyyn:


<img width="956" height="454" alt="image" src="https://github.com/user-attachments/assets/d34696b3-6322-483e-899e-9a41e6e920b9" />


- ZAP haetaan localhostista portista 8080.
- Lisätään "Proxy by Patterns" -osioon IP-osoitteet, joista halutaan tietoa ZAP:piin (Metasploitable ja PortSwigger).

Nyt sitten kokeilemaan!

Oikeasta yläkulmasta valitsen FoxyProxin kuvakkeen, josta otan käyttöön ```Proxy by Patterns``` -vaihtoehdon.



Seuraavaksi kokeilin ensin Metasploitablen osoitetta, jonka jälkeen kokeilin Googlen. Ymmärtääkseni nyt ZAP:in pitäisi saada tieto vain Metasploitablesta, mutta ei mistään muusta.

<img width="1336" height="711" alt="image" src="https://github.com/user-attachments/assets/c5e4703c-d3d1-408b-b763-99e552521005" />

- ZAP saa dataa Metasploitablen osoitteesta.

<img width="1403" height="714" alt="image" src="https://github.com/user-attachments/assets/65f94c57-70d1-4d31-9b0e-087c7a8b0965" />

- ZAP ei saa dataa Googlen osoitteesta, eli kaikki toimii.

## PortSwigger Labs - Ratkaise tehtävät & Selitä ratkaisusi

Seuraavaksi aloin ratkaisemaan PortSwiggerin labroja, niitä on tullut aikaisemmilla toteutuksilla ja vapaa-ajalla pari selvitettyä.

### c) [Reflected XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)

  Labrojen aiheista löytyy hyvin tietoa tehtävän x) lähteistä. ALoitin tämän labin kirjautumalla PortSwiggeriin tunnuksillani.
  
  Tehtävänannon mukaan halutaan toteuttaa "scripting-attack", joka sisältää ```alert```-funktion.

  Selityksiä ja erilaisia hyökkäyksiä löytyy [OWASP](https://community.owasp.org/attacks/xss/):in dokumentaatiosta aiheesta.
  
  Labran verkkosivulla on mahdollista kirjoittaa koodia hakukenttään ja se myös näkyy suoraan urlissa muodossa: ```https://0aec001a03a07dc780bc4e5200fe0014.web-security-academy.net/?search=kissatkoiria```.
  Voit siis ajaa koodia suoraan urlista tai hakukentästä.  kokeilin hakukentän kautta koodin ajamista hakusanalla/koodilla: ```<script>alert('haloohaloo')</script>```

  <img width="516" height="172" alt="image" src="https://github.com/user-attachments/assets/695aeb15-6298-475b-b0ed-d1f8d22cfcbf" />

  - Ajo onnistui, luoden "hälytyksen" verkkosivulta käyttäjälle.
  - Saman koodin pystyy syöttämään myös URL:iin ```?search=``` jälkeen.
  
  <img width="1277" height="208" alt="image" src="https://github.com/user-attachments/assets/21e5b722-8572-49e3-81eb-1fd998331f5a" />

  - Labra selvitetty!

### d) [Stored XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)

Tiivistelmässäkin mainitsemani Stored XSS on heikkous, jossa hyökkääjä tallentaa koodia palvelimelle, sitten se jää sinne muiden uhrien ajettavaksi aina, kun sivu avataan.

Tässä labrassa on Stored XSS heikkous ja tehtävänannossa pyydetään tulostamaan ```alert```-funktio kommenttikentän kautta. Eli haitallinen koodi voidaan tallentaa kommenttikenttään ja kaikki käyttäjät ajavat sen sivun auetessa.

<img width="768" height="556" alt="image" src="https://github.com/user-attachments/assets/3c6e4347-39e5-42fc-80d9-2cfc84475824" />



Kokeilin samankaltaista hyökkäystä tähän labraan, kuin aikaisempaankin.

Avasin labrasivulla ensimmäisen postauksen, josta siirryin kommenttiosioon.

Kirjoitin kommenttikenttään koodin: ```<script>alert()</script>```. Oikeassa tapauksessa sisälle voitaisiin tehdä esimerkiksi evästeitä kaappaava skripti. Tämä on haitaton "testi"-skripti. Muita esimerkkejä löytyi OWASP:in [Web Security Testing Guidesta](https://owasp.github.io/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting.html)(OWASP s.a).

<img width="813" height="615" alt="image" src="https://github.com/user-attachments/assets/5f0d3675-88fb-402a-af8c-60ba1609d729" />

<img width="518" height="128" alt="image" src="https://github.com/user-attachments/assets/24d50453-2e94-44be-a0fe-46acd877936c" />

- Ponnahdusikkuna pomppaa nyt aina, kun avaan tämän postauksen.
- Lippu saatu!


### e) Selitä esimerkin avulla, mitä hyökkääjä hyötyy XSS-hyökkäyksestä

Aikaisemmassa tehtävässä jo mainitsin ```alert```-funktiosta testauksena. Se on nopeasti kirjoitettava vaaraton testiskripti. Jos se menee läpi, voit kokeilla jotain vaarallisempaa.

Mediumin ([Bhatt 2025](https://medium.com/@adityabhatt3010/the-art-of-xss-hacking-from-basics-to-advanced-exploits-6a3276f81aaa)) artikkelissa mainitaan esimerkiksi näistä evästeitä kaappaavista skripteistä, joilla voidaan saada muiden tilit omaan käyttöön. XSS-hyökkäyksiä on valtava määrä, yksinkertaisia ja monimutkaisia. Tällainen haavoittuvuus on erittäin vakava, sillä jossain miljoonien käyttäjien verkkosivuilla ajettu hyökkäys voi vaaraantaa kaikkien käyttäjätilit, ellei jopa enemmän.

## PortSwigger - Path traversal

### f) [File path traversal, simple case](https://portswigger.net/web-security/file-path-traversal/lab-simple)

Path traversal on haavoittuvuus joka syntyy, kun verkkosovellus käyttää syötettä suoraan tiedostojen avaamiseen tai lukuun ([PortSwigger 2026](https://portswigger.net/web-security/file-path-traversal#what-is-path-traversal)).

Tässä labrassa tehtävänä on saada tulostettua ```/etc/passwd```-tiedosto. Tiedostoon pitäisi päästä tuotekuvien kautta.
File traversalia olen joskus harjoitellut [pws.college](https://pwn.college/) Linux-ympäristössä.

Aloitin avaamalla ZAP:in, jonka jälkeen avasin labrasivulla kuvan saadakseni proxyyni dataa.

<img width="1081" height="474" alt="image" src="https://github.com/user-attachments/assets/167e8d2d-40ef-48c0-9b75-a04cd96470ce" />

Klikkasin ```GET```-pyyntöä ja painoin näppäimistöltäni näppäimiä: ```CTRL + W``` avatakseni "Requester" näkymän.

<img width="523" height="254" alt="image" src="https://github.com/user-attachments/assets/edd2a471-c679-4266-87cb-6e229f4229eb" />

- Muuttamalla maalattua ```filename=```-kohtaa pystyn manipuloimaan kohteesta tulevia tiedostoja.

Voit liikkua hakemistopolkuja ylöspäin käyttäen ```../```-polkua. 

Päästäkseni ylöspäin juurihakemistoon ja sieltä ```/etc/passwd```, voin kokeilla: ```../../../etc/passwd```. Täten pompin ylöspäin ja sitten lähden hakemistoihin, mihin haluan.

Korvasin ```filetype```-kohdan:

<img width="513" height="249" alt="image" src="https://github.com/user-attachments/assets/a47c1ad2-85fc-476c-85e3-875a1c7340d8" />

Ajoin pyynnön ja tarkastelin "History"-osion syötteen, muutin myös kohdan: ```Body:``` tekstiksi.

<img width="1271" height="759" alt="image" src="https://github.com/user-attachments/assets/65941fd0-60bc-4b6a-932e-396109ced837" />

- Kappas vain, siellähän on salasanoja!

<img width="659" height="193" alt="image" src="https://github.com/user-attachments/assets/6ea45fb3-4fa1-492c-9bcf-c885c55ad36a" />

- Lippulappu!


### g) [File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)

Tämä haavoittuvuus on samanlainen, mutta suorat polut ovat estetty, eli ei ylöspäin liikkumista ```../../../``` hyödyttäen.

Aloitin samalla tavalla kuin aiemmin, eli avaan kuvatiedoston ja tarkastelen URL:ia ja ZAP-dataa:

<img width="890" height="550" alt="image" src="https://github.com/user-attachments/assets/abffcaa3-3d8e-475f-9c5f-a399b5b84407" />

- Samanlainen tilanne kuin aiemmin, mutta suorat polut on estetty.

Tehtävänannossa kuitenkin kerrottiin, että path traversal on "blocked" mutta tiedostoja käsitellään oletushakemistossa. PortSwiggerin [path traversal](https://portswigger.net/web-security/file-path-traversal#what-is-path-traversal) osiossa myös mainittiin, että voit päästä absoluuttisen polun avulla lipullesi, vaikka suora polku olisi estetty. Kokeilin siis juurihakemistosta absoluuttista ```/etc/passwd```-polkua ja etenin samalla tavalla, kuin aiemmassa tehtävässä:

<img width="592" height="406" alt="image" src="https://github.com/user-attachments/assets/e10e6327-b6fc-43e9-84f7-0a8a82168d05" />


<img width="1015" height="196" alt="image" src="https://github.com/user-attachments/assets/7861dc25-6544-4514-8745-7a7076cd9be0" />

- Olin vähän hämmentynyt tämän tehtävän takia, kun tuntui olevan helpompi kuin aikaisempi.
- Lippu kuitenkin saatu.

### h) [File path traversal, traversal sequences stripped non-recursively](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)

Tehtävänannon mukaan tässä labissa verkkosovellus poistaa käyttäjän antamat siirtymisparametrit.

Aloitin taas sniffailemalla ZAP:illa tuotekuvaa ja avaamalla sen requesterissa.

Aloin kokeilemaan eri ratkaisuja syötteen manipuloimiseen. Ennen pari path traversal lippua saaneena uskoisin, että "filtterin" läpi voi yrittää kiertää. PortSwiggerin [path traversal](https://portswigger.net/web-security/file-path-traversal#what-is-path-traversal) osiossa puhutaan esimerkiksi "nested traversal" tekniikasta, jolla ohitetaan filttereitä käyttämällä vaikka: ```....//../```.  Toimii sillä periaatteella, että sovellus poistaa syötteestä ``../``, mutta ei koko pitkää syötettä (?).

Kokeilen siis seuraavanlaista syötettä requesterilla: ```....//....//....//etc/passwd```

<img width="1270" height="742" alt="image" src="https://github.com/user-attachments/assets/ece1725a-da6e-4f36-8e81-ccf4392acedf" />

<img width="975" height="206" alt="image" src="https://github.com/user-attachments/assets/191f9da7-179d-4672-aa88-8a4efe7e1e2d" />


- Toimii!


### PortSwigger - Insecure Direct Object Reference (IDOR)

i) [Insecure direct object references](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)

Labrassa kohde säilyttää käyttäjien chat-lokeja suoraan palvelimen tiedostojärjestelmässä ja noutaa ne käyttäen staattista IP-osoitetta.
Tehtävänä on saada käyttäjän "Carlos" salasana ja kirjautua hänen käyttäjälleen.

Avaan labin etusivun, josta siirryn Live chattiin:

<img width="1287" height="676" alt="image" src="https://github.com/user-attachments/assets/c7d46be0-3b73-45e0-8dc5-a92f0b83a418" />


Kirjoitan chattiin viestin ja saan vastauksen:

<img width="703" height="328" alt="image" src="https://github.com/user-attachments/assets/8ba9d472-7436-4eda-8a50-40e8cb044a51" />

Yritän selvittää ZAP:illa, mitä tässä tapahtuu.

<img width="1277" height="678" alt="image" src="https://github.com/user-attachments/assets/cd1cc809-3404-4cf2-9e36-b6cdb7e6d657" />

- Keskustelumme on plaintext muodossa mukavasti.
- Huomaan heti, että ```GET```-pyyntö hakee tiedostoa "2.txt".

Haluan kokeilla, mikä on tiedosto "1.txt", joten vaihdan syötteen siihen.

<img width="893" height="335" alt="image" src="https://github.com/user-attachments/assets/4243031d-c760-4f4a-9ad3-b0caf65ca2d1" />

- Well well well...

Kokeilen käyttäjätunnusta "Carlos" salasanalla, minkä juuri varastin:

<img width="710" height="389" alt="image" src="https://github.com/user-attachments/assets/4245b044-1450-47d4-9f16-c4be4309f51f" />

- Jee!

Hyökkäys siis toimii niinkin helposti, kuin muokkaamalla tiedostonimeä, jota olet vastaanottamassa.



## Lähteet

Bhatt, A. 2025. The Art of XSS Hacking: From Basics to Advanced Exploits. Medium. Luettavissa: https://medium.com/@adityabhatt3010/the-art-of-xss-hacking-from-basics-to-advanced-exploits-6a3276f81aaa

Jung, E. 2026. FoxyProxy Standard. Firefox Add-ons. Luettavissa: https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/

Karvinen, T. 2026. Tunkeutumistestaus. Kurssisivusto. Luettavissa: https://terokarvinen.com/tunkeutumistestaus/

OWASP. s.a. Attacks: XSS. OWASP Community. Luettavissa: https://community.owasp.org/attacks/xss/

OWASP. s.a. Web Security Testing Guide: Testing for Stored Cross Site Scripting. OWASP Foundation. Luettavissa: https://owasp.github.io/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting.html

OWASP Top 10 Team. 2021. Broken Access Control. OWASP Top 10. Luettavissa: https://top10.owasp.org/2021/A01_2021-Broken_Access_Control/

PortSwigger. 2026. Cross-Site Scripting (XSS). Web Security Academy. Luettavissa: https://portswigger.net/web-security/cross-site-scripting

PortSwigger. 2026. File Path Traversal. Web Security Academy. Luettavissa: https://portswigger.net/web-security/file-path-traversal


PortSwigger. 2026. Insecure Direct Object Reference (IDOR). Web Security Academy. Luettavissa: https://portswigger.net/web-security/access-control/idor


PortSwigger Ltd. 2026. Web Security Academy. PortSwigger. Luettavissa: https://portswigger.net/web-security

pwn.college. Learn to Hack. Luettavissa: https://pwn.college/

Veijalainen. 2026. h2_täysin_laillinen_sertifikaatti.md. GitHub. Luettavissa: https://github.com/veitim/tunkeutumistestaus/blob/main/h2_t%C3%A4ysin_laillinen_sertifikaatti.md

Zaproxy. s.a. Server Certificates. OWASP ZAP. Luettavissa: https://www.zaproxy.org/docs/desktop/addons/network/options/servercertificates/


## Labrat

PortSwigger. 2026. File Path Traversal - Absolute Path Bypass. Web Security Academy. Luettavissa: https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass

PortSwigger. 2026. File Path Traversal - Simple Case. Web Security Academy. Luettavissa: https://portswigger.net/web-security/file-path-traversal/lab-simple

PortSwigger. 2026. File Path Traversal - Sequences Stripped Non-Recursively. Web Security Academy. Luettavissa: https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively

PortSwigger. 2026. Insecure Direct Object References Lab. Web Security Academy. Luettavissa: https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references

PortSwigger. 2026. Reflected XSS into HTML context with nothing encoded. Web Security Academy. Luettavissa: https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded

PortSwigger. 2026. Stored XSS into HTML context with nothing encoded. Web Security Academy. Luettavissa: https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded




