# h1 Viisikko

## x) Lue ja tiivistä

### Run Salt Command Locally

### Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux

### Raportin kirjoittaminen
- Raportin tulee olla täsmällinen ja toistettava. Raportissa tulee atkiivisesti kertoa mitä teit ja mitä tapahtui. Raporttia on hyvä kirjoittaa jo työtä tehdessä.
- Raportissa olisi hyvä myös sisällyttää ympäristö, jossa työ tehtiin, koska monet asiat ja mahdolliset ongelmat toimivat sekä ilmenevät eri tavalla tietokoneissa ja verkoissa.
- Kirjoita menneessä aikamuodossa ja kerro tarkasti, mitä komentoja annoit tai mitä klikkasit. Raportoi kaikki tulokset ja mahdolliset viat ohjelmissa, työkaluissa ja laitteissa.
- Raportin tulee olla helppolukuinen. Käytä väliotsikoita, kirjoita selkeää kieltä ja viittaa mahdollisiin lähteisiin.
- Vältä sepittämistä, plagiointia ja kuvien luvatonta käyttöä.

### Salt Install Guide: Linux (DEB)

## a) Asenna Debian 12-Bookworm virtuaalikoneeseen
Debian 12 asennettu virtuaalikoneeseen. Lisäksi päivitetty kaikki ohjelmat, otettu käyttöön palomuuri ja asennettu Virtualbox Guest Additions. Ei ongelmia asennuksessa.

![K1](1.png)

## b) Asenna Salt
Salt asennusta varten oli tarve hakea uudet pakettivarastot asentamista varten. Tämän ensimmäinen vaihe oli luoda *keyrings* kansio ja hakea julkinen avain.

![K2](2.png)

Seuraavaksi piti hakea itse pakettivarasto ja sen configuraatiot.

![K3](3.png)

Paketit päivittelemällä saatiin uuden pakettivaraston metadatat ajatasalle.

![K4](4.png)

Tämän jälkeen päästiinkin asentamaan itse vaadittu Salt-minion.

![K5](5.png)

Testataan vielä, että Salt-minion asentui onnistuneesti tarkastamalla versiota.

![K6](6.png)

## c) Viisi tärkeintä

### pkg
Saltin pkg hallitsee asennettavia ohjelmistopaketteja, joten sillä voidaan asentaa, päivittää tai poistaa paketteja. Testasin sitä asentamalla tree paketin komennolla **pkg.installed**.

![K7](7.png)

Tree paketti asennettu onnistuneesti. Seuraavaksi testasin vielä toimintaa.

![K8](8.png)

Toimii onnistuneesti, seuraavaksi vielä testasin poistaa paketin komennolla **pkg.removed** ja sekin onnistui ongelmitta.

![K9](9.png)

### file
Saltin File hallitsee tiedostoja. Kokeilin tätä luomalla ja poistamalla tiedostoja. Aluksi loin tyhjän tiedoston nimeltä hellotero komennolla **file.managed**.

![K10](10.png)

Tämän jälkeen loin vielä moitero tiedoston, mihin lisättiin teksti foo ja kun sen toimintaa testattiin cat komennolla, nähdään sisältö onnistuneesti.

![K11](11.png)

Lopuksi poistin vielä hellotero tiedoston komennolla **file.absent**.

![K12](12.png)

### service
Saltin service komento hallitsee palveluprosesseja (Daemoneja). Testissäni oli apache2 ja aloitetaan käynnistämällä se komennolla **service.running**.

![K13](13.png)

Ei lähtenytkään käyntiin, koska apachea ei ole asennettuna. Joten seuraavaksi asensin sen. 

![K14](14.png)

Testatessa uudestaan alkuperäistä käynnistyskomentoa, lähti se asentamisen jälkeen toimimaan **service.running** komennolla.

![K15](15.png)

Toimintaa testattu vielä selaimessa localhost sivustolla.

![K16](16.png)

Lopulta sammutin prosessin **service.dead** komennolla ja testasin, että se todellakin sammui.

![K17](17.png)

### user
Saltin user komento hallitsee käyttäjiä, esimerkissä aloitetaan luomalla käyttäjä terote08 komennola **user.present**

![K18](18.png)

Kuten cat komennolla nähdään, kotihakemistoon on luotu uusi käyttäjä terote08.

![K19](19.png)

Lopuksi poistin käyttäjän terote08 komennolla **user.absent**.

![K20](20.png)

### cmd
cmd komento hallitsee komentojen suorittamista ja sitä voidaan käskeä suoriutumaan tietyissä olosuhteissa. Käytännössä **cmd.run** toimii esimerkiksi tapauksissa, missä halutaan ettei toimintoa suoriteta vahingossa useaan kertaan. Kokeilin tätä suorittamalla touch /tmp/foo komennon joka luo tyhjän tiedoston, jos sitä ei ole olemassa.

![K21](21.png)

Ja kun ajetaan sama komento uudestaan, tulee ilmoitus, että tiedosto on jo olemassa.

![K21_1](21_1.png)

## d) Idempotentti
Saltin idempotenssi ilmenee suorittaessa samoja komentoja useaan kertaan, millä nähdään ettei ne tee turhia muutoksia, jos annettu komento on jo halutussa tilassa. Havainnoin tätä suorittamalla aluksi **service.running** komennon joka käynnistää apache2 palvelun.

![K22](22.png)

Ja kun sama komento ajetaan heti perään uudestaan nähdään, että suoritusaika on lyhyt ja mitään muutoksia ei tehty sillä apache2 on jo käynnissä.

![K22](23.png)

Havainnoin vielä samaa suorittamalla **pkg.installed** komennon, millä asennettiin tree paketti.

![K23](23.png)

Ja kun suoritetaan jälleen sama komento uudestaan, nähdään saman kaavan toistuvan.

![K24](24.png)

## Lähteet
Karvinen T 2025. h1 Viisikko. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/ Luettu 28.03.2025

Karvinen T 2006. Raportin kirjoittaminen. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/ Luettu 18.01.2024

Kasper Nurminen 2024. Linux Palvelimet - H1 Oma Linux. GitHub. Luettavissa: https://github.com/nurminenkasper/Linux-palvelimet/blob/main/h1-oma-linux.md Luettu 28.3.2025


