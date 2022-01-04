# 1. Gebruikers (en groepen) aanmaken
Het doel van deze opgave is om de opdrachten en de begrippen met betrekking tot gebruikers en groepen te bestuderen, binnen de context van Linux als een multi-user-systeem.

1. De VM komt met een default user osboxes - dat ben jij (nu toch nog)! Log in als deze gebruiker.
Geef hieronder telkens het commando en de uitvoer

```
su osboxes
```

Wat is het commando om de huidige directory op te vragen? In welke map bevind je jou nu?

```
pwd
```

Wat is het UID van deze gebruiker, wat is de GID?


```
id -u
id -g
```

2. Maak een nieuwe gebruiker aan met de naam alice, zonder specifieke opties. Werk hiervoor met adduser.
Voorzie een geschikt wachtwoord voor deze gebruiker (en vergeet het niet! Noteer het eventueel in je verslag of in de beschrijving van je VM


```
sudo adduser alice

sudo useradd alice
sudo password alice
```

3. Configuratiebestanden voor gebruikersbeheer
In welk bestand kan je de UID, gebruikersnaam, homedirectory, enz. van alle gebruikers terugvinden?
In welk configuratiebestand kan je al de bestaande gebruikersgroepen nakijken, en ook de gebruikers die lid zijn van elke groep?


```
cat /etc/passwd
```

In welk configuratiebestand vind je de wachtwoorden van alle gebruikers?


```
sudo cat /etc/shadow
```

4. Gebruikersgroepen aanmaken
Maak een groep aan met de naam sporten

```
groupadd sporten
```

In welk configuratiebestand vind je het GID van deze groep terug?

```
cat /etc/group
```

Wat zal het GID zijn van de groepen zwemmen en judo als je deze nu onmiddellijk zou aanmaken? Maak ze aan en controleer!

```
Zwemmen gaat id 1003 hebben en judo gaat id 1004 hebben. Nu wordt het telkens met één verhoogd.
```


Voeg de gebruiker alice toe aan de groepen sporten en zwemmen

```
sudo usermod -aG sporten alice
sudo usermod -aG zwemmen alice
```

Log in als alice door in een terminal het commando su - alice (let op de spaties!) uit te voeren
Zorg er nu voor dat de groep sporten de primaire groep wordt van alice.

```
sudo usermod -g sporten alice
```

Zorg er voor dat alice uitgelogd is, ga terug naar root
5. Maak nu de gebruikers in onderstaande tabel aan. Zorg er voor dat ze al meteen bij aanmaken tot de aangegeven groepen behoren. Kies zelf geschikte wachtwoorden voor deze gebruikers en vergeet ze niet (vul eventueel een kolom toe aan de tabel).

![image](https://user-images.githubusercontent.com/70543493/147927411-569c4878-622b-4a2b-b98f-bf05876a19ff.png)

Geef de gebruikte commando's om de gebruikers aan te maken en ook om te verifiëren of dit correct gebeurd is:

Opmerking!! Voeg '-m' toe om een homedirectory te maken bij creatie van een gebruiker. Dit werd niet expliciet gevraagd.

![image](https://user-images.githubusercontent.com/70543493/147928709-bc02f685-db57-4481-a7eb-5317439e0c92.png)

Verwijder nu de groep alice en controleer.

```
sudo groupdel alice
cat /etc/group | grep alice
```

Gebruiker daniel gaat een tijdje niet meer sporten. Zorg er voor dat deze gebruiker tot nader order geen toegang meer kan hebben tot het systeem (zonder het wachtwoord of de gebruiker te verwijderen!).

```
sudo usermod -L daniel
```

Hoe kan je controleren dat daniel inderdaad geen toegang meer heeft tot het systeem? In welk bestand kan dat en hoe zie je daar dan dat het account afgesloten is?

```
Er komt een uitroepteken bij zijn naam in /etc/passwd te staan.
```

Gebruiker daniel komt terug naar de sportclub. Geef hem opnieuw toegang tot het systeem.

```
sudo usermod -U daniel
```

Gebruiker eva stopt helemaal met sporten. Verwijder deze gebruiker, maar doe dit zorgvuldig: zorg er in het bijzonder voor dat ook haar homedirectory verwijderd wordt.

```
sudo userdel -r eva
cat /etc/passwd | grep eva
```

Log aan als de gebruiker carol
Controleer of je in de “thuismap” bent van deze gebruiker. Maak onder deze map een bestand test aan door middel van het commando touch.

```
su carol
pwd
touch test
```

Probeer nu als gebruiker carol je te verplaatsen naar de “thuismap” van alice.

```
sudo usermod -g sporten alice
```

Kan je de inhoud van de mappen binnen de thuismap van alice bekijken?
Probeer nu als carol onder de “thuismap” van alice ook een bestand test te maken. Lukt dit? Kan je dit verklaren?

```
su carol
pwd
cd
cd /home/carol
```

6.  Werken als root
Bekijk de eerste regels van het bestand /etc/shadow. Wat bemerk je bij de gebruiker root?
Log in als de root-gebruiker met het commando sudo -i (let op de spatie!) 
Wat is de home-directory van root?

```
pwd
/root
```

Wat is het UID van deze gebruiker, wat is de GID?

```
id -u
0
id -g
0
```

Stel, nog steeds ingelogd als root, een wachtwoord in voor root. Kies een uniek, nieuw wachtwoord!
Merk je de verandering in /etc/shadow?

```
sudo passwd
cat /etc/shadow | grep root
```

Log in als de root-gebruiker met het commando su - (let op de spatie!)
Log uit, en log opnieuw in met sudo su -. Wat wordt er anders?

```
Je moet je wachtwoord niet meer meegeven.
```

7. Jezelf toevoegen en admin maken
Voeg jezelf (met je eigen gekozen loginnaam) toe aan de VM waarin we werken. Gebruik hiervoor useradd -m 'jouw usernaam'
  

Bewerk /etc/passwd zodat je ook bash gebruikt als default shell.
  
```
sudo useradd -m dylan
sudo usermod -g adm dylan

sudo nano /etc/passwd

Ga naar de gebruiker en pas helemaal achteraan /bin/sh aan als /bin/bash
```
  
  
# Eigenaars en groepseigenaars veranderen
1. Maak als root onder /srv/ twee directories aan met de naam groep/verkoop/ en groep/inkoop/. Maak ook 2 groepen aan met de namen verkoop en inkoop. Maak twee gebruikers aan, margriet met primaire groep verkoop en roza, die als primaire groep inkoop heeft. Zorg dat de groepen eigenaar zijn van de overeenkomstige directories en dat margriet eigenaar is van directory verkoop en roza van het directory inkoop. Geef de gebruikte commando’s en controleer:

```
cd /srv
mkdir /groep

cd /srv/groep
mkdir /groep/inkoop
mkdir /groep/verkoop

groupadd verkoop
groupadd inkoop

sudo useradd -g verkoop margriet
sudo useradd -g inkoop roza

sudo chgrp inkoop inkoop
sudo chgrp verkoop verkoop

sudo chown margriet verkoop
sudo chown roza inkoop

Nu moet je ls -l doen om te zien of alles is doorgevoerd.
Kijk of roza/margriet er tussen staan én of inkoop/verkoop
ook vermeld wordt bij hun groepen.
```
  
2. Zorg ervoor dat gebruikers en groepen uit de vorige stap alle permissies hebben. Geef het geschikte commando en controleer.

```
chmod u+rwx inkoop
chmod g+rwx inkoop
chmod u+rwx verkoop
chmod g+rwx verkoop

ls -l om te checken
```

3. Voeg een gebruiker, vb alice, toe aan de groep inkoop en verkoop en controleer. Geen van beide groepen zijn primair.

```
usermod -aG inkoop alice
usermod -aG verkoop alice
```

4. Log in als alice en ga naar de directory verkoop. Laat de gebruiker hier een leeg bestand, bestand1, aanmaken in de directory verkoop. (Indien je hier problemen ondervindt, log dan in via een andere terminalvenster).

```
su alice
cd /groep/verkoop
touch bestand1
```

6. Zorg er nu voor dat de groepseigenaar van de directory verkoop automatisch de groepseigenaar wordt van alle bestanden en directories die onder verkoop gemaakt worden. Geef de gebruikte commando’s.

7. Doe hetzelfde voor de directory inkoop. Geef de gebruikte commando’s.

8. Verander opnieuw naar gebruiker alice en laat deze gebruiker een leeg bestand2 aanmaken in de directory verkoop. Geef de gebruikte commando’s.

9. Wie is nu eigenaar van bestand2 en wie groepseigenaar?

10. Laat nu gebruiker margriet een leeg bestand bestand3 aanmaken. Controleer de eigenaar van bestand3 en de groepseigenaar.

11. Laat nu gebruiker alice bestand3 verwijderen. Lukt dit?

12. Zorg er nu voor dat de gebruikers elkaars bestanden niet kunnen verwijderen. Als de gebruiker echter eigenaar is van het betreffende directory mag dit wel. Leg uit hoe je dit doet en controleer. Schrijf je gevolgde procedure op.

```
chmod +t /groep/verkoop
chmod +t /groep/inkoop
```


# 2. Streams, pipes, redirects
In onderstaande vragen is het telkens de bedoeling één commando te geven om de taak uit te voeren. Probeer altijd eerst om de uitvoer op de console te tonen voordat je wegschrijft naar het gevraagde bestand. Zo zie je sneller eventuele fouten.

1. Voeg het bestand landen en autokentekens samen met het commando join (zoek de werking ervan op met het man-commando). Het resultaat wordt opgeslagen in het bestand landenkentekens.

```bash
join landen autokentekens >> landenkentekens
```

Haal uit landenkentekens alleen kolom 2 en kolom 3 eruit en sla dit resultaat op als landenkentekens2.

```bash
cut -d' ' -f2,3 landenkentekens >> landenkentekens2
```

Voeg vanop de command-line Italië en Spanje toe aan het einde van landenkentekens2 met hun respectievelijke kentekens. Je mag hier voor elk land een apart commando gebruiken.

```bash
echo 'Italië I' >> landenkentekens2
echo 'Spanje ESP' >> landenkentens2
```

Sorteer landenkentekens2 alfabetisch op de autokentekens. Sla het bekomen resultaat op in gesorteerdeautokentekens. Controleer het resultaat.

```bash
sort -k2 landenkentekens2 > gesorteerdeautokentekens
```

België B
Zwitserland CH
Duitsland D
Spanje E
Frankrijk F
Italië I
Nederland NL

# 2.4.2. Filters

Bekijk de uitvoer van het commando ip a (opvragen van de IP-adressen van deze host). Filter de IPv4 (niet IPv6) adressen er uit:

127.0.0.1/8
10.0.2.15/24

```bash
ip a | grep 'inet '

of om specifiek enkel het IP-adres op te halen en géén andere info
sed -e --> whitespace verwijderen

ip a | grep 'inet ' | set -e 's/^[ \t]*//' | cut -d' ' -f2
ip a | grep 'inet ' | tr -s ' ' | cut -d' ' -f3
```

Werken met doorlopende tekst
Deze oefeningen gebeuren met lorem.txt

Tel het aantal regels, wooren en tekens in lorem.txt
  45  404 2738 lorem.txt
  
```bash
wc lorem.txt
```
  
Herformatteer lorem.txt zodat elke tekstregel max. 50 lettertekens bevat en nummer daarna elke (niet-lege) regel. Het resultaat wordt weggeschreven in een nieuw bestand, nlorem.txt.

```bash
fmt -50 lorem.txt | nl
```

Druk een lijst af van alle individuele woorden in lorem.txt (negeer hoofdletters), samen met het aantal keer dat elk woord voorkomt. De lijst is omgekeerd gesorteerd op aantal voorkomens, en dan alfabetisch geordend. Werk stap voor stap:

Maak eerst een lijst van woorden, m.a.w. druk lorem.txt af met elk woord op een aparte regel
Verwijder overblijvende leestekens (, en .) en lege regels
Sorteer de woorden (niet hoofdlettergevoelig)
Maak een lijst met voor elk woord het aantal keer dat het voorkomt
Sorteer op het aantal voorkomens en behoud de alfabetische sortering van de woorden
 11 sed 
 10 et 
  8 quis 
  7 eget 
  7 mi 
  6 in 
  6 nec 
  6 nunc 
  6 tortor 
  5 ac 
  5 accumsan 

```bash
cat lorem.txt | tr 'A-Z' 'a-z' | tr -s '.,?' | tr ' ' '\n' | sort | uniq -c | sort -k1 -r -n
```

# Gestructureerde tekst
Vele tekstbestanden zijn gestructureerd als tabellen, bv. CSV (comma-separated values). Op een Linux-systeem zijn verschillende configuratiebestanden ook op zo'n manier opgedeeld, maar dan vaak met een : als scheidingsteken. In de volgende oefeningen werken we met het bestand passwd, dat je gedownload hebt.

Schrijf in users.txt een gesorteerde lijst weg van gebruikers met een UID strikt groter dan 1000 (tip: gebruik hiervoor awk). De inhoud van het bestand moet er zo uit zien:

roberts
ryu
sparrow
student
teach
yoshiro

```bash
awk -F: '$3>1000{ print $1 }' passwd > users.txt
```

Tel het aantal gebruikers in users.txt

```bash
wc -l users.txt
```


Genereer voor elke gebruiker in users.txt een nieuw wachtwoord m.h.v. het commando apg -n AANTAL en schrijf deze weg in newpass.txt. Het aantal gebruikers in users.txt wordt berekend in de opdrachtregel. Tip: gebruik "command substitution," notatie $(commando). Dit zal het gegeven commando uitvoeren en de uitdrukking $(...) vervangen door de uitvoer (stdout) ervan.

```bash
apg -n $( wc -l users.txt ) > newpass.txt
```

Maak een tekstbestand newusers.txt met daarin de lijst van gebruikers uit users.txt en hun overeenkomstige wachtwoord uit newpass.txt, gescheiden door een TAB. Controleer de inhoud van newusers.txt:

```bash
paste users.txt newpass.txt > newusers.txt
```

$ cat newusers.txt
roberts hewpopIrb6
ryu     vicNimEp
sparrow whowlash4
student Evcorfoi
teach   cymgardye
yoshiro sirgElkont

Converteer newusers.txt naar een CSV-bestand newusers.csv waar de inhoud van elke kolom omgeven is door dubbele aanhalingstekens en gescheiden door een kommapunt. Controleer de inhoud van newusers.csv

$ cat newusers.csv
"roberts";"hewpopIrb6"
"ryu";"vicNimEp"
"sparrow";"whowlash4"
"student";"Evcorfoi"
"teach";"cymgardye"
"yoshiro";"sirgElkont"
Druk een lijst af van de gebruikers in passwd die Bash als shell hebben, samen met hun UID en home-directory. Sorteer op UID. De uitvoer moet er zo uitzien:

root:0:/root
vagrant:1000:/home/vagrant
student:1001:/home/student
ryu:1002:/home/ryu
yoshiro:1003:/home/yoshiro
sparrow:1004:/home/sparrow
teach:1005:/home/teach
roberts:1006:/home/roberts

# 2.4.3 Variabelen
Geef zoals gewoonlijk het commando om de opgegeven taak uit te voeren en controleer ook het resultaat.

Druk met behulp van de juiste systeemvariabele de gebruikte bash-versie af op het scherm. Geef het gebruikte commando weer.

Je bent ingelogd als gewone gebruiker.

Maak een variabele pinguin aan en geef deze de waarde Tux.
Hoe kan je de inhoud opvragen van deze variabele en afdrukken op het scherm?
Open nu een sub(bash)shell in je huidige bashomgeving.
Hoe kan je controleren dat er nu twee bashshells actief zijn en dat de ene een subshell is van de andere?
Probeer nu in deze nieuwe subshell de inhoud van de variabele PINGUIN af te drukken op het scherm. Lukt dit?
De verklaring hiervoor ligt in het type variabele. Welke soort variabele is PINGUIN en hoe kan je dit controleren? Keer hiervoor terug naar je oorspronkelijke bashshell
Zorg er nu voor dat de inhoud van PINGUIN ook in elke nieuwe subshell kan gelezen worden? Hoe doe je dit? Schrijf het gebruikte commando neer.
Open opnieuw een sub(bash)shell in je huidige bashomgeving en controleer of je nu de inhoud van PINGUIN kan lezen. Welk soort variabele is PINGUIN nu? Doe dan ook de controle.
Zoek de inhoud en betekenis op van volgende shellvariabelen en vul volgende tabel aan:

# 3. Package management (debian)

We verkennen onderstaande opdrachten vanuit de Linux Mint VM.

1. Installeer onderstaande applicaties of “packages”. Zorg er voor dat je dit zowel via de grafische gebruikersinterface kan als vanop de command-line.

git
ShellCheck
vim
vim-gtk3


```bash
apt install git shellcheck vim vim-gtk3
```

2. Download de volgende package sl from the Ubuntu system: https://packages.ubuntu.com/focal/games/sl
Or more aware of your CPU - from https://packages.ubuntu.com/focal/amd64/sl/download
Installeer vervolgens dit gedownload .deb bestand.
Kan je het commando sl nu laten werken in de CLI?

```bash
installeer sl
commandline: apt install sl
man sl
```

3. Kan je hetzelfde doen met de package cavepacker? https://packages.ubuntu.com/focal/amd64/cavepacker/download, tenzij je een ander type processor hebt.
Kan je met dpkg de informatie opvragen over deze package (zie man page)? Waardoor loop je vast?

4. Haal een config bestand uit een package.
Download deze package http://ftp.de.debian.org/debian/pool/main/i/isc-dhcp/isc-dhcp-server_4.4.1-2.3_arm64.deb

1. Waarom kan deze package nooit compatibel zijn met jouw VM?

2. Kan je alle bestanden oplijsten die hierin vervat zitten? 

3. Hoe haal je de dhcpd.conf uit dit bestand?

5. Update de lijst van beschikbare software op je systeem. Toon een lijst van bij te werken packages. 
Kan je één package updaten (en de rest van de lijst niet), e.g. firefox?


```bash
apt list --upgrade
apt upgrade firefox -- firefox upgraden
```

# DHCP server installatie
In de volgende labo opdracht koppelen we jouw kennis van het installeren van software, en het instellen van IP adressen aanéén. We installeren én configureren een DHCP server op de (server) VM, en laten de GUI-VM client worden van deze nieuwe server. 

DHCP server
1. Log in met ssh op de server. Dit laat je toe om te werken vanuit de GUI op deze server (en dus ook copy-paste e.d. te kunnen gebruiken). Dit kan met 
ssh -l admin 192.168.76.2

```bash
ssh -l admin 192.168.76.2
```

2. Gebruik de zoekmogelijkheid van dnf om een package te vinden die corresponeert met de ISC DHCP server. Wat is de package naam? Installeer.

```bash
dnf search isc dhcp
sudo dnf install dhcp-server
```

3. Bestudeer het bestand /usr/share/doc/dhcp-server/dhcpd.conf.example. Wat heb je nodig om enkel in één subnet IP-adressen uit te delen? Wat is m.a.w. een minimale configuratie?

```bash
Je moet een range meegeven.

subnet ... netmask ... {
  range startIP endIP;
}

```


4. Kopieer de relevante informatie naar /etc/dhcp/dhcpd.conf. Deel enkel adressen uit van 192.168.76.100 tot 150. Start de service. 

```bash
sudo dnf install nano
sudo nano /etc/dhcp/dhcpd.conf

in file:
subnet 192.168.76.0 netmask 255.255.255.0 {
  range 192.168.76.100 192.168.76.150;
}

sudo systemctl enable dhcpd
sudo systemctl start dhcpd
```

5. Kan je de status van de service opvragen?

```bash
sudo systemctl enable dhpd
```

DHCP client
In het netwerk is de Linux Mint VM de enige client - maar die gebruikt momenteel nog een statisch adres!

1. Test je server door op de Mint CLI handmatig een DHCP client op te starten: dhclient -i 'interface'. 

```bash

```

2. Bekijk je IP-adressen op deze VM. Welke adressen heeft de 2e netwerkkaart nu?

```bash
ip a | grep 'inet '
```

3. Kan je in de logfiles op de server in de map /var/lib/dhcpd terugvinden welk adres je VM gekregen heeft? Hoe lang kan je dit gebruiken (m.a.w. wat is de lease time?).

```bash
cat /var/lib/dhcpd/dhcpd.leases | grep ends
```

4. Pas het bestand /etc/network/interfaces aan zodat deze 2e netwerkkaart geen statisch, maar een dynamisch IP-adres krijgt d.m.v. DHCP (nu ja, jouw DHCP server!). Herstart de netwerking op de Mint VM. Welk(e) adres(sen) heeft jouw 2e netwerkkaart nu nog?

```bash
statisch ip-adres voorbeeld: 
iface eth0 inet static
address 192.168.1.5
netmask 255.255.255.0
gateway 192.168.1.254

dhcp voorbeeld:
auto eth0
iface eth0 inet dhcp
```

# Scripting 102

De unit tests van de oefeningen worden in volgorde uitgevoerd. Zolang er nog fouten in een oefening gevonden zijn, worden de tests van de volgende nog niet uitgevoerd.

Maak een script met de naam onderelkaar.sh die de op de command line als argumenten ingevoerde zin woord per woord onder elkaar afdrukt op het scherm. Als de gebruiker geen argumenten opgegeven heeft, wordt er een gepaste foutboodschap op stderr afgedrukt en stopt het script met een foutcode (exit-status verschillend van 0). Voorbeeld van de uitvoer:

```bash
#!/bin/bash
set -o nounset
set -o errexit
set -o pipefail

for word in "$@"
do
    echo $word
done
```

Schrijf een script gebruikerslijst.sh dat een gesorteerde lijst van users (uit /etc/passwd) weergeeft op het scherm. Maak gebruik van het het commando cut.

```bash
#!/bin/bash

set -o pipefail
set -o errexit
set -o nounset

cat /etc/passwd | cut -d: -f1 | sort
```

Schrijf een script elf-params.sh dat maximaal 11 parameters kan weergeven op het scherm. Extra parameters worden genegeerd. (Tip: gebruik positionele parameters en shift)

```bash
#!/bin/bash

set -o errexit
set -o pipefail
set -o nounset

count=0

while [ $count -lt 11 ]
do
    echo ${1}
    
    if [ -z $2 ]; then
      exit 1
    fi
    
    shift
    let count=count+1
done
```

Schrijf een script datum.sh dat het aantal elementen van het commando date weergeeft en daarna al de elementen onder elkaar. Maak gebruik van positionele parameters en het set commando. Gebruik ook een while-lus.

```bash
#!/bin/bash

set -o nounset
set -o errexit
set -o pipefail

datum=$(date)

echo ${datum}

for element in ${datum}
do
    echo $element
done
```


# 4. Webserver opzetten
In dit labo zullen we een webserver opzetten in de (server) VM die je in het vorige labo gemaakt hebt. Een van de populairste toepassingen van Linux als server is de zgn. LAMP-stack. Deze afkorting staat voor Linux + Apache + MySQL + PHP. De combinatie vormt een platform voor het ontwikkelen van webapplicaties waar vele bekende websites (bv. Facebook) op gebaseerd zijn.

De Apache webserver installeren
Het is belangrijk dat je controleert voordat je aan dit labo begint, dat je twee netwerkinterfaces hebt op je virtuele machine. De ene moet van het type NAT zijn. Deze heeft verbinding met het internet en heeft typisch als IP-adres 10.0.2.15. De andere netwerkinterface moet van het type Internal Network zijn. Via deze kan je communiceren met de Linux GUI VM en je webserver testen. Als je niets hebt veranderd de standaardinstellingen van VirtualBox, is het server IP-adres hoogstwaarschijnlijk 192.168.76.2.

```bash
sudo dnf install httpd

systemctl status httpd
systemctl start httpd

curl 127.0.0.1
curl localhost
```

Installeer Apache op je virtuele machine en verifieer dat hij draait en vanop de GUI VM bereikbaar is.
Installeer ondersteuning voor PHP en verifieer dat dit werkt, bijvoorbeeld met een eenvoudige PHP-pagina

MariaDB (MySQL)
MariaDB is de naam van een variant (fork) van de bekende database MySQL. Op sommige Linux-distributies (zoals Fedora) is MySQL zelfs niet meer beschikbaar. MariaDB is wel grotendeels compatibel en kan perfect dienen als vervanger. Installeer MariaDB op je virtuele machine. Voer daarna het script mysql_secure_installation uit om het root-wachtwoord voor MariaDB in te stellen. 

Testing & logfiles
Met welk commando test je of een host op het netwerk op dit moment online is? Probeer dit uit vanop je VM met het IP-adres van je host-systeem en voeg de uitvoer hieronder in. Welk protocol uit de TCP/IP familie wordt door deze tool gebruikt?

```bash
curl localhost
curl 127.0.0.1

TCP wordt hier gebruikt.
```

Met het commando ss -tln kan je opvragen welke services er draaien op je systeem, ahv. de open netwerkpoorten. Leg uit wat de opties (-tln) betekenen. Probeer het commando uit op je VM wanneer Apache en MariaDB draaien en voeg de uitvoer hieronder in. Geef voor elke open poort beneden de 10.000 welke netwerkservice er mee geassocieerd is.

```bash
-tln
t staat voor tcp
l staat voor sockets die nu aan het 'listening' zijn
n
p staat voor het PID
```

Met welk commando kan je de logs voor een bepaalde netwerkservice bekijken?

```bash
journalctl -u httpd
journalctl -u mariadb-server
```

Wat is de naam van het logbestand waar je kan opvolgen welke webpagina's er opgevraagd worden aan je webserver?

```bash
sudo cat /var/log/httpd/access_log
```

Open dit bestand met tail -f en laad een webpagina via een webbrowser. Wat gebeurt er in het logbestand?

```bash
sudo cat /var/log/httpd/access_log | tail -f
curl localhost

Resultaat: Er komt een lijntje bij. Dit met de datum en ook het adres die de site wou bezoeken.
```
