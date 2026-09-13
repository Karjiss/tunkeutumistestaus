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

  




## Lähteet

OWASP 2021: OWASP Top 10:2021. A01 Broken Access Control. Luettavissa: https://top10.owasp.org/2021/A01_2021-Broken_Access_Control/

Karvinen, Tero. 2026. Tunkeutumistestaus. Luettavissa:

