# h5 CSIKerava
## a) CSI Kerava - Find-komennolla kotihakemistosta sekä etc-kansiosta viisi viimeisintä muokattua tiedostoa.
En ehtinyt olemaan tunnilla loppuun saakka,joten aloitin suoraan teron vinkistä: `find -printf '%T+ %p\n'`. Komento antoi ymmärtääkseni etsii tiedostoja /etc/-hakemistosta ja tulostaa niiden viimeisen muokkausajan ja polun. Tämä komento ei kuitenkaan järjestele muokkauksia mitenkään. Siksi lisään komentoon uuden komennon: `sort -r` ja `head -n 5`.

- `printf '%T+ %p\n'`: Määrittää tulosteen muodon. `%T+` tulostaa muokkausajan tarkemmin ja `%p` tulostaa tiedoston polun.
- `sort -r`: Järjestää tulokset käänteisessä aikajärjestyksessä (uusimmasta vanhimpaan).
- `head -n 5` : Näyttää vain viisi ensimmäistä riviä, eli viisi viimeisintä muokattua tiedostoa.

ETC-kansion tulokset: Huomaan, että minulla ei ole oikeuksia joihinkin kansioihin, joten käytän sudo:a, jolloin saamme jokaisen mahdollisen muutoksen näkyviin. Muutoksia ei tullut.

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/ac8199f4-9b42-4d4f-95fc-115c194784fd)

