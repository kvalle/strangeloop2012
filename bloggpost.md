Erfaringer fra Strange Loop 2012
================================

I slutten av september gikk den fjerde årlige konferansen [Strange Loop](https://thestrangeloop.com/) av stabelen i St. Louis, Missouri. 
Strange Loop er en leverandøruavhengig konferanse med fokus på nye og voksende teknologier innenfor IT-verdenen.
Presentasjonene spenner over temaer som programmeringspråk, alternative databaser, distribuerte systemer, mobil og web, og mer, og omfatter en blanding av både teknologier som alt er tatt i bruk i industrien og innovative idéer som fortsatt hører hjemme i akademia.

Med foredrag som [We Really Don't Know How To Compute!][sussman-compute] (Gerald J. Sussman) og [Simple Made Easy][hickey-simple] (Rich Hickey) blant fjorårets keynotes hadde vi naturligvis høye forventninger i forkant av konferansen.

Dette er en erfaringsrapport og oppsummering fra BEKKs tre utsendte.
Hva hørte vi om? Hva lærte vi? 
Hvilke foredrag var kulest? 
Og hva kan være viktig å ta med seg videre?

![Hovedscenen på Peabody Opera House](https://raw.github.com/kvalle/strangeloop2012/master/bilder/peabody-opera.jpg)

Overordnede trender
-------------------

Enkelte temaer ble nevnt igjen og igjen i forskjellige foredrag.
Dette er trendene vi føler krystaliserte seg tydeligst på årets konferanse.

### Tema 1: Re-tenking av databasen 

Et av temaene under konferansen var idéen om at dagens databasesystemer er altfor dårlige.
De to viktigste foredragene under dette temaet var Nathan Marz med [Runaway complexity in Big Data... and a plan to stop it][marz-abstract] og Rich Hickey sitt [The Database as a Value][hickey-abstract].
Utgangspunktet for disse foredragene oppsummeres forholdsvis godt i dette svært provokative sitatet fra Marz:

> "The relational database will be a footnote in history.
> Not because of SQL, restrictive schemas, or scalability issues.
> But because of *fundamental flaws* in the RDBMS approach to managing data."

Løsningene Marz presenterer ligger på det konseptuelle planet, som en rekke idéer han har samlet i det han kaller *The Lambda Architecture*.
Rich Hickey presenterte derimot en mer håndfast løsning, i form av [Datomic][datomic].
Til tross for små forskjeller i fokus og arkitektur, løser begge problemene på påfallende like måter.

Hva er så disse svakhetene med dagens RDBMS-er?
Vel, blant de mange som ble tatt opp, la oss ta en titt på de to viktigste.

**Problem I: Muterbar tilstand**

Det første og viktigste problemet er muterbar tilstand.
Både Marz og Hickey identifiserer dette som hovedkilden til kompleksitet i systemene vi bruker i dag, og begge forslår løsninger som baserer seg på immutability.

Ikke-muterbare systemer er fundamentalt enklere — vi sitter igjen med kun CR i stedet for CRUD, og den eneste skriveoperasjonen som er igjen blir å legge til nye data.

Dette løses ved å introdusere *tid* som et konsept i kontekst av data.
Alle data som lagres blir stemplet med tidspunktet lagringen skjedde.
Nye data kan alltid skrives, men endring og sletting av gamle data forbys.
Oppdateringer gjøres i praksis ved å lagre et nytt faktum med et nyere timestamp.

Slik tilrettelegges det for at spørringer kan serveres de *nåværende* versjoner av data, mens det også er mulig å grave i historiske data hvis ønskelig.
Det er mer til dette enn vi har mulighet til å diskutere her, men kort oppsummert minner det svært om måten versjonskontrollsystemer fungerer.

**Problem II: Sammenblanding av lagring og spørring**

Dette er et spørsmål om normalisering av data.
Vi ønsker oss å ha dataene våre i pene og ryddige normaliserte former, men blir ofte tvunget til å denormalisere for å få akseptabel ytelse på spørringer.

Dette skyldes måten dagens systemer blander sammen to ansvarsområder: lagring av data, og behandling av spørringer på disse dataene.
Løsningen er naturligvis å dele opp ansvarsområdene, slik at hver oppgave kan løses på best mulige måte.

Denne separasjonen av lagring og spørring er også løst på liknende måter i de to systemene.
I Marz sin Lambda Achitecture gjøres spørringer mot såkalte *precomputed views* som lages av dataene.
Disse er det to typer av: batch views og realtime views.
Førstnevnte er, som navnet tilsier, oppdatert av batch-jobber som forsøker å holde viewene up to date.
De nyeste dataene, det som enda ikker er oppdatert i batch views, holdes i realtime views som kontinuerlig oppdateres.
Et liknende skille finnes i Datomics arkitektur, men her er det kun indekser som oppdateres på denne måten.

Ved å dele opp slik kan en eksponere dataene på den måten som er mest effektiv for spørringene, og en sikrer at lesing og skriving av data ikke går i beina på hverandre.
Det gjør også at en ikke er avhengig av en query engine som ligger sammen med lagringstjenesten, og en kan flytte spørremotoren nærmere klientene eller benytte forskjellige spørremotorer og språk i forskjellige kontekster.

![Marzs definisjon av datasystemer](https://raw.github.com/kvalle/strangeloop2012/master/bilder/marz-data-system.jpg)

### Tema 2: Relasjonell programmering

Disse idéene om hvordan databaser bør fungere er tett knyttet opp det neste temaet — relasjonell programmering.
Det var i løpet av konferansen en hel del prat om forskjellige deklarative programmeringspråk, en språkgruppe som setter relasjoner mellom data i høysetet.

Det hele startet første dagen med et foredrag som tok for seg et språk som stammer helt tilbake til slutten av 70-tallet, men som har begynt å komme litt i vinden igjen.
Språket heter [Datalog](), og er et regelbasert spørrespråk for relasjonelle data.
Det er basert på et subset av Prolog, men med noen forbedringer.
Der Prolog er ment som et språk til (forholdsvis) generell bruk, er Datalog rettet mot databehandling.
Noen av forbedringene omfatter at signifikansen på reglenes rekkefølge er betydelig redusert, og at spørringer er garantert å returnere uten å grave seg inn i uendelig rekursjon.

Datalog har ingen enkelt referanseimplementasjon, men omfatter en vid familie av språk.
Et av de nyere tilskuddene til denne gruppen er nettopp Datomic, Rich Hickeys nye databasesystem.
Datomic bruker Datalog for å gjøre spørringer på data på samme måte som SQL typisk brukes i dagens RDBMS-løsninger.
En av de største nyvinningene i Datomic i forhold til ren Datalog er introduksjonen av et grunnleggende konsept om *tid*.
Dette gjør det enkelt å ikke bare resonere rundt fakta, men også hvordan fakta endrer seg over tid.

Et annet språk som er verdt å nevne, men som ikke hører hjemme i Datalog-familien, er [Bandicoot](http://bandilab.org/specification.html).
Dette er et relativt nytt språk som har som fokus å forenkle manipulering og uthenting av relasjonelle data.
Språket brukes til å skrive spørringer, ikke ulikt SQL, ved hjelp av relasjonell algebra og sett-baserte operasjoner.
Forskjellen er at spørringer defineres som funksjoner, og disse kan gjenbrukes i andre spørringer.
Det som kanskje skiller Bandicoot-spørringer mest fra SQL er at alle funksjoner blir tilordnet en tilhørende URL, og språket dermed har innebygget støtte for å bygge REST-API.
For den som er nysgjerrig har Bandicoot også et [interaktivt test-miljø](http://mingle.io) en kan leke seg med.

Et siste språk som absolutt bør nevnes i denne gruppen er [miniKanren](http://kanren.sourceforge.net/#mini).
Språket ble presentert i et intet mindre enn magisk foredrag, der en fikk se hvordan det kan gå når to språknerder virkelig får utfolde seg i Scheme.
miniKanren er en minimal implementasjon av Kanren, et deklarativt logisk programmeringsystem i Scheme med førsteklasses relasjoner.
Noe som skiller dette språket fra de nevnt over er at i Scheme, som andre lisp-dialekter, er programmeringskode også data, noe som kan gi uante muligheter for hva slags spørringer det er mulig å konstruere!


### Tema 3: JS as the runtime of the web

Dette temaet ble sparket i gang allerede i første foredrag under *Emerging Languages Camp* som ble arrangert dagen før konferansen startet for fullt.
Jeremy Ashkenas, skapere av CoffeeScript, pratet om trenden at mange nye språk [transpilerer](http://en.wikipedia.org/wiki/Transpile) til JavaScript.
Med CoffeeScript som case diskuterte han fordelene med å basere seg på en eksisterende runtime, og noen av begrensningene det gir å måtte definere språket utifra den semantikken som allerede finnes i JS.


Lars Bak presenterer mye av arbeidet bak optimaliseringen av V8-motoren og hvordan dette har [pushed the limit of web browsers] (http://www.infoq.com/presentations/Performance-V8-Dart). Dette har igjen ført til at JavaScript har fått en ytelse som gjør det mulig implementere en helt ny klasse web-applikasjoner, men ytelsen i seg selv er ikke godt nok.  Behov for bedre kodestruktur og språk, samt behov for å porte eksisterende applikasjoner har ført til et mangfold av språk som nå transpilerer til JavaScript.



Lars står selv bak Dart, som i og for seg er ment som en utfordrer til JavaScript, men som transpilerer til JavaScript for portabilitet. I tillegg ble en rekke andre språk presentert i løpet av konferansen, som for eksempel [ELM] (http://elm-lang.org/) og [Roy] (http://roy.brianmckenna.org). I tillegg hadde David Nolen en presentasjon av [ClojureScript] (https://github.com/clojure/clojurescript) og utfordringene med å uttrykke Clojure semantikk i JavaScripts termer.





Kanskje enda drøyere er Doppio, en JVM som kjører i JavaScript. Doppio er et forsøk på å bringe språkene som kjører på JVM-en til web-en, og kan kjøre mange eksisterende applikasjoner rett ut av boksen. Flere eksisterende kommandolinje-applikasjoner ble presentert, og for å dra det hele lenger ble Rhino JS motoren demonstrert på Doppio: JavaScript kjørende i Java på Doppio-JVM på JavaScript.

Tanken om å støtte eksisterende applikasjoner i JavaScript ble også tatt opp av Brendan Eich, som blant annet [demonstrerte live spilling] (http://brendaneich.github.com/Strange-Loop-2012/#/33) av [BananaBread](https://developer.mozilla.org/en-US/demos/detail/bananabread), som har tatt Cube 2 sin 3d motor som er skrevet i C++ og transpilert denne til JavaScript.

Eich poengterte nettopp det at JavaScript fremover ikke kun har utviklere, men også kompilatorer, som målgruppe og viser til at ECMAScript 6 offisielle mål er å være et bedre språk for [applications, libraries og code-generators] (http://wiki.ecmascript.org/doku.php?id=harmony:harmony).

> "We had always thought that Java's JVM would be the VM of the web, but it turns out that it's JavaScript. JavaScript's parser does a more efficient job... than the JVM's bytecode verifier."
>
> —Doug Crockford

### Tema 4: "Re-imagining Your Development Environment"

*Med foredrag om Light Table, Catnip, aneditor/anterminal, og Bret Victor's "Taking Off the Blindfold" virker det som om dette også kvalifiserer som et slags tema for konferansen. Jeg fikk dessverre ikke sett noen av disse. Var dere innom nok til å ha en mening, eller bør vi droppe det fra oversikten?*

*Jøran: Jeg tror vi kan ta det med. Dette henger egentlig tett sammen med funksjonell programmering og dynamiske språk som trend; man programmerer ved hjelp av raskest mulig feedback — og REPL over testing. Jeg er jævelig skeptisk, men folk snakket hvertfall mye om det :).*

*Kjetil: Alright! Gidder du skrive litt da, Jøran? (Også må vi kanskje finne en bedre tittel...)*

Personlige høydepunkter
-----------------------

Utover foredrageene som definerte hovedtrender på konfernansen sitter naturligvis hver av oss også igjen med noen favoritter.
Dette er våre personlige høydepunkter.


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

  Det var et foredrag strengt tatt uten relevante faglige meritter for de fleste programmerere, men fortsatt ekstremt underholdende!

  Og, hvis du vil ha en utfordring: Den siste gåten er fortsatt ikke løst.


Avslutningsvis vil jeg bare tipse deg som har lyst til å se på Clojure om [LightTable](http://app.kodowa.com/playground). 
Du kan være i gang med å skrive Clojure om få minutter! 

### Kjetil

Dagen før konferansen startet for fullt, på *preconference day*, kunne en velge mellom å delta på en rekke forskjellige workshops, eller den såkalte [Emerging Languages Bootcamp][elc] (ELC).
Jeg valgte sistnevnte, og tilbrakte en lang dag med svært mange spennende foredrag om alt fra Mozillas nye språk [Rust](http://www.rust-lang.org/) til flere funksjonelle språk som kompilerer til JavaScript.
Selv om ingen av foredragene fra ELC individuelt sett er blant mine absolutte høydepunkter fra konferansen, var det en svært bra dag.

Høydepunkter var derimot de følgende tre foredragene, presentert i en brutalt prioritert rekkefølge:

1. *Relational Programming in miniKanren (Daniel Friedman, William Byrd)*
	
	miniKanren er kort fortalt en forenklet versjon av [KANREN][kanren], et system for logisk [deklarativ programmering][deklarativ-wikipedia] implementert i Scheme.

	Sentralt i foredraget var idéen om at slike programmer som er basert på relasjoner er fleksible nok til å jobbe "baklengs" — ikke bare kan de trekke konklusjoner basert på data, men de kan komme opp med data som underbygger en gitt konklusjon basert på relasjonene.
	Og siden kode er data i Lisp innebærer dette naturligvis at vi også kan generere kode på samme måte!

	Foredraget var en tour de force av eksempler på akkurat dette, og kuliminerte i en [demonstrasjon][quine-kode] av hvordan miniKanren kan brukes til å generere [quines][quine-wikipedia].

	Uten noen tidligere erfaring med Scheme, og med liten kjennskap til en del av konseptene vi ble presentert for skal jeg være den første til innrømme at jeg tidvis hadde problemer med å følge alt som foregikk på tavla. 
	Det hindret likevel ikke dette foredraget i å være et av de mest interessante og desidert morsomste på hele konferansen, ikke minst på grunn av foredragholdernes tydelige entusiasme og nærmest barnlige glede over det de snakket om.

2. *The Database as a Value (Rich Hickey)*
	
	Vi har allerede nevnt dette foredraget over, så jeg skal ikke gjenta innholdet her.
	La oss bare si at jeg tror flere av idéene herifra kommer til å være med å forme hvordan databasesystemer blir implementert og brukt i fremtiden.

3. *Computing Like the Brain (Jeff Hawkins)*

	Med interesse for både hjernen og maskinlæring var jeg jo nødt til å like dette foredraget.
	Jeg hadde tidligere lest om [Jeffs modell av hvordan neocortex fungerer][on-intelligence], og i dette foredraget forklarte han hvordan de i Numenta bygger et imponerende datasystem basert på denne modellen.
	
	Systemet de har bygget, [Grok][what-is-grok], er en tjeneste som overvåker datastrømmer og automatisk bygger og oppdaterer modeller basert på disse.
	Modellene bruker Grok til å detektere avvik og å forutsi fremtidige data i strømmen.

	Groks arkitektur, som er inspirert av cortex, gjør det i stand til å detektere spatial-temporale mønstre på en unik måte.
	Et annet spennende aspekt er hvordan data representeres basert på idéer fra [sparse coding][sparse-code-wikipedia], noe som gir spennende muligheter for representasjon av semantisk mening — og ikke minst automatisk læring av denne!

PS: Skulle du være interessert i mer om de forskjellige foredragene har jeg også delt [notatene jeg skrev i løpet av konferansen][notater-kjetil] på GitHub.


### Johannes
Preconference day benyttet jeg til Hadoop og Scalding workshops. Scalding er et Scala API for å kunne skrive MapReduce-jobber for Hadoop. Foruten å gjøre det mulig å skrive MapReduce-jobber i ordentlig Scala gjør Scalding også det mulig å kjøre og teste jobbene uten å kjøre dem på en full Hadoop cluster. 

Ellers er mine hovedinntrykk av konferansen funksjonell programmering, retenking av databasen, og JavaScript som intermediate language og dets fremtid.

Selv om mange av presentasjonene er litt på siden av det jeg driver med til vanlig, særlig når det kommer til språk, var det veldig inspirerende og nyttig å se andre vinklinger og måter å løse problemer på. Her fikk jeg med meg mye nyttig som jeg kommer til å ta med meg videre, og jeg kommer definitivt til å se mer på funksjonelle språk og forhåpentligvis ta med meg noe av lærdommen tilbake til Java-prosjektene jeg jobber på.

Mine personlige høydepunkter var:

1. *Pushing the Limit of Web Browsers (Lars Bak)* 

	Lars Bak går igjennom sin historikk med optimalisering av VM-er som Suns HotSpot, tar for seg optimaliseringene i JavaScript i V8 for å så gå videre og presentere motivasjonene bak Googles Dart. 

2. *Project Lambda in Java 8 (Daniel Smith)*

	Java som programmeringsspråk har langsomt falt etter inntoget av C# og dynamiske språk. 
	Med Java 8 skal vi endelig få noe av det vi har lengtet etter, som closures og default methods. 
	Daniel Smith introduserer de nye språklige konstruksjonene og forklarer noe av bakgrunnen til hvordan de har blitt. 

3. *RiverTrail - Parallel programming in JavaScript (Stephan Herhut)*

	Applikasjoner utviklet i HTML og JavaScript er i stor grad portable og kan kjøres på et stort antall platformer uavhengig av underliggende software og hardware. 
	JavaScript er et sekvensielt språk og kjører i utgangspunktet på en enkelt tråd og kan ikke måle seg i ytelse med native applikasjoner på kalkulasjonstunge applikasjoner som spill og bildebehandling. 
	Intel, som har mye å tjene på å bryte platformbindingen som finnes på mobile enheter i dag, jobber hardt for å få inn støtte for høynivå parallelisering i JavaScript og har introdusert RiverTrail som nå er i Strawman status hos ECMA og mest sannsynlig blir en del av ECMAScript 8. 

	Stephan Herhut presenterer RiverTrail, og standardforslaget sammen med imponerende demoer.

Presentasjonene
----------
Hvis du ble inspirert av noen av foredragene vi har nevnt må det også nevnes at alt ble filmet og [vil bli tilgjengelig på video](https://thestrangeloop.com/news/strange-loop-2012-video-schedule) utover høsten og vinteren.

Oppsummering
-------------------
Strange Loop var *fett*.

[datomic]: http://www.datomic.com/
[hickey-abstract]: https://thestrangeloop.com/sessions/the-database-as-a-value
[marz-abstract]: https://thestrangeloop.com/sessions/runaway-complexity-in-big-data-and-a-plan-to-stop-it
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