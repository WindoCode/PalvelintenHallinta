# h5 CSIKerava


## x) Lue ja tiivistä: Apache User Homepages Automatically – Salt Package-File-Service Example

### Artikkelin pihvi on find-komennon käyttö. find-komennolla etsitään esimerkiksi tiedostoja tai kansioita.
      - `find -printf "%T+ %p\n"|sort`
      - `printf`- printtausfunktio
      - `%T+` - muokkausaika
      - `%p` - path = tiedostonimi ja polku
      - `/n`- uusi rivi
      - `sort` - aakkosjärjestys, tai järjestys pienimmästä suurimpaan numeroilla.
  - Tee aina käsin, ennen kuin alat automatisoimaan. 
  - Tee salt-tiloista idempotentti eli komennoilla aina sama vaikutus, vaikka kuinka usein sen ajaa.

## a) CSI Kerava - Find-komennolla kotihakemistosta sekä etc-kansiosta viisi viimeisintä muokattua tiedostoa.
- En ehtinyt olemaan tunnilla loppuun saakka,joten aloitin suoraan teron vinkistä: `find -printf '%T+ %p\n'`. Komento ymmärtääkseni etsii tiedostoja koko tietokoneelta ja tulostaa niiden viimeisen muokkausajan ja polun. Tämä komento ei kuitenkaan järjestele muokkauksia mitenkään. Siksi lisään komentoon uuden komennon: `sort -r` ja `head -n 5`. Määritän vielä hakukohteeksi etc-kansion. `find /etc/`.


- `find /etc/ -printf '%T+ %p\n' | sort -r | head -n 5`
- `printf '%T+ %p\n'`: Määrittää tulosteen muodon. `%T+` tulostaa muokkausajan tarkemmin ja `%p` tulostaa tiedoston polun.
- `sort -r`: Järjestää tulokset käänteisessä aikajärjestyksessä (uusimmasta vanhimpaan).
- `head -n 5` : Näyttää vain viisi ensimmäistä riviä, eli viisi viimeisintä muokattua tiedostoa.

- ETC-kansion tulokset: Huomaan, että minulla ei ole oikeuksia joihinkin kansioihin, joten käytän sudo:a, jolloin saamme jokaisen mahdollisen muutoksen näkyviin. Muutoksia ei tullut. Näemme kuitenkin muutoksia, joita olen tehnytkin aikaisemmin liittyen viime kotitehtävään: salt-minion+ssh.

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

## c) Komennus. Tee Salt-tila, joka asentaa järjestelmään uuden komennon.

- Ensin käsin. Teen yksinkertaisen Hello World-komennon. Tämän jälkeen voimme automatiEnsin komennon asentaminen käsin yhdelle koneelle (masterilla):

      $ nano hello_world                    # komennon nimi:hello_world
      $ chmod +x hello_world                # ajo-oikeuden lisääminen kaikille
      $ ./hello_world                       # testataan komentoa: komento printtaa "Hello World!"
      $ sudo mv hello_world /usr/local/bin/ # siirretään tiedosto, jotta komennon voi ajaa mistä hakemistosta tahansa
      $ hello_world                         # Komento printtaa: Hello World!

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/56ab19f2-6306-4b0e-9f18-20ad628e590a)
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/09b86cb5-66ef-4698-a840-3e85e016dace)

- Automatisoidaan prosessi. Aloitamme tekemällä kansion salt-kansioon nimeltä komennot: `sudo mkdir /srv/salt/commands` Commands-kansioon voimme linkittää hello_world-komennon `/usr/local/bin/hello_world` kansiosta. `sudo ln -s /usr/local/bin/hello_world` Tämän jälkeen teemme init.sls tiedoston, jossa pyydämme luomaan koneelle bin-kansioon uuden komennon, joka lisää hello_world komennon master-koneelta. Lisäämme myös tiedostoikeudet: 0755.

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/134480c1-57b4-4261-9a28-32f33b2c4387)



- Tämän jälkeen voimme minioneita ajamaan tilan: `sudo salt '*' state.apply commands`
- Lopputulos: Kokeillaan minionilla komentoa. `vagrant ssh t001` ja `hello_world`. 

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/78f7cc11-fb84-4c25-8974-28a8d3845207)


## d) d) Apassi. Tee Salt-tila, joka asentaa Apachen näyttämään kotihakemistoja.

- Missä apachen konfiguraatiotiedosto sijaitsee? Käytän tämän tehtävän apuna Teron ohjeita. Ohjeen löydät tämän linkin takaa. [Teron ohje](https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/). Löytääksemme nämä tiedostot meidän ensiksi ottaa käyttöön a2enmod userdir-käyttäjähakemisto. Ensiksi meidän täytyy ottaa käyttöön apache2-palvelin. `sudo apt install apache2`. Tämän jälkeen otamme käyttöön käyttäjähakemiston. `sudo a2enmod userdir`. Tämän jälkeen potkaisemme daemonia: `sudo systemctl restart apache2.` Tämän jälkeen voimme tarkistella apache2-kansion muutoksia komennolla `sudo find -printf '%T+ M %p\n%A+ A %p\n%C+ C %p\n'|sort`. 

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/703077c6-353d-431c-a153-413a8f89f407)

- Voimme todeta printistä, että konffaustiedostot löytyy /mods-enabled-kansiosta.

- Seuraavaksi automatisointi. Voimme käyttää pohjana Teron ohjeesta "Better way with files" init.sls-pohjaa. Meidän kuitenkin tulee lisätä uusi tiedosto, index.html home/public_html-kansioon. Lisäämme siis pohjalle:

```
/home/vagrant/public_html:
      file.managed:
        - makedirs: true # luo roottina kansiot
        - owner: vagrant # omistaja: vagrant, jokaisella minionilla sama nimi käytössä.
        - group: vagrant
        - mode: "0755"  # Owner kaikki oikeudet, muilla luku ja execute-oikeus.
```

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/e65d7b8d-8214-4e61-b488-9e6d04955992)

- Lopulta voimme ajaa init.sls-tiedoston `sudo salt '*' state.apply apache`.

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/832d8eb9-2579-4d10-9404-423a0c16236f)

- Tarkistetaan vielä, että index.html tiedosto on valmiina minionilla, t001-koneella. Tarkistamme, onko tiedosto oikeassa kansiossa oikeilla oikeuksilla komennolla: `ls -l public_html/`. Kirjaudumme ulos masterilta ja kirjaudumme minionille `vagrant ssh t001`.

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/bf18dac8-f505-4f98-988b-40bbc8ea90cc)

- Voimme todeta, että kotisivu on toiminnassa ja käyttäjä voi tehdä sille muutoksia.


## e) Ämpärillinen. Tee Salt-tila, joka asentaa järjestelmään kansiollisen komentoja.

- Ensiksi loin käsin hakemiston bin-kansioon, johon tein 3 Hello world-komentoa. Nimesin kansion commands-kansioksi. Komennot muikailee C-tehtävän komentoa, pienin muutoksin joka komennossa. Komennot sijaitsevat /usr/local/bin - kansiossa ja annoin kaikille komennoille ajo-oikeuden komennolla chmod +x hello1,hello2,hello3. Komennot toimivat käsin tehtynä, nyt siirrytään asian pihviin, eli automatisointiin.

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/a6e426f5-b489-43c5-b172-3b13ae410533)

- Mitä init.sls-tiedostoon tarvitsemme kirjoittaa?
- Kansion kopioiminen commands-kansiosta
- tiedostooikeuksien pysyminen samana
- kopioiminen commands-kansiosta minionin bin-kansioon.

- Yritin käyttää linkkiä salt-tilan commands-kansiosta omaan bin-kansioon init.sls tiedostossa, mutta sain virhekoodiksi salt-tilan käyttöönotossa, että sls-tiedostossa tarvitaan absoluuttinen polku tiedostoihin. Täten vaihdoin sourceksi: salt://commands/commands, jossa komennot sijaitsivat. Alla lopullinen koodi, joka toimi. 

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/2f16eb37-2ad6-43fb-b7c6-6060b5dbb920)

- Käytin tehtävässä file.recurse-funktiota, jolla voimme siirtää hakemiston samoilla oikeuksilla minion-koneille
- Komentojen sijaintina on: salt://commands/commands
- file_mode: keep tarkoittaa, että tiedoston oikeudet pysyvät samana, mikä on master koneella. Tässä tapauksessa '0755'. Täten meidän ei tarvitse muuttaa erikseen omia oikeuksia. Tämän kanssa kuitenkin tulee olla varovainen. Jos tälle tiedostolle antaa liikaa oikeuksia, tietokoneet voivat olla vaarassa, esim. sudo-hyökkäys, jossa korotetaan käyttäjän taso sudoksi.
- cp -a kopioi tiedostot minion-koneen bin kansioon /tmp/commands kansiosta.

# Lähteet

- Tero Karvinen, Apache User Homepages Automatically – Salt Package-File-Service Example, 2018. [Linkki](https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/)
- Tero Karvinen, Tehtävänanto, 2023. [Linkki](https://terokarvinen.com/2023/configuration-management-2023-autumn/)
- SaltStack, dokumentaatio, file.recurse-funktio [Linkki](https://docs.saltproject.io/en/latest/ref/states/all/salt.states.file.html)




