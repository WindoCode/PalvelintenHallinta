# h5 - Demonit

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
