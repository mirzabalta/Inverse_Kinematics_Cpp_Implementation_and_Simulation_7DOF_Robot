# Optimizacijski pristup i primjena BFGS algoritma u rješavanju problema inverzne kinematike robotskog manipulatora

Ovaj repozitorij sadrži implementaciju rješenja problema inverzne kinematike 7-DOF (7 stepeni slobode kretanja) robotskog manipulatora. **Broyden–Fletcher–Goldfarb–Shanno (BFGS)** optimizacijski algoritam primijenjen je za rješavanje ovog problema. Ovaj algoritam je implementiran u programskom jeziku C++, u obliku **S-funkcije** koja je kao blok inverzne kinematike primijenjena u simulaciji koja je rađena u **Matlab Simulink-u**. Problem inverzne kinematike je tretiran kao problem nelinearne optimizacije bez ograničenja za varijable, čime je omogućena direktna primjena BFGS metode.

## Sadržaj
- [Uvod](#uvod)
- [Algoritam](#algoritam)
- [Ulazi i izlazi algoritma](#ulazi-i-izlazi-algoritma)
- [Datoteke](#datoteke)
- [Simulacija](#simulacija)
- [Doprinos](#doprinos)
- [Licenca](#licenca)

## Uvod

Svaki zadatak koji robot obavlja može se svesti na ispravno pozicioniranje i orijentaciju vrha manipulatora, dok izvršenje zadatka obavljaju aktuatori koji pokreću zglobove pri tome mijenjajući varijable zglobova. **Direktna kinematika** robota izražava ovisnost pozicije i orijentacije vrha manipulatora od varijabli zglobova. **Inverzna kinematika** je postupak određivanja varijabli zglobova robotskog manipulatora na osnovu poznate željene pozicije i orijentacije vrha manipulatora. Problem je vrlo nelinearan, što otežava tradicionalna analitička rješenja. Za **7-DOF** redundantni robotski manipulator rješenje ovog problema nije jedinstveno, što predstavlja dodatni izazov. Ovaj projekt koristi **BFGS optimizacijski pristup**, kako bi iterativno riješio problem inverzne kinematike.

## Algoritam

U inverznoj kinematici, cilj je odrediti zglobne varijable robotskog manipulatora koji rezultiraju željenom pozicijom i orijentacijom vrha manipulatora. Ovaj problem se može postaviti kao optimizacijski problem,  gdje je cilj minimizirati grešku između željene pozicije (i orijentacije)  i stvarne pozicije (i orijentacije) vrha manipulatora.  Ova formulacija omogućava rješavanje problema inverzne kinematike koristeći optimizacijske tehnike poput BFGS algoritma, koji iterativno minimizira grešku i konvergira ka rješenju koje daje konfiguraciju zglobova za željenu poziciju i orijentaciju. U terminima optimizacije greška pozicije i orijentacije se može izraziti kao  funkcija cilja (ili kriterij optimalnosti). BFGS algoritam, kao i algoritam jednodimenzionalnog pretraživana kojeg on koristi, su detaljno opisani u fajlu Documentation.

Funkcija cilja izrazava  mjeru odstupanja trenutne pozicije i orijentacije vrha manipulatora od njegove zeljene pozicije i orijentacije. Funkcija cilja u sebe ukljucuje::

- **Grešku pozicije**: Izražava rastojanje trenutne pozicije vrha manipulatora (funkcija zglobnih varijabli) od željene pozicije.
- **Grešku orijentacije**: Izražava odstupanje trenutne orijentacije vrha manipulatora od željene orijentacije. Greška orijentacije se može izraziti vektorskim dijelom kvaterniona.

Željena pozicija i orijentacija vrha manipulatora se dobivaju na osnovu matrice homogene transformacije, dok se trenutna pozicija i orijentacija dobivaju iz jednadžbe direktne kinematike. **Zglobne varijable** predstavljaju problemske varijable algoritma optimizacije, a vektor zglobnih varijabli je izlaz algoritma inverzne kinematike.

## Ulazi i izlazi algoritma

Ulazi bloka s-funkcije u kojoj je implementiran algoritam inverzne kinematike su:
- **Pose**: Željena pozicija i orijentacija vrha manipulatora, data kao 4x4 matrica homogene transformacije.
- **Init**: Početna vrijednost zglobnih varijabli, koja predstavlja početnu aproksimaciju BFGS algoritma.

Izlazi bloka s-funkcije:
- **Config**: Vektor zglobnih varijabli, koji omogućava postizanje željene pozicije vrha manipulatora.
- **Criterion Value**: Vrijednost funkcije kriterija koja izražava mjeru odstupanja postignute pozicije i orijentacije od željene.

## Datoteke

- **Libraries** sadrži implementirane biblioteke:
  1. `BFGSAlgorithm.cpp`: Implementacija BFGS algoritma i funkcije cilja.
  2. `LineSearch.cpp`: Implementacija algoritma linijskog pretraživanja.
  3. `MatrixLibrary.cpp`: Implementacija operacija s matricama i vektorima.
  
- **Inverse_Kinematics_BFGS_wrapper.cpp**: Definira DH parametre robota i poziva funkcije iz biblioteka.
- **Robot_Params_and_Waypoints_Data.m**: Učitava parametre robota i putne tačke.
- **Robot_IK_Simulation.slx**: Simulacija robota u obliku Simulink blok dijagrama.
- **Documentation.pdf**: Detaljna dokumentacija projekta.
- **trajExampleUtils.slx** i **visualizeRobot.m**: Koriste se za vizualizaciju robota.

## Simulacija

Da biste koristili ovaj projekt, potrebno je na računaru imati instaliran Matlab, verziju R2020a ili verziju noviju od ove verzije. Simulacija je rađena sa KUKA iiwa robotom koji ima 7 stepeni slobode kretanja (7-DOF). Da bi simulirali i demonstrirali rad implementiranog algoritma inverzne kinematike, slijedite sljedeće korake:

1. Instalirati **C++ kompajler** u Matlab-u (komanda: `mex -setup C++`).
2. Klonirati repozitorij.
3. Pokrenuti `Robot_Params_and_Waypoints_Data.m`.
4. Otvoriti `Robot_IK_Simulation.slx` i kompajlirati S-funkciju.
5. Pokrenuti simulaciju.

## Doprinos

Ako želite doprinijeti projektu, slobodno otvorite **issue** ili pošaljite **pull request**. Svi doprinosi su dobrodošli.

## Licenca

Ovaj projekt je licenciran pod **MIT licencom** - pogledajte datoteku [LICENSE](./LICENSE) za detalje.

## Osobine

- **7-DOF robotski manipulator**: Dizajniran za sisteme s visokim stepenom slobode.
- **BFGS optimizacija**: Efikasno rješenje problema inverzne kinematike.
- **Brza konvergencija**: Omogućava brzo i numerički stabilno rješenje.
- **Prilagodljiv**: Jednostavno prilagodljiv različitim manipulatorima.
