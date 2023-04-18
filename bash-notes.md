Na terminalu moje masine konektujem se na udaljeni server **bandit.labs.overthewire.org** koristeci
ssh program. Koristim komandu:
ssh username@host -p port tj. konkretno **_ssh bandit0@bandit.labs.overthewire.org -p 2220_**
Port parametar se mora proslijediti jer ssh koristi 22 kao default-ni za pristup, sto u zadataku nije slucaj.
Nakon uspjesne konekcije sa **pwd** komadom provjeravam gdje se trenutno nalazim (home/bandit0).
Komandom **ls** izlistam sta se nalazi u tom folderu i nalazim fajl readme potreban za sledeci nivo.
Izvrsavam cat readme komandu koja mi u terminalu ispisuje sadrzaj readmme fajla i kopiram sifru.
Da bih se ulogovala kao bandit1 preko ssh, koji je za udaljenu konekciju izvrsavam naredbu **logout**.
Kako nisam vise povezana sa udaljenim serverom i treba da promjenim korisinika opet izvrsavam ssh komandu ali sa drugim
userom pa komanda glasi: **ssh bandit1@bandit.labs.overthewire.org -p 2220**.
Opet sa pwd komandom provjeravam gdje se nalzaim i vidim da sam u home/bandit1 folderu.
Tu je fajl imena **-** u kojem je lozinka za sledeci nivo. Kako se - koristi za argumente u naredbama naredba cat - ne 
ispisuje mi fajl u terminalu. Stoga koristim **cat < -** za ispis sadrzaja ovog fajla.
Nakon logovanja na server za sledeci nivo nalazim se u /home/bandit3 direktorijumu.
Sa cd inhere pomjeram je korak nize u stablu.
Komandom ls -a mogu da vidim i skrivene fajlove pa i **.hidden** u kom se nalazi password za sledeci nivo, koji ispisujem cat .hidden naredbom.
Nakon laogovanja na server sa **cd inhere** prelazim u inhere direktorijum. 
Sa ls komandom izlistavam sve fajlove, pa zbog - u imenu ispisujem sadrzaj svakog sa **cat < ime**.
Za pronalazenje fajla koji zadovoljava neke uslove koristim komandu **find** sa sledecim argumentima:
    . da trazi u trenutnom direktorijumu
    -type f da pretrazuje fajlove
    -size 1033c da fajl ima 1033 bajta
    ! -executable da trenutni user ne moze da ga pokrene
Za level 7 koristim naredbu **find . -type f -size 33c -readable -user bandit7 -group bandit6**
kojomm kazem da u trenutnom direktorijumu pretrazuje fajlove velicine 33 bajta koji su readable za trenutnog usera,  u vlasnistvu su usera bandit7 i pripadaju grupi bandit 6.
Da bih pronasla liniju sa rjecju 'millionth' korisitm naredbu **grep 'millionth' data.txt**.
Da bih nasla jedinstvenu liniju u fajlu koristim prvo naredbu **sort data.txt** koja mi alfabetski
rastuce sortira input fajl. Potom nad tim rezultatmo sa pipelineom | izvrsavam naredbu uniq -u koja nalazi linije koje su jedinstvene.
Da bih pronasla rijec kojoj prethodi nekoliko znakova = (i kroz testiranje razmak) koristim 
komandu **grep --text -E '={3,}\s?[a-zA-Z0-9]+' data.txt** kojom kazem:
    sa --text argumentom da mi binarni fajl data.txt posmatra kao tekstualni
    sa -E argumentom da mi pattern koji je pod znacima navoda posmatra kao 
    reuglarni izraz a ne kao string literal
    ={3,} kazem da mi trazi u tekstu bar tri znaka jednako jedni za drugim
    \s? moze da slijedi, a ne mora razmak (jer vidim kroz testiranje da ima razmak iako ne pise
    u postavci)
    [a-zA-Z0-9]+ da mi slijedi nesto sto je je human-readable oblika: malo/veliko slovo cifra
    i da se javi bar jednom, ali da uhvati i vise ponavljanja ako ih ima
    [a-zA-Z0-9]{32} da mi uhvati nakon znakova = tacno 32 karaktera koji su human readable
    jer vidim da mi se sifre sastoje od toliko karaktera.
Da dekodiram base64 text u data.txt fajlu koristim komandu **base64 -d data.txt** gdje mi je
-d parametar kojim naglasam da je rijec o dekodiranju.


