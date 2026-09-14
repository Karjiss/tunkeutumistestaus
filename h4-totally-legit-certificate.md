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

ZAP:issä valitsin ylävalikosta ```Tools --> Options```

<img width="793" height="594" alt="image" src="https://github.com/user-attachments/assets/2bfa6d34-4da4-4693-a5cf-efeb21a45568" />

Kirjoitin asetusten hakuun "CA Certificate":

<img width="732" height="334" alt="image" src="https://github.com/user-attachments/assets/12ae79f5-bb14-474c-97e8-56e20e797d51" />

- Polku = ```Network --> Server Certificates```
- Kohde löytynyt!

Tallensin sertifikaatin kotihakemistooni painamalla: ```Save```-nappia.

<img width="694" height="589" alt="image" src="https://github.com/user-attachments/assets/2bce4e7f-8b63-476c-a724-3a25b7972e77" />


CA-sertifikaatti piti asentaa vielä selaimelle, joten hommiin!

Hakkasin päätä näppäimistöön hetken aikaa, kunnes löysin tehtävien vinkkiosiosta opastuksia.

Avasin Firefoxin "Settings" valikon, josta hain "Certificates".

<img width="1009" height="727" alt="image" src="https://github.com/user-attachments/assets/92986953-4869-4515-85c1-f4898d3c77e4" />

- Huokaisin helpotuksesta.

Seuraavaksi klikkasin ```View Certificates``` --> ```Import```

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





## Lähteet

OWASP 2021: OWASP Top 10:2021. A01 Broken Access Control. Luettavissa: https://top10.owasp.org/2021/A01_2021-Broken_Access_Control/

Karvinen, Tero. 2026. Tunkeutumistestaus. Luettavissa:

