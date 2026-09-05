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

Lähteenä käytin Nmapin omilla sivuilla olevaa kirjaa, joten uskoisin sen olevan luotettava. ([Lyon, G. 2009](https://nmap.org/book/man-host-discovery.html)).

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

- Hakusanalla löytyi EternalBlue ja EternalRomance. Haussa esille tuli myös muita, kuten: EternalChampion ja EternalSynergy

Molemmista on kirjoitettu ainakin [Iltasanomissa](https://www.is.fi/digitoday/tietoturva/art-2000005426332.html), sekä [BBC:llä](https://www.bbc.com/news/technology-39905509).

Exploitista saa enemmän tietoa, kun avaa hyökkäyksen komennolla: ```use exploit/windows/smb/ms17_010_eternalblue``` ja sitten: ```info```

<img width="541" height="306" alt="image" src="https://github.com/user-attachments/assets/b0522542-57b0-423a-8ca3-e1defd2226a2" />

<img width="726" height="273" alt="image" src="https://github.com/user-attachments/assets/5b704c2c-55e8-45b1-99f5-1a06dbfd9f19" />

## e) Vertaile nmap:n omaa tiedostoon tallennusta (-oA foo) ja db_nmap:n tallennusta tietokantoihin. Mitkä ovat eri tiedostomuotojen ja Metasploitin tietokannan hyvät puolet?
