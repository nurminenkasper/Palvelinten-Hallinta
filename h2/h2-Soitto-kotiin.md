# h2 Soitto kotiin

## Rauta & HostOS

- Asus X570 ROG Crosshair VIII Dark Hero AM4
- AMD Ryzen 5800X3D
- G.Skill DDR4 2x16gb 3200MHz CL16
- 2x SK hynix Platinum P41 2TB PCIe NVMe Gen4
- Sapphire Radeon RX 7900 XT NITRO+ Vapor-X
- Windows 11 Home 24H2

**Tehtävän aloitusaika XX.4.2025 kello XXXX.**

## x) Lue ja tiivistä

### Karvinen 2021: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant

### Karvinen 2018: Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux

### Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves

#### Infra as Code - Your wishes as a text file

#### top.sls - What Slave Runs What States

## a) Hello Vagrant!
Windowsilla suoritettiin tehtäviä niin hain asennusmedian Vagrantin sivustolta ja kliksuttelin netxiä, kunnes saatiin ohjelma asennettua.

![K1](1.png)

cmd auki ja testiin, mahtoiko asennus onnistua ja onnistuihan se.

![K2](2.png)

## b) Linux Vagrant
Windows cmd käyntiin ja uutta Vagrant virtuaalikonetta varten loin uuden kansion ja ajoin sinne sisään **vagrant init debian/bookworm64** komennolla uuden Debian-virtuaalikoneen.

![K4](4.png)

Tämän jälkeen pyöräytettiin luotu virtuaalikone käyntiin **vagrant up** komennolla ja hyvinnhän se lähti rullaamaan. VirtualBoxin puolelta nähdään myös, miten uusi kone löytyy luotuna.

![K5](5.png)
![K6](6.png)

**vagrant ssh** komennolla virtuaalikone käyttöön ja testiksi vielä pingaus googlen osoitteeseen, niin nähdään sen toimivan moitteitta.

![K7](7.png)

Perään vielä **exit** millä poistuttiin virtuaalikoneesta ja kansiorakenteessa **vagrant destroy** sillä mitään tarvetta säilyttää kyseistä virtuaalikonetta ei ollut.

![K8](8.png)

## c) Kaksin kaunihimpi
Kahta Linux-virtuaalikonetta uusi kansiorakenne ja sinne Debian rakentaminen komennolla **vagrant init debian/bookworm64**

![K9](9.png)

[Teron sivusolta](https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/) löytyvä Vagrantfile tiedoston konfiguraatio sisään, poikkeuksena toki se, että vm.box oli vaihdettava tämän hetkiseen debian/bookworm64 versioon.

![K10](10.png)

Virtuaalikoneet päälle **vagrant up** komennolla.

![K11](11.png)
![K12](12.png)

Testailin vielä, että koneet saavat varmasti yhteyden toisiinsa. Availin ensin **vagrant ssh t001** komennolla t001 koneen ja tarkastelin vielä ip osoitteen varmuudeksi, vaikka se käytännössä oli jo tiedossa konfiguraatiotideoista. Siirryin seuraavaksi t002 puolelle **vagrant ssh t002** komennolla ja pingasin sieltä t001 koneen ip-osoitetta onnistuneesti. Testailin vielä, että toimiiko yhteys muuallekkin verkkoon pingaamalla googlen osoitetta.

![K13](13.png)

Hommat paketissa, joten tuhotaan jälleen ylimääräiset virtuaalikoneet komennolla **vagrant destroy**

## d) Herra-orja verkossa
Seuraavaa tehtävää varten kansiorakenne pysyi samana, mutta **vagrantfile** oli hieman muokattava sopivammaksi. Kävin muokkaamassa nimiksi master ja slave, vaikka ei tämäkään toki niinkään tarpeellinen vaihe olisi, mutta tehtävän kannalta ihan mielenkiintoinen vaihdos.

![K15](15.png)

Virtuaalikoneet käyntiin **vagrant up** komennolla

![K16](16.png)
![K17](17.png)

**vagrant ssh master** komennolla sisään ja ensimmäisenä hommana oli asennella Salt, mutta sitä varten oli tarpeellista rakentaa luottamussuhde Salt Project repon kanssa.

![K18](18.png)
![K19](19.png)

Luottamussuhteen rakentamisen jälkeen master koneelle asentelin **salt-master** version ohjelmistosta.

![K20](20.png)

**vagrant ssh slave** komennolla sisään slave koneen puolelle ja sama luottamussuhteen rakentaminen käyntiin.

![K21](21.png)
![K22](22.png)

Lopulta asentelin **salt-minion** version slave-virtuaalikoneelle.

![K23](23.png)

Slave koneen puolelle **sudoedit /etc/salt/minion** ja sinne lisäilemään master koneen IP-osoite sekä Slave ID tunnistamista varten. Loppuun vielä **sudo systemctl restart salt-minion.service** niin saatiin uudet asetukset käyttöön.

![K24](24.png)
![K25](25.png)

Takaisin master koneen puolelle tarkoituksena hyväksyä orjan avainpyyntö **sudo salt-key -A** komennolla

![K26](26.png)

Mitä ihmettä, ei toiminut? Suuntasin takaisin Slave virtuaalikoneen /etc/salt/minion tiedostoon muokkaamaan ja otin alkuun id kohdan pois, jos se olisi ongelma? Demonin uudelleenkäynnistys perään.

![K27](27.png)

Uusi yritys Master puolella tuotti kuitenkin edelleen samaa virhettä.

![K28](28.png)

Takaisin tiedoston pariin kokeilemaan eri IP-osoitetta ja lisäisin id kohdan takaisin samalla, koska se ei selvästi ollut ongelmana. Demonin uudelleenkäynnistys perään. 

![K29](29.png)

Bah, samaa virhettä tuottaa edelleen. Testailin vielä ping komennolla, toimiiko varmasti yhteys koneiden välillä.

![K30](30.png)

Se kun toimi niin rupesin hieman googlaamaan ja törmäsin Reddit postaukseen, missä salt-master ei ollut päällä joten tarkistelin sitäkin omalta osalta. **sudo systemctl status salt-master** antoikin palautetta heti ettei ole päällä joten käynnistus perään **sudo systemctl start salt-master** komennolla. 

![K31](31.png)

Joko nyt lähtisi avaimen hyväksyntä toimimaan?

[K32](32.png)

Vihdoin! Testailin vielä muutamalla komennolla, että toimiihan orjan komentaminen.

![K33](33.png)

## e) Kokeile vähintään kahta tilaa verkon yli


**Tehtävän lopetusaika XX.4.2025 kello XXXX. Aktiivista työskentelyä yhteensä noin X tuntia XX minuuttia.**

## Lähteet
Karvinen T 2025. h1 Soitto kotiin. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/ Luettu 3.4.2025
