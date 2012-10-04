# Dag 1 - Første konferansedag

## Keynote: In-memory Databases - the Future is now! (Michael Stonebraker)
    
Mer eller mindre en salgspresentasjon for en NewSQL-løsning, VoldDB, som Stonebreaker jobber på.
Lite overbevisende og med (tidvis usaklig) kritikk av NoSQL og "OldSQL".

Ekstremt dårlig valg som keynote, og ikke en lovende start på konferansen.

*Overhodet ikke verdt å se.*


## Monad examples for normal people, in Python and Clojure (Dustin Getz)

Foredraget åpnes innledningsvis med "*Hopefully you guys'll understand this...*"

- Motivasjon: Få koden til å likne mer på business-logikk
- Monads et design pattern for å kunne komponere funksjoner uten å måtte tenke på exceptions eller mulig null-verdier
  - Lar oss uttrykke logikken renere, uten å måtte flette inn try-catch og null-sjekker over alt, og kan derfor gjøre koden mer lesbar

> "Not always a good fit, but it can make the code more expressive and concise in some cases"

- Getz kjører igjennom en rekke eksempler på monads implementert fra bunnen av i lett forståelig Python
  - Det går fort, men heldigvis brukes andre halvdel av foredragt til å gå igjennom på nytt med ytterligere forklaringer rundt de vanskeligste punktene

- En monad, eller et *monadic value*, `m` er i praksis en variabel/verdi `v` satt sammen med noe metadata
- Sentrale funksjoner som trengs for å arbeide med monadene:
  - `unit(v)` tar inn en vanlig verdi og returnerer en monadisk verdi `m` basert på denne.
  - `bind(fn, m)` pakker ut den faktiske verdien `v` fra monaden `m`, kaller funksjonen `fn` på `v`, og setter sammen og returnerer deretter en ny monad `m'`. 

Getz har også lagt ut [både slides og kode](http://www.dustingetz.com/2012/09/24/StrangeLoop2012-monads-for-normal-people-in-python-slides.html) fra foredraget, noe jeg antagelig bør gå over enda en gang for å virkelig forstå alt som foregikk i eksemplene.

Forstår jeg monads fullt ut etter dette foredraget? Nei. Men jeg vet langt mer om hva konseptet handler om og hva de kan brukes til.

*Verdt å se for alle som er interessert i en (bedre) forståelse av hvordan monads fungerer.*

## Software Architecture using ZeroMQ (Pieter Hintjens)

Veldig høytsvevende foredrag om software-arkitektur generelt.
Strengt tatt lite som er direkte knyttet il ØMQ, men når stendene det nevnes får jeg lite ut av eksemplene da foredraget tydeligvis forutsetter at en allerede er kjent med ØMQ fra før av.

Foredraget var strukturert som en rekke "arkitektur one-liners" som han pratet rundt.
Noen eksempler:

- "90% of software is trash"
  - Software som lages er for dårlig, og lever ikke lenge nok.
- "The physics of software is the physics of people"
  - Software arkitektur bør likne mer på måten mennesker samarbeider.
- "Design by removing problems, not adding features"
  - Gjør alltid det enkleste som kan løse et gitt veldefinert problem

Et av de bedre poengene dreide seg om hvordan en velger/designer protokoller delsystemer skal snakke sammen via.
Han deler protokoller mellom det kan kaller *cheap* og *nasty*.

> Cheap for the chatty stuff, Nasty for the real work.

Cheap vil si verbose, enkle protokoller det er lett å parse og enkode.
Nasty er protokoller som er håndkodede for å være effektive og kjappe, men som ikke nødvendigvis hverken er lette å jobbe med, eller enkle å utvde.
Poenget hans er at det er behov for begge typer protokoller, men at kompromiss mellom de to skjeldent blir en god løsning.
Cheap bør brukes til vanlige enkle oppgaver, og nasty protokoller bør lages til de oppgavene der ytelsen virkelig trengs.

*Greit nok, men jeg ville nok ikke brukt tid på å se dette på video.*


## Deconstructing P vs NP (or why I hate sudoku) (Daniel Spiewak)

Dette foredraget var lunch-underholdningen min, og var morsomt men på ingen måte faglig relevant :) 

- Problemets opprinnelse: Brev-korrespondanse melom Kurt Gödel og John von Neuman
- Sudoku er i det generelle tilfellet NP-komplett
  - (Men ikke når en, som i oppgavene en vanligvis løser, er begrenset til oppgaver med én løsning)
- Kjennetegn ved NP-problemer
  - Kombinatorisk løsningsrom
  - "Riktig" valg ofte umulig å avgjøre før på et senere tidspunkt
  - Polynomisk nummer av gyldige tilstander
    - Og løsninger kan derfor verifiseres i polynomisk tid
- Kompleksitet uttrykk som antall transisjoner i en "kjøring" av problemet gjennom en Turingmaskin (TM)
- Tenker oss en ikke-deterministisk TM
  - Maskinen velger "riktig" ved hvert steg
  - For å telle antall transisjoner må en telle alle mulige valg ved hvert usikkre punkt
- "Problemet" med P=NP problemet er at det føles grunnleggende galt, men det er veldig vanskelig å bevise at det ikke stemmer
  - Et bevis krever at en resonerer rundt alle TM det er mulig å konstruere
- Hvis faktisk P = NP vil det *ikke* si at RSA-algoritmen faller sammen, fordi faktorisering ikke beviselig ligger i NP
- Selv om et problem kan løses i polynomisk tid, trenger dette ikke nødvendigvis bety at de er praktisk løsbare
  - Eksponenter kan tross alt bli ganske store...
  - Dette betyr at selv om P=NP trenger det faktisk ikke ha noen stor praktisk betydning!

*Artig foredrag, men lærte strengt tatt ikke noe nytt utover pensum fra AlgDat på NTNU*


## Relational Programming in miniKanren (Daniel Friedman, William Byrd)

Dette var helt klart dagens morsomste foredrag. 
Tror aldri før jeg har hatt det så morsomt på et foredrag der jeg har hatt så store problemer med å forstå hva som foregikk.

Foredraget var, som abstracten lovet, en *virvelvind* av relasjonell programmering i språket miniKanren.

- miniKanren
  - Opprinnelig beskrevet i *The Reasoned Schemer* (MIT Press, 2005)
  - Designet for å være enkelt å modifisere og utvide
  - Kjernen består av kun 3 "former" (run, fresh, conde)
  - Portet til en rekke andre språk

Hele foredraget var en enkeste lang live-code session der Friedman og Byrd presenterte en rekke eksempler på ting det er mulig å gjøre i miniKanren.

- Eksemplene dreide seg rundt bruken av `(eval-expo exp env val)`.
  - `exp` er et uttrykk
  - `env` en context/et miljø med variabel-bindinger
  - `val` en verdi som `exp` evaluerer til i `env`

Ved å fylle inn en eller flere av disse kunne de ikke bare rekne resten. ut `val` basert på resten, men også generere verdier for uttrykket `exp` som evaluerte til `val` under `env`.

De startet med enkle eksempler slik som å vise at `3 + 4 = X` har løsningen `X = 7` og at `3 + X = 7` tilsvarende kan løses som `X = 4`, og gikk videre til å demonstrere stadig mer komplekse uttrykk.

> It's quine time!

En del av det de fikk til lå helt på grensen til magi, og det hele toppet seg med demonstrasjon av hvordan en kunne generere *quines*, altså programmer som evaluerer til seg selv.

Til sist fikk vi også et eksempel på generering av et *twine* par. Altså to programmer `A` og `B` som evaluerer til hverandre. `eval(A) == B` og `eval(B) == A`.

*Absolutt verdt å se. Veldig akademisk, og ekstremt underholdende.*


## ClojureScript: Better Semantics at Low, Low Prices! (David Nolen)

- Igjen: Fordel å transpilere til JS fordi JS kjører over alt og kan brukes på nett
  - "Arms race" mellom Google, Mozilla og Apple har gjort JS veldig kjapt
- ClojureScript er utrykks-orientert (dvs, alt i språket er uttrykk, ikke en mix av uttrykk og statements slik som i JS)
- Fordeler med ClojureScript:
  - Persistente datastrukturer
  - Funksjoner som verdier
  - Protokoller
  - Namespaces
  - REPL koblet opp mot browseren
  - Macroer
- Han prater om bakgrunn og implementsjonsdetaljer for ClojureScript
  - Forklarer hvordan de har tenkt og gått frem for å transpirere en del av Clojures semantikk til (god) JS-kode
  - Mange av forskjellene mellom JS og Clojure -- fleksibilitet en har i Clojure men ikke i JS (og motsatt?) -- kan føre til ineffektiv JS.
    - Men de har åpenbart gjort mye smart for å motvirke dét.
  - Jeg ville nok fått mer ut av denne delen av foredraget hvis jeg kunne mer Clojure...

Demo av `core.logic`! (Implementasjon av miniKanren i Clojure. Ref. forrige foredrag.)

*God presentasjon. Verdt å se for alle med interesse for Clojure og web.*


## Types vs Tests : An Epic Battle? (Amanda Laucher, Paul Snively)

Presentert innledningsvis som en debatt mellom en "type-hatende test-elsker" og en "test-hatende type-elsker".
Viste seg snart å heller være en slags oppsummering av en diskusjon og serie med kode-kataer de to har hatt gående over en periode.

De brukte litt tid innledningsvis på å presentere "ytterpunktene" i debatten, presenterte oppgavene de hadde løst seg igjennom og hvilken tilnærming hver hadde hatt, og oppsummerte underveis og til slutt hovedpunktene de mer eller mindre kunne enes om:

- I små kodebaser gir typesystemet mindre verdi
- Typer skalerer bedre enn tester ettersom kodebasen vokser
- Typer har liten verdi når en prater med ikke-tekniske bruker
- Det vanskeligste er uansett å forstå kravene
- En får skjeldent klart definerte input/output-eksempler som del av kravene (som kan gjøre det vanskeligere å skrive tester?)
- Tester kan være bra for å forme idéer, og kan deretter ofte slettes
- Typer kan spare en for å måtte tenke på noen former for tester
- De fleste moderne språk har ikke sterke nok typesystemer til å gjøre ulovlige tilstander ikke-representerbare
- Tester kan verifisere det typer ikke kan bevise
- Test-kode er også kode, med egen kostnad til vedlikehold og krav til korrekthet
- *Tester* kan sjekke eksempler på "there exists", *typer* kan garantere "for all".

Dette burde vært en lyntale.
Det var en hel del gode poenger, men de brukte for mye tid på å prate om hvordan de hadde forberedt foredraget i forhold til hva de var kommet frem til.

*Litt for treg til at jeg vil anbefale den til andre enn de som er veldig opphengt i typer-vs-tester debatten.*


## The Database as a Value (Rich Hickey)
![Første slide](https://raw.github.com/kvalle/strangeloop2012/master/bilder/hickey-the-database-as-a-value.jpg)

## Pushing the Limits of Web Browsers (Lars Bak)

