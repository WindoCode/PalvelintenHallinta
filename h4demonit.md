# h4 - Demonit

## Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves
Infra as code:
- Salt kansioon luodaan uusi kansio: Hello
- Hello-kansion sisään luodaan uusi tiedosto, init.sls, Saltin tila (state)- tiedosto, joka toimii ohjeena minion-tiedostoille.
- Init.sls tiedoston sisällä pyydetään luomaan tiedosto, jonka nimi on infra-as-code. Tämä toteutuu file.managed komennolla.
- `sudo salt '*' state.apply hello`: Pyytää minioneita ajamaan hello-tilan.

top.sls: Voiko top.sls-tiedostossa olla enemmän kuin yksi tila kirjoitettuna?
- Sen sijaan, että joudumme kirjoittamaan erillisiä tila-tiedostoja, voimme kirjoittaa ne top.sls - tiedostoon.
- Tässä tapauksessa top.sls-tiedostoon on kirjoitettu, että minioneiden tulisi ajaa hello-tila.
```
$ sudo salt '*' state.apply hello^C
$ sudoedit /srv/salt/top.sls
$ cat /srv/salt/top.sls
base:
  '*':
    - hello
```
## Salt contributors: Rules of YAML

- Data on järjestetty avain: arvo pareiksi.
Esimerkki: `avain: arvo`

- Määritykset käyttävät kaksoispistettä ja yhtä välilyöntiä (“: ”) merkitsemään avain: arvo pareja: `nimi: Matti`

- Kaikki avaimet/ominaisuudet ovat kirjainkoosta riippuvaisia.`Nimi` ja `nimi` ovat kaksi eri avainta.

- Sarkaimia EI sallita, käytä VAIN välilyöntejä.

- Kommentit alkavat risuaidalla “#”.
`# Tämä on kommentti`

## Salt contributors: Salt states
- Komponentti, joka vastaa salt-tilojen täytäntöönpanosta ja hallinnasta.
- Salt statet sisältää logiikan, jolla tarkistetaan onko jo oikessa konfiguraatiossa.

Tilatiedosto sisältää 5 eri komponenttia:
- Tunniste(Identifier): tilalle annettu nimi
- Tila (State): Tilamoduulin nimi, joka sisältää esimerkiksi funktion: `pkg.installed`. Tässä tapauksessa viitataan paketinhallintaan.
- Funktio (Function): `pkg.installed`-funktion `installed`-osa, jolla tarkistetaan, onko paketti oikeasti asennettu.
- Nimi(Name): tiedoston nimi, jota asennetaan.
- Agrumentit(Arguments): Argumentit, jotka hyväksytään tilassa, esim: `arg_value`
```
/srv/salt/example.sls

identifier:
  module.function:
    - name: name_value
    - function_arg: arg_value
    - function_arg: arg_value
    - function_arg: arg_value
```


## Hello SLS!
Testiympäristö:
```
Käyttöjärjestelmä: Ubuntu 22.04 LTS
CPU: Intel® Core™ i5-1135G7 2.4 GHz
Muisti: 8 GB
```

Käytin tähän tehtävään h2-tehtävänannon aikana rakennettua salt-ympäristöä. Vagrantin virtuaalikoneiden konfiguraatiotiedosto on sama, mitä h2-tehtävänannossa, joten pystyin käynnistämään sen suoraan `vagrant up` - komennolla. Tämän jälkeen kirjauduin SSH:n avulla master-koneelle, komennolla: `vagrant ssh tmaster`.

Siirryin editoimaan init-tiedostoa, jolla luomme heimaailma-funktion. `$ sudo nano /srv/salt/hello/init.sls`. Kirjoitamme tilan init.sls tiedoston sisään.

```
helloworld:
  cmd.run:
    - name: echo "Hei maailma!"
```

Tallennan tiedoston CTRL+X + Y. Tämän jälkeen voimme ajaa tilan koneille. `$ sudo salt '*' state.apply hello`. 

### Lopputulos, molemmat minionit printtasivat "Hei maailma!".:
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/225eb751-ea32-43f7-afa6-444383738438)

## Top.

Koska jotkut ympäristöt saattaa sisältää satoja tilatiedostoja, jotka kohdistuvat tuhansiin koneisiin, ei ole käytännöllistä suorittaa jokaista tilaa erikseen. Siksi luomme `top.sls`, tiedoston, jolla voimme ajaa useita tiloja samanaikaisesti. Tässä tapauksessa ajamme vain `hello` tilan. Täten meidän ei tarvitse ilmoittaa enään tilaa, mitä haluamme ajaa. Tarvitsemme vain komennon: `sudo salt '*' state.apply`.

Kirjoitetaan top.sls tiedosto.

$ sudo nano /srv/salt/top.sls
```
base:
  '*':
    - hello
```

Tämän jälkeen annamme komennon: `sudo salt '*' state.apply`

### Lopputulos: Vagrant käyttää top.sls tiedostoa ajaakseen siinä listatut tilat: `hello`.
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/5b5eef8f-7a1c-4faf-804c-400b81502d43)


## Apache.

Aloitin asennusprosessin käsin, ensiksi kirjauduin ulos master-koneelta, jonka jälkeen kirjauduin t001-minionille. `vagrant ssh t001`. Ensimmäiseksi tarkistin oliko koneella apache2-asennusta komennolla `sudo systemctl status apache2`. Asennusta ei löytynyt, joten asensin sen komennolla `sudo apt-get install apache2`. Tämän jälkeen siirryin apachen kansioon komennolla `$ cd /var/www/html`. `ls`-komennolla näytämme kansion tiedostot, josta löydämme `index.html` tiedoston. Käytämme `rm index.html`-komennon,jolla poistamme tiedoston. Teemme uuden tiedoston kansioon komennolla. `sudo nano newindex.html`.

Lopuksi demonin tarkistus komennolla `sudo systemctl status apache2 | grep "Active"`. Voimme tällä komennolla todeta apachen olevan käynnissä. Poistamme samalla apache2-asennuksen ja siirrymme takaisin masterille tekemään uutta automatisoitua `init.sls`-tiedostoa.

- `vagrant ssh tmaster`
- `sudo nano /srv/salt/hello/init.sls`
- Kirjoitamme init.sls tiedostoon 4 funktiota: apachen asennus, html-tiedoston poistaminen, uuden html-tiedoston luominen sekä daemonin tarkastuksen.

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/eb86687b-a4d7-463c-bb57-51c3fe365ce1)

Koska meillä on jo top.sls tiedosto, jonka loimme aikaisemmin, voimme ajaa juuri tehdyn `init.sls`-tiedoston `sudo salt '*' state.apply`-komennolla. Testataan:

### Lopputulos: Olemme saaneet toimimaan tehtävänannon 4 erilaista funktiota!
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/f7f59d66-8892-4cf9-bc8a-f99760934825)

## SSHouto.

- Kirjaututaan pois vagrant ssh-yhteydestä: `exit`.
- `sudo apt update`
- Asennetaan ssh-serveri `sudo apt-get install openssh-server`
- Vaihdetaan portti ssh-konfiguraatiotiedostossa: `sudo nano /etc/ssh/sshd_config`
- Poistetaan #, eli kommentti esimerkistä, ja vaihdetaan portti 22 porttiin 1234.
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/40f35814-e31a-4c20-96b8-9be435939e45)
- Kun konfiguraatiotiedostoa muokataan, tulee aina Teron sanoin "potkaista demonia": Eli käynnistämme demonin uudestaan: `sudo systemctl restart sshd.service`
- Kokeillaan lopputulosta! ??? Profit

### Lopputulos: Yhteys: onnistunut!
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/4ee3d8dc-2c7f-483e-80e1-4cdffa314cad)

## Lähteet
- Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves [linkki](https://terokarvinen.com/2023/salt-vagrant/)
- SaltStack, Rules of YAML [linkki](https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml)
- SaltStack, Salt states [linkki](https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules)
- Karvinen 2023: Tehtävänanto: [linkki](https://terokarvinen.com/2023/configuration-management-2023-autumn/)



  
