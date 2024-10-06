# Hoofdstuk 1

## Configuratie

SSH sleutelpaar genereren. Dit heb ik gedaan met het volgende commando:  
`ssh-keygen -t rsa -b 4096 -C "jonas.vandael@student.hogent.be"`  
De standaardlocatie van de sleutels is de verborgen map `.ssh` in de home directory. De publieke sleutel is deze met als naam `id_rsa.pub`. Deze kan je bekijken met het commando `cat ~/.ssh/id_rsa.pub`.  
Daarna sleutel in GitHub plakken, is eenvoudig. Je kan de SSH-verbinding met GitHub testen aan de hand van het volgende commando: `ssh -T git@github.com`

## Package management

### 1. Update de lijst van beschikbare software op je systeem. Toon een lijst van bij te werken package (zonder deze ook daadwerkelijk bij te werken!)

`sudo apt update`

### 2. Kan je één package updaten (en de rest van de lijst niet), bv. Firefox?

`sudo apt install firefox`

### 3. Installeer onderstaande applicaties of “packages”. Zorg er voor dat je dit zowel via de grafische gebruikersinterface kan als vanop de command-line.

`sudo apt install git
sudo apt install jq
sudo apt install shellcheck
sudo apt install vim-gtk3`

### 4. Ga naar de website van Visual Studio Code, download en installeer de .deb-variant. Installeer ook nuttige plugins zoals Markdown All In One, Markdownlint, ShellCheck, GitLens, Git Graph

<https://code.visualstudio.com/>

### 5. Download de package sl van de Ubuntu

Nu kan met het commando `sl` een animatie gegenereerd worden van een trein.

### 6. Kan je hetzelfde doen met de package cavepacker? 

Dit lukt niet op deze manier doordat er een dependency ontbreekt. IK heb het wel kunnen installeren met apt.

### 7. Haal een config bestand uit een package

- Waarom is deze package niet compatibel met jouw VM?
    De package is specifiek voor de s390x architectuur, wat een IBM System/390 en zSEries is. 
- Kan je alle besetanden oplijsten di ehierin vervan zitten?
    Met het commando `dpkg -c isc-dhcp-common_4.4.3-P1-5_s390x.deb`
    De -c staat voor contents.
- Hoe haal je de dhcpd.conf uit dit bestand?
    Om het bestand dhcpd.conf uit de package te halen, moet je eerst de package extraheren. Dit kan met het dpkg-deb-commando: `dpkg-deb -x isc-dhcp-common_4.4.3-P1-5_s390x.deb ./uitgepakt`
    Ik vind het dhcpd.conf niet terug in dit bestand.

### 8. "Hypnotix" en "Rhythmbox" zal je niet nodig hebben. Verwijder deze applicaties.

Dit is eenvoudig met volgende commando's:
`sudo apt remove hypnotix`
`sudo apt remove rhytmbox`

## Curl

Handige link: <https://curl.se/docs/tutorial.html>

### 1. Gebruik curl om je publieke IP-adres op te vragen bij icanhazip.com

`curl icanhazip.com`

### 2. Gebruik curl om de url https://hmpg.net/ op te halen

- Laat eerst afdrukken op de terminal
  `curl https://hmpg.net/`
- Sla de pagina op in een bestand
  `curl -o pagina.html https://hmpg.net/`
- Open het gedowloade bestand in een webbrowser. Vergelijk met het origineel. Lukt dit zoals je zou verwachten?
  De afbeeldingen van de pagina ontbreken.
- Dowload de pagina opnieuw. Wordt het reeds bestaande bestand overschreven of niet? Gedraagt curl zich hier anders dan het gelijkaardige commando wget?
  Ja, het bestand wordt overschreven als je exact hetzelfde commando ingeeft. Wanneer je gebruik maakt van wget slaat hij de pagina op als een bestand en bij een nieuwe uitvoering van het commando voeggt het een volgnummer toe.
  Met curl kan je dit voorkomen op de volgende manier:
  `curl -o "pagina_$(date +%s).html" https://hmpg.net/`

### 3. Wat gebeurt er als je https:// in de vorige URL weglaat?

- Wat zie je nu als je het resultaat op de terminal laat afdrukken?
  De inhoud van een webpagina die meedeelt dat de website verplaatst is.
- Zoek de optie om de HTTP headers te tonen en zoek in de uitvoer naar de status van het resultaat van de query. Welke statuscode krijg je hier? Welke krijg je met de https-URL?
  Als je zowel de header als webpagina wilt zien: `curl -i hmpg.net`
  Als je enkel de header wilt zien: `curl -I hmpg.net`
- Kan je er voor zorgen dat curl zo'n redirect automatisch volgt?
  Ja, met de optie -L: `curl -L hmpg.net`

### 4. Met curl kan je ook met een FTP-server interageren. Tegenwoordig wordt dat minder gedaan, maar soms komt dit wel nog van pas.

- Ga naar http://ftp.belnet.be/debian/ en bekijk het bestand README.
- Gebruik nu curl om dit README-bestand te downloaden via FTP (dus NIET via http(s)!).
`curl ftp://ftp.belnet.be/debian/README`
- Probeer hetzelfde, maar nu geef je een gebruikersnaam op (anonymous) en een leeg wachtwoord
Dit kan je doen met volgende structuur : `ftp://gebruikersnaam:wachtwoord@ftp-server/adres`
Dus in dit geval met een leeg wachtwoord: `curl ftp://anonymous:@ftp.belnet.be/debian/README`

### 5. Curl is vooral interessant om met REST-API's te communiceren

- Haal de URL <https://en.wikipedia.org/api/rest_v1/page/random/summary> op, volg eventuele redirects. Wat is het verschil van de uitvoer met een "gewone" webpagina?
Dit retourneerd altijd gegevens in JSON formaat i.p.v. het op te maken in een webpagina met html.
- Je kan de uitvoer beter leesbaar maken door te "pipen" naar jq.
`curl -L https://en.wikipedia.org/api/rest_v1/page/random/summary | jq`
- Als je de uitvoer van curl omleidt, dan wordt er een progress bar getoond. Zoek de optie om deze te verbergen
Dit kan je uizetten met de optie -s of --silent
`curl -Ls https://en.wikipedia.org/api/rest_v1/page/random/summary | jq`

### 6. Gebruik curl om via het Open Data Portaal van Stad Gent het real-time aantal vrije plaatsen in de openbare Gentse fietsenstallingen op te halen.

- Als je de evolutie van het aantal vrije plaatsen in de tijd zou willen bijhouden, dan herhaal je deze query op geregelde tijdstippen (later zien we hoe je dat doet). Zorg er voor dat het resultaat opgeslagen wordt in een bestand met een "timestamp" in de naam, bv: fietsenstallingen-JJJJMMDD-uummss.json. Tip: gebruik het commando date en command substitution.
`curl -o "fietsenstallingen-$(date +'%Y%m%d-%H%M%S').json"  "https://data.stad.gent/api/explore/v2.1/catalog/datasets/real-time-bezettingen-fietsenstallingen-gent/records?limit=20"`
