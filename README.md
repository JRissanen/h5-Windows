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
Varmistin vielä, että uusi tekstitiedosto tosiaan luotiin ja että sen sisältö on oikea.

![Screenshot 2022-11-27 143213](https://user-images.githubusercontent.com/116954333/204136499-222f6470-0a3d-438f-9797-2e61b5c5e0dd.png)

Hyvältä näytti.

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
















