# h3 - Versio

## Testiympäristö
- Käyttöjärjestelmä: Ubuntu 22.04 LTS
- CPU: Intel® Core™ i5-1135G7 2.4 GHz
- Muisti: 8 GB

## Online. Tee uusi varasto GitHubiin.
- Painetaan plus-ikonia Githubissa ja create new repository.
<p align="center">
<img src="https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/3baa390f-5ed7-4a97-94c3-b82f50baf6f1" width=60% height=60%>
</p>

- Asetetaan nimeksi "Testi, lyhyeen kuvaukseen tehtävänannon mukaan "winter". Lisätään README-tiedosto sekä GNU v3.0-lisenssi.
<p align="center">
<img src="https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/f976bdee-df33-4714-94f6-59f117a6c43e" width=50% height=50%>
</p>

### Lopputulos: Saimme tehtyä tehtävänannon mukaisen varaston!
<p align="center">
<img src="https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/4d3c37ba-05dc-446b-8873-4f84e640f614" width=50% height=50%>
</p>

## b) Dolly. Kloonaa edellisessä kohdassa tehty uusi varasto itsellesi, tee muutoksia, puske ne palvelimelle, ja näytä, että ne ilmestyvät weppiliittymään.
- Luon ensiksi ssh-avainparin omalla tietokoneella, jonka jälkeen yhdistän sen GitHubiin, tämän jälkeen kloonaamme, muokkaamme ja puskemme muutokset GitHubiin.

### SSH-avainparin luominen

-Ensiksi tarvitsemme päivitykset koneelle sekä SSH-palvelun, jos sitä ei ole.:

```
$ sudo apt update
$ sudo apt-get install openssh-server
```

- Tämän jälkeen luomme uuden SSH-avainparin:
```
$ ssh-keygen
```
- Testikäytössä en lisää avaimelle passphrase:a. Jos käytät ssh-avainta tuotannossa, tämän lisääminen on välttämätöntä.

<p align="center">
<img src="https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/2443f67c-92a7-484d-9bc7-41cf7d98a811" width=50% height=50%>
</p>

- Logissa ilmoitetaan avaimen lokaatioksi `/home/valtteri/.ssh/id_rsa.pub`. Kopioidaan kyseinen avain ja lisätään se GitHubiin.
```
$ cd /home/valtteri/.ssh/
$ nano id_rsa.pub
$ cat ~/.ssh/id_rsa.pub
```
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/692d5719-3da9-46b7-b774-b9978f90bf59)

- Kopioidaan id_rsa.pub-julkinen avain ja liitetään se GitHubiin.
- Avataan GitHub -> Settings -> SSH and GPG-keys -> Add a SSH-key
- Liitetään tiedot ja annetaan githubissa kuvaava nimi avaimelle. 2FA pyytää vielä varmistamaan puhelimella muutoksen.

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/6a8c4e50-ba45-402f-942a-63fac451511f)

- Seuraavaksi kloonataan uusi varasto. `$ git clone git@github.com:WindoCode/Testi.git`.
- Siirrytään koneella repoon: `$ cd .ssh/Testi`.
- Lisätään tekstitiedosto: tärkeä.md, sisällytetään tekstiä testin vuoksi.
- Git pyytää meidän tietoja, annetaan ne muutoksen yhteydessä: 
```
$ git config --global user.email "valtteribaus@gmail.com
$ git config --global user.name "Valtteri Heinonen"
$ git commit
```
- Commit-komento avasi kommenttitiedoston, johon lisäämme kommentin: "Add important information related this project.".

```
$ git push
```

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/897f818e-3f45-43c6-b8da-4f04eb2ff30f)

#### Lopputulos: Tiedosto on syntynyt uuteen varastoon GitHubissa!

![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/332d8b2b-17d1-494d-9592-6ec0be312408)

## c) Doh! Tee tyhmä muutos gittiin. Tuhoa huonot muutokset.
- Lisäämme git-varastoon "henkilötietoja", jotka eivät saa olla osana repoa.
```
$ nano user_data
$ git add .
$ ls (Tiedosto on luotu repositioon "user_data")
$ git reset --hard
$ ls
```
- Lopputulos: Saimme poistettua henkilötiedot. (huh)
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/4ac3a341-8ab2-499d-a4ea-d0409e6e02f2)
## d) Tukki. Tarkastele ja selitä varastosi lokia. 
- Pääsemme logiin käsiksi komennolla: `git log --patch`
- Ensimmäisenä saamme tiedon viimeisimmästä muutoksesta, Saamme tiedon kuka sen teki ja milloin (Minä, Perjantai, 00:22). Seuraavaksi meille esitetään kommentti, jonka varaston muokkaaja on lisännyt, tässä tapauksella 'Add important information related this project'
- Toiseksi saamme tiedon reposition luomisesta. Tämän teki minun Github-käyttäjä. (Torstai,23:16)
![image](https://github.com/WindoCode/PalvelintenHallinta/assets/110290723/3c9944d7-ac35-4ca0-9dcb-ea743b1890b5)



