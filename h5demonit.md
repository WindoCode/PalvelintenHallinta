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

- Määritykset käyttävät kaksoispistettä ja yhtä välilyöntiä (“: ”) merkitsemään avain: arvo pareja.
Esimerkki: `nimi: Matti`

- Kaikki avaimet/ominaisuudet ovat kirjainkoosta riippuvaisia.
Esimerkki: Nimi ja nimi ovat kaksi eri avainta.

Sarkaimia EI sallita, käytä VAIN välilyöntejä.
Esimerkki: `avain:\t` arvo on virheellinen, käytä `avain: arvo`

Kommentit alkavat risuaidalla “#”.
`# Tämä on kommentti`


## Salt contributors: Salt states, kohdat
