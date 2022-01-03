# Gebruikers (en groepen) aanmaken
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
