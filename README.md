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

# 5. Webserver hardening
Als eerste stap beperken we de ruimte voor de mysql server: we perken het IP adres in, en veranderen het poortnummer.

MariaDB IP-adres veranderen
De configuratie van de mysql server vind je typisch in het bestand my.cnf. Bekijk dit bestand. Waar vind je op deze server de werkelijke configuratie terug? Maak een backup van dit 'mariadb...cnf' bestand door het te kopiëren voor je start met het bewerken. 

```bash
cd /etc/my.cnf.d
sudo cp mariadb-server.cnf mariadb-server.cnf.kopie
```

Ga de status van de mariadb server na met systemctl. Verifiëer het IP-adres en poortnummer van de service met ss.

```bash
systemctl status mariadb-server
systemctl start mariadb-server
ss -tln toont aan dat dit op poort '3306' is
```

Open het configuratiebestand. Plaats bij de sectie [server] in het bestand een variabele bind_address, en zet de waarde op het "intnet" IP-adres van je server. Herstart de service, en verifiëer met ss dat de mariadb service enkel nog actief is op dit IP-adres. 
Installeer de package netcat. Maak een test-verbinding met de mariadb server: nc -nvz [IP-adres] 3306. Test dit eveneens met de twee andere IP-adressen van je server (NAT, localhost). Verifiëer dat dit niet langer werkt! 

```bash

cd /etc/my.cnf.d
sudo nano mariadb-server.cnf

//
Bij server:
[server]
bind_address=192.168.76.2
//

na configureren van bestand:

systemctl restart mariadb-server
ss -tln
```



MariaDB poortnummer veranderen

3306 is een te publiek gekende poort - en trekt gemakkelijk online de aandacht van hackers. Laten we een alternatieve poort zoeken: welk poortnummer vind je in /etc/services terug voor het niet langer gebruikte UniSQL protocol? Noteer.

```bash
cat /etc/services | grep unisql
toont poort 1978 als mogelijkheid
```

Bewerkt opnieuw het mysql configuratiebestand, en plaats een waarde port onder het IP-adres van hierboven. Stel op deze manier het poortnummer in op dit van UniSQL.Herstart de mariadb service.
Als alles goed gaat, zal je mariadb server niet opstarten! Immers: SELinux laat slechts een beperkt aantal poorten toe voor MariaDB. Ga op https://dev.mysql.com/doc/refman/8.0/en/selinux-context-mysqld-tcp-port.html na hoe je het hierboven gekozen poortnummer kan toevoegen aan de juiste SELinux context. Herstart nu nogmaals je mariadb service - tot hij werkt.
Test je (mariadb) server met nc -nvz [IP-adres] [nieuw poortnummer].

```bash
sudo nano mariadb-server.conf

//
[server]
bind_address=192.168.76.2
port=1978
//

systemctl restart mariadb-server

foutmelding!

sudo semanage port -a -t mysqld_port_t -p tcp 1978

sudo dnf install ns

nc -nvz 192.168.76.2 1978
```


MariaDB directory veranderen

Standaard bewaart de DB server zijn data in de map /var/lib/mysql. Maak een map /dbdata aan op je server. Verander de eigenaar en de groep van deze map naar mysql:mysql - het is deze gebruiker die operaties uitvoert op de map als de service zaken verandert!
Wijzig in het config bestand de default locatie voor de database data naar deze map.
Herstart de mariadb service. Als alles goed gaat, zal je mariadb server niet opstarten! Opnieuw gaan we ook SELinux nog moeten aanpassen opdat deze map gebruikt mag worden.
Ga na op https://mariadb.com/kb/en/selinux/ welke aanpassingen je nog moet uitvoeren op je nieuwe map. 
Herstart nu nogmaals je mariadb service - tot hij werkt.
mysql data input

```bash

cd /
mkdir /dbdata
sudo chown mysql:mysql /dbdata
ls -l | grep dbdata (om te controleren, kijk of er bij dbdata ook twee maal mysql staat)

sudo nano /etc/my.cnf.d/mariadb-server.cnf

//[mysqld]
datadir=/dbdata
..
//

start niet op!

sudo semanage fcontext -a -t mysqld_db_t "/dbdata(/.*)?"
sudo restorecon -Rv /dbdata
```

Nu we een werkende database server hebben (weliswaar op een ingeperkt IP, een andere poort en een niet conventionele map), kunnen we de sql database ook initialiseren en er een set data aan voeden:

```bash
sudo mysql_secure_installation

current root pwd: niks ingeven

set root pwd: Y
              wachtwoord kies je zelf, ik koos 'root' twee maal
              
Enkel op 'disallow root login' 'N' antwoorden. De rest 'Y'.

```

Stel een (mysql) root wachtwoord in voor deze server met mysqladmin password
Verbind met de mysql server op de CLI met mysql -u root -p. Geef het gekozen wachtwoord in.
Maak een gebruiker aan die toegang krijgt tot een test-database. In SQL: 

```sql
CREATE DATABASE IF NOT EXISTS trialsite;
GRANT ALL ON trialsite.* TO 'www_user'@'%' identified by 'YourSitePassword';
FLUSH PRIVILEGES;
```

Voer met deze nieuwe gebruiker een set van data in in jouw database:
```sql
mysql --user="www_user" --password="YourSitePassword" "trialsite" << _EOF_
DROP TABLE IF EXISTS trialsite_tbl;
CREATE TABLE trialsite_tbl (
  id int(5) NOT NULL AUTO_INCREMENT,
  name varchar(50) DEFAULT NULL,
  PRIMARY KEY(id)
);
INSERT INTO trialsite_tbl (name) VALUES ("Mr. IPtables");
INSERT INTO trialsite_tbl (name) VALUES ("Mrs. SELinux");
_EOF_
```

Test je gecreëerde database met het volgende bash script test_db.sh. Je kan dit zelf aanmaken op je server system:

```
#!/bin/bash
test_database='trialsite'
test_table='trialsite_tbl'
test_user='www_user'
test_password='YourSitePassword'

mysql --host=192.168.76.2 --port=1978 \
  --user="${test_user}" \
  --password="${test_password}" \
  "${test_database}" \
  --execute="SELECT * FROM ${test_table};"
```

Als je succesvol test, is je screen output het volgende:
[admin@server ~]$ bash test_db.sh 
+----+--------------+
| id | name         |
+----+--------------+
|  1 | Mr. IPtables |
|  2 | Mrs. SELinux |
+----+--------------+

# Webserver [apache2]
In dit vervolg zetten we enerzijds een php pagina op die kan verbinden met de database server; anderzijds gaan we aan de slag met het firewall-cmd.

Testen basisconnectie

Surf vanop je Linux GUI VM naar jouw webserver op 192.168.76.2. Waardoor kan je niet verbinden, als dit niet zou werken?
Pas de firewall op de server aan opdat deze verbinding wel kan tot stand komen.
PHP script met SELinux context

```bash
staat mijn firewall aan:
systemctl status firewalld

welke services heb ik staan in mijn firewall?
sudo firewall-cmd --list-all

http én https toevoegen
sudo firewall-cmd --add-service=http
sudo firerwall-cmd --add-servce=https

checken:
sudo firewall-cmd --list-all

testen of je kan verbinden vanop de client (dus niet de server):
--> via browser op Mint vm
--> via commandline op Mint vm is 'curl 192.168.76.2'
```

Installeer de extra package php-mysqlnd op je server. Deze laat toe om via php te communiceren met een SQL server - wat we vervolgens gaan doen.

```bash
dnf install php-mysqlnd
```

Ga naar jouw home folder (cd ~). Download hier het bestand test.php vanaf http://157.193.215.171/test.php

```bash

cd

bestand ophalen:
'wget http://157.193.215.171/test.php'

ls -l
```

Verplaats (met mv) vervolgens dit bestand naar de map /var/www/html.

```bash

sudo mv test.php /var/www/html

ls -Z /var/www/html

```


Dit bestand is momenteel eigendom van jou - de downloader. Ga dit na met het ls -Z commando.
Echter, bestanden in deze map moeten toebehoren aan de juiste context. Neem de map /var/www/html als referentie, en stel dezelfde SELinux instellingen in voor dit nieuwe bestand. Hint: https://linuxconfig.org/introduction-to-selinux-concepts-and-management -> zoek op chcon --reference

```bash
restorecon -R /var/www/html
chcon --reference /var/www/html /var/www/html/test.php
```

Als je succesvol bent, zal de PHP pagina correct inladen - maar wel geen connectie kunnen maken met de database server. Error:
PHP met SELinux connections

We kunnen bij connectieproblemen verleid worden tot zoeken naar problemen bij de firewall. Echter, gezien de database service op dezelfde server geïnstalleerd is als de apache HTTP server, verlopen de verbindingen van het ene software programma naar het andere niet via de externe IP-adressen. Die gaat via het localhost adres - ook al worden andere IP-adressen gebruikt. Wie het verkeer monitort met e.g. wireshark of tcpdump (zie het vak "CyberSecurity & Virtualisation"), zou dit kunnen aantonen. Dit valt echter buiten de scope van dit vak "Linux".

Long story short: SELinux laat een HTTP daemon niet standaard toe om verbindingen met een database server te leggen. Om dit toch toe te laten, moet een boolean waarde verander worden. De stappen hiervoor vind je op https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/security-enhanced_linux/sect-security-enhanced_linux-booleans-configuring_booleans

```bash
getsebool httpd_can_network_connect_db
sudo setsebool httpd_can_network_connect_db on
```

# 6. Scripting "103" - opgaves
Werk volgende scriptoefeningen uit (zonder unit tests). Ze gaan in op testen en voorwaarden inbouwen in je script voordat je naar verdere stappen gaat.

Dit script zal een bestand kopiëren. Bron en doel worden als argumenten meegegeven. Test of het doelbestand bestaat. Indien wel, wordt het script afgebroken. (Opm. geen unit tests voor deze oefening)

```bash
#!/bin/bash

set -o nounset
set -o errexit
set -o pipefail

if [ ${#} -eq 0 ]; then
        echo "Geen vars meegegeven!"
        exit 1
fi


bestand=${1}
doel=${2}
doelbestand=${doel}${bestand}

if [ ! -f ${bestand} ]; then
        echo 'Bestand niet gevonden!'
        exit 1
fi
```
Sorteer de inhoud van een bestand (arg1) en toon de laatste regels (aantal regels = arg2). Indien argument 1 ontbreekt, melding geven en afbreken. Indien argument 2 ontbreekt neemt men 20 als default waarde. Om te testen maak je een bestand aan met alle letters van het alfabet, in de volgorde van je toetsenbord. (Opm. geen unit tests voor deze oefening)

```bash                                
#!/bin/bash

bestand=${1}
aantalRegels=${2}

if [ $# -eq 0 ]; then
        echo "Niks meegegeven!"
        exit 1
elif [ ! -e ${1} ]; then
        echo "Het bestand bestaat niet!"
        exit 1
elif [ -e ${2} ]; then
        sort "${bestand}" | tail -"${aantalRegels}"
else
        sort "${bestand}" | tail -20
fi
```

Dit script moet testen of het opgegeven bestand (arg1) bestaat en uitvoerbaar is. Indien het niet uitvoerbaar is, moet het uitvoerbaar gemaakt worden.

```bash
#!/bin/bash

bestand=${1}

if [ ! -e "${bestand}" ]; then
        echo "Dit bestand bestaat niet!"
        exit 1
else
        if [ -x "${bestand}" ]; then
                echo 'Dit bestand is executable'
                exit
        else
                echo 'Dit bestand is niet executable!'
                chmod +x "${bestand}"

                if [ -x "${bestand}" ]; then
                        echo 'Geslaagd!'
                else
                        echo 'Niet geslaagd :('
                fi
        fi
fi
```

Dit script maakt gebruik van het cal (kalender commando). De gebruiker wordt verplicht om de drie eerste letters van de maand (jan-feb-maa-apr-mei-jun-jul-aug-sep-okt-nov-dec) in te geven. Vervolgens wordt de maandkalender van die maand weergegeven. Geef foutmelding indien geen correcte maand wordt ingegeven en stop het script. De gebruiker kan ook het jaartal ingeven (niet verplicht). Indien niet ingegeven wordt het huidige jaar gebruikt.

# Webserver Automatiseren

Opdracht
Als je de "web"-VM opstart zal je merken dat deze VM nog grotendeels "leeg" is. Zorg er voor dat dit een volwaardige webserver wordt. Dit proces moet volledig geautomatiseerd gebeuren. Je vult de nodige stappen aan in het script provisioning/web.sh.

Installeer Apache (met ondersteuning voor HTTPS)
Configureer de firewall
Configureer SELinux waar nodig
Installeer een demo PHP-pagina die een query uitvoert op de database en het resultaat toont op de webpagina
 
 ```bash
aantalinterfaces = $(ip a | grep 'inet ' | wc -l)

if [ ${aantalinterfaces} -lt 3 ]; then
    echo 'Error:\t Niet genoeg interfaces.'
    return 1
fi

link_local=127.0.0.1
nat_IP_address=$(ip a | grep 'inet ' | sed -n 2p | tr -s '   ' | cut -d' ' -f3 | cut -d'/' -f1)
local_IP_address=$(ip a | grep 'inet ' | sed -n 3p | tr -s '   ' | cut -d' ' -f3 | cut -d'/' -f1)
DB_IP=192.168.76.4

dnf install httpd php mariadb-server
dnf install php-mysqlnd
dnf install mod_ssl openssl

systemctl start httpd

services=$(firewall-cmd --list-all)

if [ ! grep -q 'http ' ${services} ]; then
    firewall-cmd --add-service=http
fi

if [ ! grep -q 'https' ${services} ]; then
    firewall-cmd --add-service=https
fi

systemctl restart firewalld

restorecon -R /var/www/html
```

# 7. Scripting "201"

Schrijf een script passphrase.sh dat een willekeurige wachtwoordzin genereert zoals gesuggereerd door http://xkcd.com/936/. Gebruik een woordenlijst zoals /usr/share/dict/words (moet je mogelijks installeren). Opties en argumenten:

passphrase.sh [-h|-?|--help]: druk uitleg over het commando af en sluit af met exit-status 0. Eventuele andere opties en argumenten worden genegeerd.
passphrase.sh [N] [WORDS]
N = het aantal woorden in de wachtwoordzin (standaardwaarde = 4)
WORDS = het bestand dat de te gebruiken woordenlijst bevat (standaardwaarde = /usr/share/dict/words)
Sluit af met een passende foutboodschap (op stderr!) en exit-status als:
er twee of meer parameters gegeven werden
WORDS niet bestaat of niet leesbaar is
Tip: met het commando shuf kan je de volgorde van lijnen tekst door elkaar schudden.

```bash
#!/bin/bash
N=4
WORDS="/usr/share/dict/words"

# Functions BEGIN
usage() {
    echo "passphrase.sh [N] [WORDS]"
    echo "N = het aantal woorden in de wachtwoordzin (standaardwaarde = 4)"
    echo "WORDS = het bestand dat de te gebruiken woordenlijst bevat (standaardwaarde = /usr/share/dict/words)"
    exit 0
}

dir_is_valid() {
    if [[ ! -r ${WORDS}  ]]; then
        echo "${WORDS} bestaat niet of kan niet gelezen worden" >&2
        exit 1
    fi
}

generate_passphrase() {
    for word in $(shuf ${WORDS} | head -n ${N}); do
        passphrase="${passphrase} ${word}"
    done

    echo "${passphrase}"
}

handle_passphrase() {
    case $# in
        0) generate_passphrase;;

        1) N="$1"
            generate_passphrase;;

        2) N="$1"
            WORDS="$2"
            dir_is_valid
            generate_passphrase;;

        *) echo "Meer dan 2 argumenten " >&2
            exit 1;;
    esac
}
# Functions END


case "$1" in
    -h | -? | --help) usage;;

    *) handle_passphrase "$@";;
esac
```

Schrijf een script om een backup te maken van de als argument opgegeven directory, meer bepaald een Tar-archief gecomprimeerd met bzip2.

Het archief krijgt als naam DIRECTORY-TIMESTAMP.tar.bzip2 met:
DIRECTORY = de naam van de directory waarvan je een backup maakt
TIMESTAMP = de huidige datum/tijd in het formaat JJJJMMDDUUMM
vb. “student-201312021825.tar.bzip2” voor directory /home/student
Er wordt in dezelfde directory als het archief een log weggeschreven naar een bestand backup-TIMESTAMP.log van de uitvoer (zowel stdout als stderr) van het tar-commando.
Gebruik: backup.sh [OPTIES] [DIR]
Opties en argumenten:
-h|-?|--help: druk uitleg over het commando af en sluit af met exit-status 0. Eventuele andere opties en argumenten worden genegeerd.
-d|--destination DIR: de directory waar de backup naartoe geschreven moet worden. Standaardwaarde is /tmp
DIR de directory waarvan er een backup gemaakt moet worden. Standaardwaarde is de home-directory van de huidige gebruiker.
Sluit af met een passende foutboodschap (op stderr!) en exit-status als:
er teveel argumenten gegeven worden
de directory waarvan een backup gemaakt moet worden niet bestaat
de directory waar de backup naartoe geschreven moet worden niet bestaat

```bash
#!/bin/bash

DOEL="/tmp"

# Functions BEGIN
usage() {
    echo "backup.sh [OPTIES] [DIR]"
    exit 0
}

handle_backup() {
    DATE=$(date +%y%y%m%d%H%M)
    DIRNAME=$(basename ${DIR})
    
    if [ ! -d $DOEL ]; then
        echo "${DOEL} bestaat niet" >&2
        exit 1
    fi

    if [ ! -d $DIR ]; then
        echo "${DIR} bestaat niet" >&2
        exit 1
    fi

    tar cfj "${DOEL}/${DIRNAME}-${DATE}.tar.bzip2" $DIR
}
# Functions END


case "$1" in
    -h | -? | --help) 
        usage;;

    -d | --destination)
        DOEL=$2
        DIR=$3
        handle_backup;;

    *)
        if [ $# -eq 1 ]; then
            DIR=$1
            handle_backup
        else
            usage
        fi
        ;;
esac
```

# Cron and jobs
1. Log in op je database server (zie opdracht automation). Als gebruiker root, maak een cronjob aan die de mariadb service elke dag herstart om 03:03. 

```powershell
vagrant status
vagrant up db
vagrant ssh db
```

```bash
EDITOR=nano crontab -e

in nano:
//
3 3 * * * service mariadb.service restart
//
crontab -l
```

extra:
```bash
Site: https://crontab.guru/
“At 22:00 on every day-of-week from Monday through Friday.”
0 22 * * 1-5 service mariadb.service restart

“At minute 23 past every 2nd hour from 0 through 20.”
23 0-20/2 * * * service mysql restart

“At minute 0 past hour 0 and 12 on day-of-month 1 in every 2nd month.
0 0,12 1 */2 *
```

2. Deze wijziging voerde je uit op de actieve database server. Reflecteer: waar zou je deze cronjob wijziging eigenlijk moeten doorvoeren?
```
In het crontab bestand moet je dit manueel ingeven. Je kan dit ook gewoon commandline doen op de prompt (niet in nano), maar in het configbestand kan je meer in detail gaan.
```


# 8. SSH
In deze labo-oefeningen gaan we dieper in op key-based authentication (wat veiliger is als password based login). De oefening wordt uitgevoerd op de Mint GUI VM enerzijds (genaamd M), en op de twee servers die je voorheen maakte met vagrant (genaamd W en D anderzijds, resp. de webserver en database server). De letter vooraan de opdracht omschrijft wat je waar moet uitvoeren.
Als er gevraagd wordt welk wachtwoord gebruikt wordt, kan het antwoord ook zijn 'geen wachtwoord'.

M: Maak een nieuw RSA sleutelpaar aan met ssh-keygen. Kies een wachtwoord om de (private) sleutel op slot te doen.
Als je de map .ssh bekijkt, welke bestanden zijn er bij gekomen?

```bash
ssh keygen -t rsa
'drie maal enter'

cd /home/osboxes/.ssh

Er zijn drie bestanden bijgekomen. Namelijk id_rsa, id_rsa.pub en known_hosts.
```

M: kopieer de publieke sleutel naar zowel server D als W. 

```bash
scp id_rsa.pub vagrant@192.168.76.3:~/
'yes' en dan wachtwoord (vagrant) ingeven

scp id_rsa.pub vagrant@192.168.76.4:~/
'yes' en dan wachtwoord (vagrant) ingeven
```
  
D: Log in op deze server via SSH (vanaf M). Welk wachtwoord geef je nu in?
Ga naar de map .ssh, en bekijk de inhoud van het bestand authorized_keys. Welke gebruikers hebben al toegang via deze deur?
Append de publieke sleutel van opdracht 2 in dit bestand.
Herhaal voor server W.

```bash
pwd --> /home/vagrant
ls --> enkel id_rsa.pub
ls .ssh --> enkel authorized_keys

cat id_rsa.pub >> ssh/authorized_keys
cat .ssh/authorized_keys --> twee sleutels

Idem op webserver.
```

M: Log opnieuw in via SSH op server D. Er komt een pop-up window. Welk wachtwoord moet je hier ingeven?
Herhaal voor server W.

```
ssh -l vagrant 192.168.76.3
Je moet geen wachtwoord ingeven.

ssh -l vagrant 192.168.76.4
Je moet geen wachtwoord ingeven.
```

M: Voeg je private key toe aan een SSH-agent met ssh-add. Ga na met ssh-add -L dat de key effectief toegevoegd is.
Log opnieuw in via SSH op server D. Welk wachtwoord moet je hier ingeven?

```bash
ssh-add --> 'Identity added /home/osboxes/.ssh/id_rsa (...'
ssh-add -L --> sleutel moet zichtbaar zijn
ssh -l vagrant 192.168.76.3
Geen wachtwoord nodig om in te loggen.
```

M: Log opnieuw in via SSH op server D, maar gebruik nu de optie ssh -A. Bekijk de man-page over wat dit doet.

```bash
ssh -A gaat agent forwarding aanzetten of enablen. Dit is een extra laag authenticatie dat je op jouw systeem gaat plakken.
```

D: Log vervolgens in op W vanaf D - gewoon met SSH (zonder optie -A). Welk wachtwoord heb je nodig?
Log uit (nu zie je normaal terug op D), en bekijjk de actieve keys in de agent op dit systeem.

```bash
MINT --> DB --> WEB via SSH

MINT: 
ssh -l vagrant 192.168.76.3 // MINT --> DB         ++ Hier moet je het wachtwoord van DB ingeven.
ssh -l vagrant 192.168.76.4 // MINT --> DB --> WEB ++ Hier moet je het wachtwoord van de WEB ingeven.
exit                        // MINT --> DB
cat .ssh/authorized_keys    // MINT --> DB
cat .ssh/known_hosts        // MINT --> DB
exit                        // MINT
```

# 9. Mount
De labo-oefeningen gaan er van uit dat je de extra VDI hebt toegevoegd aan jouw Linux Mint VM.

```
In virtualbox moet je je VM kiezen. 
Ga dan naar Instellingen (tandwiel) van de VM.
In het tabblad 'Opslag' moet je een extra harde schijf toevoegen. Klik op onderstaand icoontje.

![image](https://user-images.githubusercontent.com/70543493/148177739-67a05f02-7f0b-4e63-b889-6ddaf0f46179.png)

Nu moet je de .vdi kiezen die werd meegegeven op Chamilo of het leerpad. Druk op 'Kiezen'.
Nu zou er, net zoals hierboven, een extra schijf moeten toegevoegd zijn.
```

Gezien je voor quasi alle opdrachten root-rechten nodig hebt, kan je de oefenigen uitvoeren als root. Via sudo -i kan je interactief inloggen als deze administrator van het systeem.

```bash
sudo su -
of 
sudo -i
```

Ga naar de map /mnt. Maak er twee submappen aan, genaamd booty en kernel.
Mount /dev/sdb1 op zowel de map boot enkernel.
Kopieer het bestand /boot/vmlinuz naar de map /mnt/booty. 
Is het bestand nu ook aanwezig in de map /mnt/kernel? Leg uit.

```bash
cd /mnt

mkdir booty
mkdir kernel
of
sudo mkdir {booty, kernel}
ls /mnt              --> twee mappen

sudo mount /dev/sdb1 /mnt/booty
sudo mount /dev/sdb1 /mnt/kernel

cp /boot/vmlinuz /mnt/booty

Het bestand is beschikbaar op zowel de map /booty als de map /kernel.
```

Hermount /dev/sdb1 read-only op kernel (opties -o remount,bind,ro). Verbreek de verbinding met de map /mnt/booty. 

```bash
hermounten:
remount /dev/sdb1 /mnt/kernel -o remount,bind,ro

verbreek de verbinding:
umount /mnt/booty
```

Probeer nu in /mnt/kernel het bestand dat je kopieerde te verwijderen. Waarom lukt dit niet?
Installeer de tool gparted op jouw systeem. Bekijk (grafisch) welke ruimte er nog vrij is op deze harde schijf. 

```bash
rm /mnt/kernel/vmlinuz
error: 'can't remove ...: Read-Only File'

apt-install gparted

gparted

![image](https://user-images.githubusercontent.com/70543493/148184267-a39a3f9d-a906-4290-a62e-1a1b34c01297.png)

```

Kan je nog een primaire partitie aanmaken?

```bash
Ja, maar enkel als je minstens één extended partitie hebt. Anders lukt het niet meer om er eentje toe te voegen.
```

Kan je nog een logische partitie aanmaken?

```bash
Er zijn hoogstens vijf logische partities mogelijk. 
```

Sluit de grafische tool, en maak een nieuwe partitie aan die de volledige vrije ruimte gebruikt met fdisk. Welk naam krijgt deze block device? Hoeveel opslagruimte is er in deze partitie?

```bash
fdisk -l

fdisk /dev/sdb

command (m for help): n

partition type: p

select: 1

last sector: 4194302

created new partition

command (m for help): w --> belangrijk dat je dit doet na het aanmaken vd partitie
```


Formateer deze partitie met NTFS.

```bash
sudo mkfs -t ntfs /dev/sdb1 --> meegegeven partitie formatteren
lsblk -f                    --> is de partitie geformatteerd?
```

Bewerkt /etc/fstab. Voeg een lijn toe die deze nieuwe partitie (met ntfs) mount op de map /mnt/kernel. 

```bash
UUID ophalen:

```

Test dit uit door gewoon mount /dev/sdb7 (zonder doelmap) uit te voeren. De aanvullende info komt uit /etc/fstab!
Zoek de UUID van de partitie /dev/sdb3. Voeg een entry toe in /etc/fstab, die deze partitie mount op /mnt/booty.
Test dit uit door mount /mnt/booty (zonder device) uit te voeren.
Bekijk de inhoud van de map /mnt/kernel. Waar is het bestand heen? Bekijk je mount tabel (filter op sdb), en verklaar.


# 11. Automatisering DNS-server installatie
We gaan verder op het labo van hoofdstuk 6, over het automatiseren van een serverinstallatie. Je hebt dus ook je Github-repository voor dit automatiseringslabo nodig, we gaan hier zaken aan toevoegen.

Op dit moment heb je een opstelling met verschillende VMs die aangesloten zijn op de VirtualBox "intnet" interface:

Linux Mint (sinds het begin van het semester!)
web - Vagrant VM, opgezet in labo 6
db - Vagrant VM, opgezet in labo 6
Om aan het labo te beginnen moet de Linux-Mint VM opgestart zijn. VMs web en db zijn voorlopig nog niet nodig.

We gaan wel een nieuwe VM aanmaken met de naam "srv". Daarvoor moet je eerst in vagrant-hosts.yml volgende code toevoegen:

- name: srv
  ip: 192.168.76.254
  intnet: true
  
 
In de directory provisioning/ moet je ook een script met de naam srv.sh toevoegen. Kopieer eventueel het script web.sh en verwijder alle code die te maken heeft met het installeren van Apache. Je kan ook in je Github repo de originele versie van web.sh opzoeken en die opslaan als srv.sh.

Start tenslotte de VM op met vagrant up srv.

Iteratie 1: caching name server
Om te beginnen installeren we BIND als een caching name server, d.w.z. BIND heeft zelf nog geen zonebestanden met resource records, maar gaat alle queries doorsturen naar een andere DNS server.

Installeer BIND
Zorg er voor dat BIND naar alle interfaces luistert en dat alle hosts DNS queries mogen sturen
Vergeet de configuratie van de firewall niet!
Start de service op
Verwerk de voorgaande stappen in het srv.sh-script, zodat je deze VM kan reconstrueren!

```bash
dnf install -y bind
dnf install -y dhcp

log "luister op alle interfaces + zorg dat iedereen toegang heeft"
sed -i 's/127.0.0.1/any/' /etc/named.conf 
sed -i 's/localhost/any/' /etc/named.conf

log "recursion uitschakelen + zone maken via reverse/normal lookup zone
sed -i 's/recursion yes/recursion no/' /etc/named.conf

log "config file wijzigen"
cp "${PROVISIONING_SCRIPTS}/files/${HOSTNAME}/named.conf" /etc
cp "${PROVISIONING_SCRIPTS}/files/${HOSTNAME}/dhcpd.conf" /etc/dhcp/

log "laat poort 53 toe"
firewall-cmd --permanent --add-port 53/udp

log "reloading firewall"
firewall-cmd --reload

log "start BIND"
systemctl enable --now named

log "start dhcp"
systemctl enable --now dhcpd
```

Voer de troubleshooting-taken voor de transportlaag uit: draait de service? Op welke poort(en)? Is de firewall correct geconfigureerd?
Zet de query log aan voor de DNS service
Open een aparte terminal met het systeemlog voor de BIND service (journalctl -f ...)
Voer een DNS query uit vanop de server zelf (stuur de query naar localhost)
Voer een DNS query uit vanaf de Linux Mint-VM
Krijg je antwoord op je DNS queries? Wat zie je in de logs?

Iteratie 2: authoritative name server
Gebruik het voorbeeld in de slides als basis om srv te configureren als een authoritative name server voor domein linux.lan met volgende hosts:

db - 192.168.76.3
web - 192.168.76.4, met alias www
srv - 192.168.76.254, met alias ns
mail - 192.168.76.10, met aliassen imap en smtp
dit is een fictieve host, we gaan deze niet implementeren!
Schrijf een zonebestand voor de forward zone (reverse lookup komt later) en pas het hoofdconfiguratiebestand aan. Controleer telkens de syntax van zowel je zonebestand als het hoofdconfiguratiebestand!



Verwerk ook deze stappen in het installatiescript srv.sh Let er op dat als je bestanden kopieert naar de VM, deze de juiste permissies en eigendomsrechten hebben. Voor BIND is dit belangrijk! Kijk zelf na welke permissies de configuratiebestanden moeten hebben en pas deze toe op eventuele nieuwe bestanden die je naar de VM kopieert.

Test het resultaat! Doe een DNS query voor bv. "www.linux.lan". Krijg je zowel het CNAME als het A-record terug? Kan je de mailserver van het domein opvragen?  Werkt een DNS query voor Google nog?

Iteratie 3: reverse lookup, authoritative only
Schrijf vervolgens een reverse lookup zone. Wat zal de naam voor deze zone moeten worden? Pas ook het hoofdconfiguratiescript aan om deze zone te laden. Zet recursie nu uit (optie recursion). Zorg ook hier dat dit geautomatiseerd kan gebeuren!

Controleer opnieuw het resultaat: voer een "reverse lookup" uit met een van de IP-adressen van de hosts in linux.lan. Werkt een (forward) lookup voor google.com nog?

Installatie DHCP
We hadden in labo 3 handmatig een DHCP-server geïnstalleerd op een AlmaLinux-VM. In dit labo gaan we deze functionaliteit toevoegen aan srv. Haal je labo-nota's van hoofdstuk 3 er bij en automatiseer de installatie van DHCP. Bij de subnet-declaratie geef je volgende zaken mee:

de range blijft zoals in labo 3 voorzien: 192.168.76.100-150
geef de domeinnaam mee aan clients
geef clients ook een DNS-server, nl. het IP-adres van srv
Als je de AlmaLinux-VM nog in gebruik hebt, sluit deze dan nu af. Voer het provisioning-script voor srv uit en verifieer dat de DHCP-service actief is.

Als je in de Linux Mint-VM een vast IP had ingesteld op de interface aangesloten op het intnet netwerk, verwijder dat dan en vraag opnieuw een IP-adres via DHCP. Zet de netwerkinterface uit en opnieuw aan. Controleer je nieuwe IP-adres en of er een DNS-server is ingesteld voor deze interface (resolvectl status).

Probeer opnieuw DNS-queries uit, maar zonder een DNS-server te specifiëren. Normaal zou nu zowel een query voor google.com (via NAT-interface) als www.linux.lan (via intnet-interface) moeten werken!

Probeer tenslotte in een webbrowser de website "www.linux.lan" te openen. Dit zou moeten werken als je de VMs db en web hebt opgestart.

Afwerken opstelling
Controleer ten slotte nog eens of alle nodige code in de provisioning-scripts aanwezig is. Voer vagrant destroy uit en vagrant up. De 3 VMs zouden moeten opstarten, foutloos hun provisioning-script uitvoeren en de opstelling zou zonder verdere manuele handelingen moeten werken zoals ervoor!

# Script Examen

```bash
#! /bin/bash

# Stop het script bij een onbestaande variabele
set -o nounset
set -o errexit
set -o pipefail

### Algemene variabelen wrden eerst gedefinieerd
# De map waarin je op zoek gaat naar het opgegeven type bestanden
SEARCH_DIR=${1}
# De map waar je de documenten gaat opslaan
BACKUP_TEMP_DIR=${2}
BACKUP_DIR=/var/www/backups


### --- functions ---

# installeer de webserver, ook al zou de service al geïnstalleerd zijn. 
# Gebruik idempotente commando's waar mogelijk.
function install_nginx {
  # Ga na of de map voor de web-inhoud bestaat. Indien niet, maak ze aan
  
  
  if [ -d $BACKUP_DIR ] ;
  then
    mkdir ~/"$BACKUP_DIR"
  fi

  # Installeer de webserver software 
  dnf update
  dnf install nginx 2>&1 /dev/null


  # Pas de configuratie van de webserver aan
  #tr '/usr/share/nginx/html' '/var/www/backups'
  sed -i 's|/usr/share/nginx/html|/var/www/backups|g'

  # Herstart de service
  systemctl restart nginx

  # Firewall ... 
  firewall-cmd add-service=http
  firewall-cmd add-service=https

  firewall-cmd add-port=80
  systemctl restart firewalld
}

# kopieer de symbolisch gelinkte bestanden van de zoekmap naar de backupmap, inclusief indexbestand
function copy_symlink_files {
  local WorkDIR=$1
  local DestDIR=$2

  touch indexfile
  
  # Hint: werk met find en schrijf naar een tijdelijk bestand pamd_index.txt




  # Mapje leegmaken als het bestaat
  if [ ! -d ~/"$BACKUP_DIR" ] ; 
  then
    for file in "$BACKUP_DIR"/*
    do
      rm file
    done

    rm $BACKUP_DIR/*
  fi

  #  kopieer alle bestanden uit het indexbestand met een loop
  while read -r bestand
  do
    cp "$bestand" "$BACKUP_DIR"/
	done < $indexfile # Hier kan je het tijdelijk bestand inlezen in een loop
}

function rename_mtaMTA {
  # Zorg er voor dat er _geen_ output is van deze functie!
  for file in $BACKUP_DIR
  do
    rename 's/^mta-/MTA-/g'
  done
}

function readonly_permissions {
  for file in "$BACKUP_DIR"/*
  do
    #chmod a-rwx $file
    #chmod o+r $file
    chmod 400 $file
  done
}

function create_tarball {
  local WorkDIR=$1
  local FileName=$2

  tar="alternatives_$(date '+%Y-%m-%d').tar.gz" $WorkDIR
  
  # maak de tarbal aan
  tar -czvf $tar $BACKUP_DIR

  # kopieer de tarball naar de doelmap
  cp $tar $WorkDIR

  # geef de inhoud van de tarbal weer
  tar -tvf $tar
}


### --- main script ---
### Voer de opeenvolgende taken uit

# installeer nginx, ook al is het reeds geïnstalleerd. 
install_nginx

# geef de datum weer van vandaag, gebruik deze globale variabele
DATUM=$(date)
printf "Vandaag is het %d/%m/%y" "$DATUM"

# leegmaken doelmap

copy_symlink_files ...

rename_mtaMTA ...

readonly_permissions ... 

create_tarball ...

# Einde script

```
