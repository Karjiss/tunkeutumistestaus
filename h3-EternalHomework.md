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

