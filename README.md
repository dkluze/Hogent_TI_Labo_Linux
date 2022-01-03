Gebruikers (en groepen) aanmaken
Het doel van deze opgave is om de opdrachten en de begrippen met betrekking tot gebruikers en groepen te bestuderen, binnen de context van Linux als een multi-user-systeem.

De VM komt met een default user osboxes - dat ben jij (nu toch nog)! Log in als deze gebruiker.
Geef hieronder telkens het commando en de uitvoer
Wat is het commando om de huidige directory op te vragen? In welke map bevind je jou nu?
Wat is het UID van deze gebruiker, wat is de GID?
Maak een nieuwe gebruiker aan met de naam alice, zonder specifieke opties. Werk hiervoor met adduser.
Voorzie een geschikt wachtwoord voor deze gebruiker (en vergeet het niet! Noteer het eventueel in je verslag of in de beschrijving van je VM
Configuratiebestanden voor gebruikersbeheer
In welk bestand kan je de UID, gebruikersnaam, homedirectory, enz. van alle gebruikers terugvinden?
In welk configuratiebestand kan je al de bestaande gebruikersgroepen nakijken, en ook de gebruikers die lid zijn van elke groep?
In welk configuratiebestand vind je de wachtwoorden van alle gebruikers?
Gebruikersgroepen aanmaken
Maak een groep aan met de naam sporten
In welk configuratiebestand vind je het GID van deze groep terug?
Wat zal het GID zijn van de groepen zwemmen en judo als je deze nu onmiddellijk zou aanmaken? Maak ze aan en controleer!
Voeg de gebruiker alice toe aan de groepen sporten en zwemmen
Log in als alice door in een terminal het commando su - alice (let op de spaties!) uit te voeren
Zorg er nu voor dat de groep sporten de primaire groep wordt van alice.
Zorg er voor dat alice uitgelogd is, ga terug naar root
Maak nu de gebruikers in onderstaande tabel aan. Zorg er voor dat ze al meteen bij aanmaken tot de aangegeven groepen behoren. Kies zelf geschikte wachtwoorden voor deze gebruikers en vergeet ze niet (vul eventueel een kolom toe aan de tabel).

Gebruikersnaam	Primaire groep	Secundaire groep
bob	sporten	judo
carol	sporten	zwemmen
daniel	sporten	judo
eva	sporten	zwemmen
Geef de gebruikte commando's om de gebruikers aan te maken en ook om te verifiëren of dit correct gebeurd is:
Verwijder nu de groep alice en controleer.
Gebruiker daniel gaat een tijdje niet meer sporten. Zorg er voor dat deze gebruiker tot nader order geen toegang meer kan hebben tot het systeem (zonder het wachtwoord of de gebruiker te verwijderen!).
Hoe kan je controleren dat daniel inderdaad geen toegang meer heeft tot het systeem? In welk bestand kan dat en hoe zie je daar dan dat het account afgesloten is?
Gebruiker daniel komt terug naar de sportclub. Geef hem opnieuw toegang tot het systeem.
Gebruiker eva stopt helemaal met sporten. Verwijder deze gebruiker, maar doe dit zorgvuldig: zorg er in het bijzonder voor dat ook haar homedirectory verwijderd wordt.
Log aan als de gebruiker carol
Controleer of je in de “thuismap” bent van deze gebruiker. Maak onder deze map een bestand test aan door middel van het commando touch.
Probeer nu als gebruiker carol je te verplaatsen naar de “thuismap” van alice.
Kan je de inhoud van de mappen binnen de thuismap van alice bekijken?
Probeer nu als carol onder de “thuismap” van alice ook een bestand test te maken. Lukt dit? Kan je dit verklaren?
Werken als root
Bekijk de eerste regels van het bestand /etc/shadow. Wat bemerk je bij de gebruiker root?
Log in als de root-gebruiker met het commando sudo -i (let op de spatie!) 
Wat is de home-directory van root?
Wat is het UID van deze gebruiker, wat is de GID?
Stel, nog steeds ingelogd als root, een wachtwoord in voor root. Kies een uniek, nieuw wachtwoord!
Merk je de verandering in /etc/shadow?
Log in als de root-gebruiker met het commando su - (let op de spatie!)
Log uit, en log opnieuw in met sudo su -. Wat wordt er anders?
Jezelf toevoegen en admin maken
Voeg jezelf (met je eigen gekozen loginnaam) toe aan de VM waarin we werken. Gebruik hiervoor useradd -m <jouw loginnaam>
Bewerk /etc/passwd zodat je ook bash gebruikt als default shell.
Bekijk /etc/shadow. Bemerk dat jijzelf als gebruiker nog geen wachtwoord hebt! Stel het in met passwd
Nu wil je jezelf eveneens adminstrator van het systeem maken, zodat je met sudo beheerstaken kan uitvoeren. Aan welke groep voeg je jezelf hiervoor toe?
Hint: https://www.ibm.com/docs/en/cabi/1.1.4?topic=premises-configuring-sudo-access-users
Eigenaars en groepseigenaars veranderen
Maak als root onder /srv/ twee directories aan met de naam groep/verkoop/ en groep/inkoop/. Maak ook 2 groepen aan met de namen verkoop en inkoop. Maak twee gebruikers aan, margriet met primaire groep verkoop en roza, die als primaire groep inkoop heeft. Zorg dat de groepen eigenaar zijn van de overeenkomstige directories en dat margriet eigenaar is van directory verkoop en roza van het directory inkoop. Geef de gebruikte commando’s en controleer:

# ls -l groep/
total 0
drwxr-xr-x. 2 roza     inkoop  6 Sep 22 22:23 inkoop
drwxr-xr-x. 2 margriet verkoop 6 Sep 22 22:23 verkoop
Zorg ervoor dat gebruikers en groepen uit de vorige stap alle permissies hebben. Geef het geschikte commando en controleer.

Voeg een gebruiker, vb alice, toe aan de groep inkoop en verkoop en controleer. Geen van beide groepen zijn primair.

Log in als alice en ga naar de directory verkoop. Laat de gebruiker hier een leeg bestand, bestand1, aanmaken in de directory verkoop. (Indien je hier problemen ondervindt, log dan in via een andere terminalvenster).

Wie is nu eigenaar van bestand1 en wie de groepseigenaar?

Zorg er nu voor dat de groepseigenaar van de directory verkoop automatisch de groepseigenaar wordt van alle bestanden en directories die onder verkoop gemaakt worden. Geef de gebruikte commando’s.

Doe hetzelfde voor de directory inkoop. Geef de gebruikte commando’s.

Verander opnieuw naar gebruiker alice en laat deze gebruiker een leeg bestand2 aanmaken in de directory verkoop. Geef de gebruikte commando’s.

Wie is nu eigenaar van bestand2 en wie groepseigenaar?

Laat nu gebruiker margriet een leeg bestand bestand3 aanmaken. Controleer de eigenaar van bestand3 en de groepseigenaar.

Laat nu gebruiker alice bestand3 verwijderen. Lukt dit?

Zorg er nu voor dat de gebruikers elkaars bestanden niet kunnen verwijderen. Als de gebruiker echter eigenaar is van het betreffende directory mag dit wel. Leg uit hoe je dit doet en controleer. Schrijf je gevolgde procedure op.

Reflectievragen
1. Je kan inloggen op het systeem als root met de volgende twee commando's:

su - 

sudo su -

Beide manieren leveren je root access op, maar de wachtwoorden kunnen verschillend zijn. Wiens wachtwoord gebruik je bij het eerste commando, wiens bij het tweede?

2. Geef drie manieren om een gebruiker (tijdelijk) de toegang tot het systeem te ontzeggen.
