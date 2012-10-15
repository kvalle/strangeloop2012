Erfaringer fra Strange Loop 2012
================================

Den 23. til 25. september i år gikk den fjerde årlige [Strange Loop](https://thestrangeloop.com/) konferansen av stabelen i St. Louis, Missouri.

Strange Loop er en leverandøruavhengig konferanse med fokus på nye og voksende teknologier.
Presentasjonene spenner over temaer som programmeringspråk, alternaltive databaser, distribuerte systemer, mobil og web, og mer, og omfatter en blanding av teknologier som alt er tatt i bruk i industrien og innovative idéer som fortsatt hører hjemme i akademia.

Med foredrag som [We Really Don't Know How To Compute!][sussman-compute] (Gerald J. Sussman) og [Simple Made Easy][hickey-simple] (Rich Hickey) blant fjorårets keynotes hadde vi naturligvis høye forventninger i forkant av konferansen.
Dette er en erfaringsrapport og oppsummering fra BEKKs tre utsendte.

![Hovedscenen på Peabody Opera House](https://raw.github.com/kvalle/strangeloop2012/master/bilder/peabody-opera.jpg)

Overordnede temaer/trender
--------------------------

Enkelte temaer ble nevnt igjen og igjen i forskjellige foredrag.
Dette er trendene vi føler krystaliserte seg mest tydelig på årets konferanse.

### Tema 1: Re-tenking av databasen (og relasjonell programmering)

- Rich Hickey: Datomic / the database as a value
- Nathan Marz: The Lambda Architecture / runaway complexity

Dette er også knyttet opp mot foredraget om Datalog og plassen dette har som query-engine i de nye løsningene.
I tillegg til datalog/datomic har vi også foredragene om lisp-variantene av dette, miniKanren og core.logic.

### Tema 2: Transpilering til JS

Språk som transpilerer til JS var definitivt hot.
Det ble presentert flere språk på ELC, i tillegg til Dart, ClojureScript, JVM i JS, etc på selve konferansen.
Selvsagt i tillegg til Eichs avsluttende keynote om JS's fremtid.

### Tema 3: "Re-imagining Your Development Environment"

Med foredrag om Light Table, Catnip, aneditor/anterminal, og Bret Victor's "Taking Off the Blindfold" virker det som om dette også kvalifiserer som et slags tema for konferansen.
Jeg fikk dessverre ikke sett noen av disse.
Var dere innom nok til å ha en mening, eller bør vi droppe det fra oversikten?

Jøran: Jeg tror vi kan ta det med. Dette henger egentlig tett sammen med funksjonell programmering og dynamiske språk som trend; man programmerer ved hjelp av raskest mulig feedback — og REPL over testing. Jeg er jævelig skeptisk, men folk snakket hvertfall mye om det :).

*Annet jeg har glemt å ta med?*

Personlige høydepunkter
-----------------------

### Johannes

*TODO*

### Jøran

Jeg innledet oppholdet med to sesjoner training i JavaScript, Backbone og CoffeeScript. Begge deler falt litt gjennom for min del, da det ble litt i overkant enkle saker. Det er dog likevel hyggelig å få tatt en gjennomgang av ting man kan!

Mitt hovedinntrykk av konferansen er: Språk. Gjennom mine fire år som programmerer har jeg alltid sett programmeringsspråk som verktøy, men her ble jeg eksponert for en kultur der programmeringsspråk former hvordan man tenker på problemer. Jeg har leflet litt med funksjonell programmering tidligere, men jeg ser at jeg har veldig mye å lære.

Og på StrangeLoop2012 var det et språk som gjaldt: Clojure. For en Java-utvikler var det tidvis mye å ta innover seg, men jeg tror det er ekstremt sunt å bli eksponert for en annen kultur innimellom.

Mine høydepunkter inkluderte:

1. *The State Of JavaScript (Brendan Eich)*

  > "Nobody wants to take credit... nobody wants to take the blame. So I take the blame"

  Gøy å høre the "grandfather of JavaScript" snakke om språket som i løpet av ganske få år har vært programmeringsverdens både mest forhatte og elskede språk.

  Brendan innledet med historien om hvordan JavaScript kort fortalt ble designet på 10 dager og sluppet på markedet før han på noen måte følte det var klart — og det plutselig var for sent å endre fordi "you can't break the web!"

  Videre fortalte han om planene videre med ECMAScript 6 — et arbeid som selvsagt er svært møysommelig, da alle endringer helst skal kunne innføres uten at eksisterende kode bryter. 
  Det er interessant å høre hvordan designerne av ECMAScript ser mot blant annet CoffeeScript når de designer JavaScript for fremtiden.

  Til sist fortalte han om de mange prosjektene som foregår med JavaScript som VM nå. 
  Det er helt åpenbart at han er minst like overrasket som alle andre over hvordan JavaScript har utviklet seg til å bli "the runtime of the web" — og hvor godt det fungerer.

2. *The Database as a Value (Rick Hickey)*

  Veldig fin presentasjon! 
  Facinerende og inspirerende å se noen "finne opp" databasen på nytt med utgangspunkt i funksjonell programmering. 
  Rich beskrev en database bestående av immutable datastrukturer som benytter seg av event sourcing, som gir andre løsninger på ACID-problemer enn det vi har sett for oss tidligere.

3. *Famous Unsolved Codes: Kryptos (Elonka Dunin)*

  [Kryptos][kryptos-wikipedia] er et kunstverk hos CIA på Langley. 
  Den består — enkelt fortalt — av fire deler som alle utgjør en kryptografisk gåte. 
  Med bakgrunn i informasjonssikkerhet synes jeg det var uendelig interessant å høre om hvordan folk har jobbet for å løse disse gåtene — og ikke minst hvilken entusiasme folk viser for det!

  Et foredrag strengt tatt uten relevante faglige meritter for de fleste programmerere, men fortsatt ekstremt underholdende!

  Og, hvis du vil ha en utfordring: Den siste gåten er fortsatt ikke løst.


Avslutningsvis vil jeg bare tipse deg som har lyst til å se på Clojure om [LightTable](http://app.kodowa.com/playground). 
Du kan være i gang med å skrive Clojure om få minutter! 

### Kjetil

Dagen før konferansen startet for fullt, på *preconference day*, kunne en velge mellom å delta på en rekke forskjellige workshops, eller den såkalte [Emerging Languages Bootcamp][elc] (ELC).
Jeg valgte sistnevnte, og tilbrakte en lang dag med svært mange spennende foredrag om alt fra Mozillas nye språk [Rust](http://www.rust-lang.org/) til flere funksjonelle språk som kompilerer til JavaScript.
Selv om ingen av foredragene fra ELC individuelt sett er blant mine absolutte høydepunkter fra konferansen, var det en svært bra dag.

Høydepunktene var derimot de følgende tre foredragene, presentert i brutalt prioritert rekkefølge:

1. *Relational Programming in miniKanren (Daniel Friedman, William Byrd)*
	
	miniKanren er kort fortalt en forenklet versjon av [KANREN][kanren], et system for logisk [deklarativ programmering][deklarativ-wikipedia] implementert i Scheme.

	Sentralt i foredraget var idéen om at slike programmer som er basert på relasjoner er fleksible nok til å jobbe "baklengs" — ikke bare kan de trekke konklusjoner basert på data, men de kan komme opp med data som underbygger en gitt konklusjon basert på relasjonene.
	Og siden kode er data i Lisp innebærer dette naturligvis at vi også kan generere kode på samme måte!

	Foredraget var en tour de force av eksempler på akkurat dette, og kuliminerte i en [demonstrasjon][quine-kode] av hvordan miniKanren kan brukes til å generere [quines][quine-wikipedia].

	Uten noen tidligere erfaring med Scheme, og med liten kjennskap til en del av konseptene vi ble presentert for skal jeg være den første til innrømme at jeg tidvis hadde problemer med å følge alt som foregikk på tavla. 
	Det hindret likevel ikke dette foredraget i å være et av de mest interessante og desidert morsomste på hele konferansen, ikke minst på grunn av foredragholdernes tydelige entusiasme og nærmest barnlige glede over det de pratet om.

2. *The Database as a Value (Rich Hickey)*
	
	Vi har allerede nevnt dette foredraget over, så jeg skal ikke gjenta innholdet her.
	La oss bare si at jeg tror flere av idéene herifra kommer til å være med å forme hvordan databasesystemer blir implementert og brukt i fremtiden.

3. *Computing Like the Brain (Jeff Hawkins)*

	Med interesse for både hjernen og maskinlæring var jeg jo nødt til å like dette foredraget.
	Jeg hadde tidligere lest om [Jeffs modell av hvordan neocortex fungerer][on-intelligence], og i dette foredraget forklarte han hvordan de i Numenta bygger et imponerende datasystem basert på denne modellen.
	
	Systemet de har bygget, [Grok][what-is-grok], er en tjeneste som overvåker datastrømmer og automatisk bygger og oppdaterer modeller basert på disse.
	Modellene bruker Grok til å detektere avvik og å forutsi fremtidige data i strømmen.

	Groks arkitektur, som er inspirert av cortex, gjør det i stand til å detektere spatial-temporale mønstre på en unik måte.
	Et annet spennende aspekt er hvordan data representeres basert på idéer fra [sparse coding][sparse-code-wikipedia], noe som gir spennende muligheter for representasjon av semantisk mening.


PS: Skulle du være interessert i mer om de forskjellige foredragene har jeg også delt [notatene jeg skrev iløpet av konferansen][notater-kjetil] på GitHub.


Oppsummering
------------

*TODO*

Det er også verdt å nevne at videoer av alle foredragene [slippes gradvis de kommende månedene](https://thestrangeloop.com/news/strange-loop-2012-video-schedule).


[elc]: http://emerginglangs.com/
[sussman-compute]: http://www.infoq.com/presentations/We-Really-Dont-Know-How-To-Compute
[hickey-simple]: http://www.infoq.com/presentations/Simple-Made-Easy
[kanren]: http://kanren.sourceforge.net/
[notater-kjetil]: https://github.com/kvalle/strangeloop2012/tree/master/notater
[on-intelligence]: http://www.amazon.com/On-Intelligence-ebook/dp/B003J4VE5Y/
[what-is-grok]: https://www.numenta.com/how_grok_works.html
[deklarativ-wikipedia]: http://en.wikipedia.org/wiki/Declarative_programming
[quine-wikipedia]: http://en.wikipedia.org/wiki/Quine_(computing)
[quine-kode]: https://github.com/webyrd/quines#generating-quines
[sparse-code-wikipedia]: http://en.wikipedia.org/wiki/Sparse_coding
[kryptos-wikipedia]: http://en.wikipedia.org/wiki/Kryptos