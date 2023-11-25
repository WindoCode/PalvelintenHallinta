# h5 CSIKerava

## a) CSI Kerava - Find-komennolla kotihakemistosta sekä etc-kansiosta viisi viimeisintä muokattua tiedostoa.
En ehtinyt olemaan tunnilla loppuun saakka,joten aloitin suoraan teron vinkistä: `find -printf '%T+ %p\n'`. Komento ymmärtääkseni etsii tiedostoja koko tietokoneelta ja tulostaa niiden viimeisen muokkausajan ja polun. Tämä komento ei kuitenkaan järjestele muokkauksia mitenkään. Siksi lisään komentoon uuden komennon: `sort -r` ja `head -n 5`. Määritän vielä hakukohteeksi etc-kansion. `find /etc/`.


- `find /etc/ -printf '%T+ %p\n' | sort -r | head -n 5`
- `printf '%T+ %p\n'`: Määrittää tulosteen muodon. `%T+` tulostaa muokkausajan tarkemmin ja `%p` tulostaa tiedoston polun.
- `sort -r`: Järjestää tulokset käänteisessä aikajärjestyksessä (uusimmasta vanhimpaan).
- `head -n 5` : Näyttää vain viisi ensimmäistä riviä, eli viisi viimeisintä muokattua tiedostoa.

ETC-kansion tulokset: Huomaan, että minulla ei ole oikeuksia joihinkin kansioihin, joten käytän sudo:a, jolloin saamme jokaisen mahdollisen muutoksen näkyviin. Muutoksia ei tullut. Näemme kuitenkin muutoksia, joita olen tehnytkin aikaisemmin liittyen viime kotitehtävään: salt-minion+ssh.

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/ac8199f4-9b42-4d4f-95fc-115c194784fd)

- Kotikansion tulokset: muokataan komento: `find ~ -printf '%T+ %p\n' | sort -r | head -n 5`. Muutokset näyttävät olevan selaimen tiedostomuutoksia, joita käytän parhaillaan. Tämä on ymmärrettävä lopputulos.

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/b9270957-0081-4df6-b6b0-5feb65ddc72f)

## b)Muokkaa asetuksia jostain graafisen käyttöliittymän (GUI) ohjelmasta käyttäen ohjelman valikoita ja dialogeja. Etsi tämä asetus tiedostojärjestelmästä.

- Muokkaan esimerkiksi Visual Studio Code:n fonttiasetuksia GUI:n kautta ja etsin konffaustiedoston tietokoneelta. 

- Löydämme Code:n asetukset ensiksi avaamalla code:n, jonka jälkeen: `Settings, Commonly used, Editor:font size.`.

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/e0c289de-33d7-4ed0-85c5-03d6844bdb37)

- Siirrytään konfiguraatiotiedoston etsintään. Etsin tiedostosijainnin suoraan googlesta, Stack Overflown sivuilta:
[Linkki](https://stackoverflow.com/questions/58900482/what-are-all-configuration-files-used-by-visual-studio-code-and-where-does-it-s)

- Visual studio coden konfiguraatiotiedosto löytyy: `$HOME/.config/Code/User/settings.json`

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/871640c1-9f5f-4141-8bba-85c43e9c25b3)

- Seuraavaksi voimme editoida tiedostoa, `sudo nano setting.json`. Muutamme fonttikooksi 11, aikaisemmin oli 15.

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/1d3b0549-a405-4632-aa1b-8d819f3df8cc)

- Voimme nähdä myös tämän muutoksen kansiossa tiedostomuutoksina. Voimme käyttää edellisen kohdan komentoa löytääksemme muutokset. `find -printf '%T+ %p \n' | sort`
