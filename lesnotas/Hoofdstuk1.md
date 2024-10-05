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