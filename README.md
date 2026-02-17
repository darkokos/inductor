# Inductor

## Tema

Tema projektnog zadatka je definicija minimalnog, strogo statički tipiziranog funkcionalnog jezika Inductor, potom implementacija prevodioca i virtuelne mašine koja interpretira bytecode koji rezultuje prevođenjem izvornog koda napisanog u Inductor-u. Implementacija prevodioca i virtuelne mašine će se vršiti u programskom jeziku Rust.

### Jezik

Inspiracija za izbor funkcionalnog jezika je moj interes za konceptima funkcionalnog programiranja, koji su retko obrađivani na drugim predmetima.

Dakle, težio bih ka definiciji čistog funkcionalnog jezika, ali, ako se ovo pokaže kao prepreka, ne bih se klonio uvođenja koncepata koji pripadaju drugim paradigmama programiranja. Ipak su čisti funkcionalni jezici vrlo retki (Haskell je jedini opšte-poznati primer), jer je njihova sintaksa, kao i sama paradigma, suviše "matematička", te često previše komplikovana i nečitljiva za opštu primenu.

Glavni cilj pri definiciji jezika biće da bude Tjuring kompletan. Ovaj jezik će to postići podrškom sledećih stavki:

1. Skladištenje i čitanje podataka - podrška kroz konstante (literale) različitih tipova, i, kasnije, promenljive (let binding)
2. Izvršavanje aritmetičkih operacija - podrška za ustaljene aritmetičke operacije sabiranja, oduzimanja, množenja i deljenja
3. Odlučivanje - podrška kroz uslovno grananje (if-then-else)
4. Sposobnost ponavljanja operacija - podrška kroz rekurzivne pozive funkcija

Tek onda dolazi podrška za funkcionalne koncepte, kao što su:

- Funkcije višeg reda i funkcije kao vrednosti promenljivih i parametara (first-class functions)
- Podrazumevana nepromenljivost podataka
- Nepostojanje iskaza (sve je izraz)
- Rekurzija kao podrška za petlje
- Podudaranje obrazaca (pattern matching)

### Prevodilac

Prevodilac će biti organizovan u standardne komponente:

1. Lexer - vrši tokenizaciju ,to jest, leksičku analizu
2. Parser - vrši sintaktičku analizu
3. Sistem tipova - vrši semantičku analizu
4. Generator koda - emituje kod koji predstavlja cilj prevođenja

Izlaz svake komponente je ujedno i ulaz za sledeću komponentu.

#### Lexer i parser

Lexer i parser će biti implementirani ručno, ili korišćenjem Rust crate-ova za generisanje istih. Primeri takvih crate-ova su:

- [LALRPOP](https://github.com/lalrpop/lalrpop) - LR/LALR parser generator
- [nom](https://github.com/rust-bakery/nom) - parser kombinator
- [Rustemo](https://github.com/igordejanovic/rustemo) - LR/GLR parser generator

#### Sistem tipova

Kako je jezik strogo statički tipiziran, sistem tipova će biti zadužen da dodeli tip svakom izrazu, kao i da proveri validnost svake operacije i poziva funkcije, proverom podudaranja tipova podataka nad kojima su te operacije i funkcije definisane. Sistem tipova će takođe biti zadužen da proveri i iscrpnost podudaranja obrazaca.

Nasuprot prethodnom, sistem tipova neće vršiti zaključivanje tipova, zbog toga što zaključivanje tipova dosta komplikuje implementaciju sistema tipova, a, takođe, smanjuje jasnoću samog programskog jezika. Dakle, kako je jezik statički tipiziran, za svaku promenljivu i parametar će tip morati biti eksplicitno naveden u izvornom kodu.

#### Generisanje koda

Izlaz generatora koda, kao i celog prevodioca, biće niz instrukcija bytecode-a.

Skup instrukcija koji čini ovaj bytecode biće definisan od strane mene, i njegov cilj će biti da bude minimalan, to jest, da bude dovoljan da se njime mogu predstaviti svi koncepti koje Inductor podržava, bez potrebe da bude veći.

Cilj prevođenja u takav bytecode je da se programi napisani u Inductor-u mogu izvršiti na procesoru, bez komplikacija koje donose prevođenje za različite platforme, ili prevođenje za backend kao što je LLVM.

#### Interpretacija rezultujućeg bytecode-a

Kako bi se koncipirani bytecode mogao izvršiti na procesoru, biće potrebno da se implementira interpreter koji razume skup instrukcija koji čini bytecode, i koji izvršava niz instrukcija koje prevodilac emituje.

Tačnije, biće potrebno implementirati virtuelnu mašinu koja će vršiti interpretaciju zasnovanu na steku. Čitanjem instrukcija bytecode-a, virtuelna mašina će upisivati vrednosti na stek, pa ih potom čitati i vršiti operacije nad njima. Instrukcije takođe mogu sadržati uslovne i bezuslovne skokove.

## Napredne tehnike programiranja

Deo projekta koji će biti realizovan u cilju ispunjenja uslova izrade projektnog zadatka predmeta napredne tehnike programiranja je:

1. Definicija jezika
2. Prevodilac
   1. Lexer
   2. Parser
   3. Sistem tipova
   4. Generator koda
3. Interpreter rezultata prevođenja

Koncepti koje bi jezik u ovoj fazi podržavao su:

- Funkcije prvog reda i rekurzija
- Podudaranje obrazaca nad prostim vrednostima (vrednostima prostih tipova)
- Uslovna grananja
- Binarne aritmetičke operacije

## Diplomski rad

Proširenja koja će biti realizovana u sklopu diplomskog rada su:

- Funkcije višeg reda
- Zatvaranja
- Algebarski tipovi podataka (izvedeni tipovi)
- Promenljive (let bindings)
