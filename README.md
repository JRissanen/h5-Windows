# h5-Windows
This is a repository for exercise h5 for Configuration Management Systems course

__a) Hello Window Salt! Tee Windowsille SLS-tiedostoon Salt-tila, joka tekee tiedoston nimeltä "suolaikkuna.txt".__

En ollut vielä aikaisemmin tehnyt Windowsilla mitään muita Salt harjoituksia, kuin tunnilla tehdyt tehtävät, joten aloitin lähes puhtaalta pöydältä.</br>
Aloitin tekemällä hakemiston: `mkdir C:\Users\Julius\temppi\srv\salt`. </br>
Tein myös harjoitukselle oman hakemiston: `mkdir suolaikkuna`. </br>
Tein uuden `init.sls`-tiedoston salt tilaa varten: `notepad.exe .\init.sls`

![Screenshot 2022-11-27 135858](https://user-images.githubusercontent.com/116954333/204135748-95401cf7-2c06-42c7-b672-f32c14ec05ed.png)

`init.sls`-tiedoston sisältö: </br>
```
C:\Users\Julius\temppi\srv\salt\suolaikkuna\suolaikkuna.txt:
  file.managed:
    - contents: "Jotain sisältöä..."
```

Seuraavaksi yritin ajaa tilaa saltilla: `salt-call --file-root=. --local state.apply suolaikkuna`. </br>
Tämä ei kuitenkaan onnistunut, vaan antoi virheilmoituksen: </br>
```
local:
    Data failed to compile:
----------
    No matching sls found for 'suolaikkuna' in env 'base'
```

En tiedä mistä kyseinen virheilmoitusjohtui, mutta päätin kokeilla ajaa tilaa uudestaan koko polulla: </br>
`salt-call --file-root=C:\Users\Julius\temppi\srv\salt --local state.apply suolaikkuna`.

![Screenshot 2022-11-27 143020](https://user-images.githubusercontent.com/116954333/204136122-a7043a9e-f0d4-44a2-953a-fc69eb9727f3.png)

Kaikki näytti nyt toimivan niinkuin pitääkin. </br>
Varmistin vielä, että uusi tekstitiedosto tosiaan luotiin ja että sen sisältö on oikea. </br>
Hyvältä näytti.

![Screenshot 2022-11-27 143213](https://user-images.githubusercontent.com/116954333/204136499-222f6470-0a3d-438f-9797-2e61b5c5e0dd.png)


__b) Ei vihkoa, ei kynää. Kerää Windows-koneen tekniset tiedot tekstitiedostoon. Vapaaehtoinen bonus: Saatko tiedot tallennettua myös json-muodossa?__

PowerShellissä komennolla: `Get-CopmuterInfo` saa tietoja koneesta. </br>
Aloitin kirjoittamalla komennon, joka kertoo tietoja koneesta ja samalla tallentaa tiedot uuteen tekstitiedostoon: </br>
`Get-ComputerInfo > C:/Users/Julius/temppi/tietokoneen_tietoja.txt` </br>

Tarkistin sen jälkeen, että uusi tekstitiedosto tietokoneen tiedoilla varmasti luotiin. </br>
Siirryin hakemistoon, johon määritin tiedoston tallennettavaksi: `cd C:/Users/Julius/temppi/` </br>
Käytin komentoa: `ls` listatakseni hakemiston sisällön, ja siellä näkyi uusi teskstitiedosto. </br>
Tarkistin sen sisällön vielä komennolla: `notepad.exe .\tietokoneen_tietoja.txt`. </br>
Kaikki näytti hyvältä.

![Screenshot 2022-11-29 122819](https://user-images.githubusercontent.com/116954333/204505296-348f3b56-44db-4aa3-9eac-8c91cd8f13c5.png)

__c) Kop kop. Onko TCP-portti auki vai kiinni? Näytä esimerkit portin kokeilusta Linuxilla ja Windowsilla. Näytä kummallakin käyttöjärjestelmällä ainakin yksi avoin ja yksi suljettu portti. (Kokeile tätä vain omaan koneeseesi. Vieraiden koneiden ja verkkojen porttiskannaaminen on kiellettyä. Yksittäisen portin testaavat komennot ovat suositeltavia, esim. nc, tnc)__

Aloitin kokeilemalla Linux järjestelmälläni. </br>
Käytin komentoa: `ss` listaamaan tietoa eri porteista. </br>
`sudo ss -ltnp` </br>
Käytin päätettä `-ltnp` rajatakseni hakua, ettei näytölle tule turhaa tietoa. </br>
Lisäsin komennon perään vielä: `|grep salt` päätteen, jotta saisin vain Saltin tiedot. </br>
`sudo ss -ltnp|grep salt`

Sitten kokeilin saako Salt-masterin portteihin yhteyden muodostettua: </br>
`nc -vz <ip-osoite> <porttinumero>` </br>
Aiemmin listattujen porttien mukaan Salt-masterille kuului portit 4505 ja 4506 ja näihin myös sai yhteyden. </br>
Kokeilin vielä porttia 4507 ja siihen ei saanut yhteyttä.

![Screenshot 2022-11-29 163054](https://user-images.githubusercontent.com/116954333/204568805-46a77c72-2f36-46f5-9697-5176ad9df401.png)

Windowsin PowerShellillä en saanut portti scannausta onnistumaan. </br>
Selvitin ip-osoitteen komennolla: `ipconfig`.</br>
Kokeilin mudostaa yhteyttä komennolla: `tnc <ip-osoite> -p <porttinumero>` </br>
Se ei kuitenkaan toiminut, vaan antoi virheilmoitusta.

![Screenshot 2022-11-29 174614](https://user-images.githubusercontent.com/116954333/204575957-642f4ec7-67b3-4e55-a7e4-c7fba849d5d6.png)

Sitten kokeilin Saltin omien sivujen ohjeiden mukaisesti avata portit 4505 ja 4506: </br>
`netsh advfirewall firewall add rule name="Salt" dir=in action=allow protocol=TCP localport=4505-4506` </br>
Kokeilin uudestaan muodostaa yhteyttä, mutta vieläkin sama virheilmoitus:

![Screenshot 2022-11-29 174930](https://user-images.githubusercontent.com/116954333/204576809-57a7d916-b878-478b-a9f9-1d772b640b56.png)

Ajoin komennon: `Get-NetTCPConnection`, mutta silläkään ei tullut portteja 4505 ja 4506 näkyviin.

__Lähteet__

https://terokarvinen.com/2022/palvelinten-hallinta-2022p2/?from=MoodleNews#h1-hello-salt

https://repo.saltproject.io/#windows

https://winbuzzer.com/2020/09/08/how-to-check-pc-specs-with-windows-10-system-information-powershell-and-more-xcxwbt/

https://docs.saltproject.io/en/latest/topics/tutorials/firewall.html

https://adamtheautomator.com/netstat-port/?utm_source=youtube&utm_medium=video&utm_campaign=techsnips-end
