# Dag 2 - Siste konferansedag

## [Computing Like the Brain][hawkins-slides] (Jeff Hawkins)

Dette var på forhånd kanskje den keynoten jeg var mest spent på. 
Etter å ha lest [On Intelligence](http://www.amazon.com/On-Intelligence-ebook/dp/B003J4VE5Y/) for godt over et år siden og blitt imponert over Hawkins overaskende konkrete modell over hvordan hjernen fungerer, var jeg spent på å se hva [Numenta](https://www.numenta.com/) hadde klart å få til de siste årene. 
Etter å ha sett dette foredraget har jeg inntykk av at de klarer å gjennomføre mye av det de har håpet, og at de virkelig kommer til å oppnå resultater.

Hawkins satte seg i utgangspunktet to mål han ønsket å oppnå:
1. Discover the operationg principles of the neocortex
2. Build systems based on these principles

Disse relativt ambisiøse målene har nå resultert i systemet *Grok*. (For en veldig kjapp oppsummering/sales pitch, se [denne youtube-videoen](http://www.youtube.com/watch?v=1mq8c2Orgso).)

> Grok receives streams of data, creates models of that data stream, and then makes predictions and detects anomalies.
>
> Grok is a cloud-based service that finds complex patterns in data streams and generates actionable predictions in real time. You stream data into Grok; it returns a stream of predictions that can drive decisions and actions.

Hawkins ser hjernen som et prediktivt modelleringssystem. 
Vi har sanser (retina, cochlea, det somatiske systemet) som gir oss en strøm av ekstremt mye data, som hjernen oversetter til handlinger og antagelser om hva som skjer og kommer til å skje.

Hjernen gjør konstant tre ting:
- Former prediksjoner
- Sjekker prediksjonene mot det som faktisk skjedde
- Genererer handlinger

Datamaskiner bruker typisk tette/kompakte representasjoner (for å utnytte minne og lagringskapasitet best mulig).
- Vi bruker få bits, og prøver å utnytte alle kombinasjoner av 1-ere og 0-er.
- Dette fører til at de enkelte bit-ene ikke kan ha noen separat mening, og kun blir betydningsfulle som del av større mønster.

Grok er derimot basert på *sparsome (sparse) distribuerte representasjoner* (SDRer) 
(Se [en video](http://www.youtube.com/watch?v=iNMbsvK8Q8Y&feature=relmfu) for mer detaljer.)
- SDRer bruker flere bits, med typisk kun noen få 1-ere og mest 0-er.
- Hvert enkelt bit innehar en semantisk betydning (som ikke er programmert/definert av bruker, men som læres av systemet)
- Siden hvert bit har betydning, vil datastrukturer som deler bits ha semantisk likhet!
- Kan lagres som invers-indekser over de aktive bit-ene.
  - Kan lagret som samples av aktive bits, siden selv delvis match vil gi semantisk like resultater.
- Ved å se etter mønstre som er del av unionen av observasjoner kan en gjøre mange prediksjoner samtidig.

Hjernen fungerer som et hieraki der sensorinput sendes inn på bunnen og prediksjoner kommer ut på toppen.
Grok er implementert på denne måten, med "lag" av nevroner som er koblet sammen.
Hvert nevron er koblet sammen med et utvalg andre nevroner.
Når disse nevronene aktiveres vil det aktuelle nevronet også aktivere seg (ved neste "time step").

Temporalitet er også viktig. 
Grok gjenkjenner og predikerer ikke bare basert enkelt-inputs, men på strømmer av data.
Dette skjer ved at nevronene ikke bare er koblet til laget under, men til flere lag nedover i hierakiet.

> "Sparse representations is the key to machine intelligence"

Grok er testet på flere praktiske problemer, og virker allerede forholdsvis imponerende.
Les mer på [Numentas hjemmesider](https://www.numenta.com/technology.html#cla).

Hawkins avslutter med noen spådommer om hva som kommer til å skje videre innen området *intelligente maskiner*.

- Mer teori: Idéer om hierarki og sensor-motor integrajon kommer til å bli viktige
- Embodyment: Systemer er i dag sky-baserte slik som Grok. I fremtiden blir det mer fokus på å få systemer som er embedded eller distribuerte.
- Hardware: 
  - Mye av det som i dag gjøres i hardware kan i fremtiden gjøres enormt mye raskere og mer effektivt på spesialiert hardware.
  - Minne kan gjøres mer feil-tolerant.
  - Hierakier basert på sparsome datastrukturer med sub-sampling gjør at ikke alt trenger å kobles sammen (hvilket kan gi oss billigere og mindre kompleks hardware).

På spørsmål om Grok er, eller skal gjøres, open source svarer Hawkins at de planlegger å åpne systemet, men at det ikke vil skje helt enda. Som startup er de visst nødt til å forsøke å tjene litt penger først... Idéene systemet bygger på er derimot publiserte, og interesserte henvises til whitepaper på numenta.com.

Konklusjon: *Definitivt verdt å se.*


## [Y Not? - Adventures in Functional Programming][weirich-slides] (Jim Weirich)

Weirich holder et morsomt og underholdene foredrag om et smalt, og som han selv sier "fullstendig unyttig" tema: Y-kombinatoren. (Her gikk ting litt unna, og notatene mine er nok ikke veldig meningsfulle uten å ha sett foredraget, dessverre.)

Innledende spørsmål: Hva er "effectively computable"?

- Alt som kan regnes ut på en turingmaskin, *eller vha λ-kalkulus*!

Lambda kalkulus i et nøtteskall: λx.x

> "Essentially, λ-calculus is one big macro expansion."

Fixpoints:
- Et fixpoint er et punkt en funksjon konvergerer mot.
- Eksempel: Ta `cos(x) = y` for et hvilket som helst `x`. Regn så `cos(y)` og gjenta mange ganger. Verdien vil konvergere mot 0.739085133. (Dette er punktet der grafen av cosinus-funksjonen møter linjen y = x.)
- Et fixpoint er altså en verdi som, hvis gitt til en funksjon, vil returnere den samme verdien.

Live demo! 

> "This is where it gets kind of scary..."

Weirich starter med en kjapp forklaring av høyere ordens funksjoner, og beskriver så en rekke forskjellige funksjonelle refaktoreringer:
- Tennent correspondence principle: Omslutt koden med en lambda, og kall den umiddelbart.
- Introduce binding: Introduser nye variabler (så lenge de ikke er referert i koden de introduseres til).
- Wrap function: Kall en eksisterende funksjon ved å kalle den fra en ny ytre funksjon.
- Inline defininition: Erstatt funksjonskall med den fulle definisjonen av funksjonen (funksjonkroppen).

Ved hjelp av disse viser han hvordan kode kan reduseres til sammensetninger/komposisjoner av lambda-uttrykk.

Ved hjelp av et factorial-eksempel viser han hvordan også kode med rekursive kall kan reduseres til lambdaer.
Dette gjøres ved å dele koden opp i en "factorial" bit og en "rekursiv" bit.
- "Factorial"-biten er en funksjon som definerer factorial for ett steg. (Kan brukes til å regne ut factorial av 1, men ingenting mer.)
- Den rekursive biten tar inn en slik funksjon, og forbedrer den ved å returnere en funksjon som klarer å løse ytterligere et steg. Dette kalles en *improver-funksjon*.

Vi trenger å finne fixpoint for improver-funksjonen.
- Den endelige factorial-funksjonen er fixpoint for factorial-improveren i eksempelet.

Denne typen fixpoint-combinator kalles *applicative order y-combinator*.

Anbefalt videre lesing: [Programming With Nothing](http://experthuman.com/programming-with-nothing).

Konklusjon: *Et foredrag som er veldig verdt å se for spesielt interesserte :)*


## [Runaway complexity in Big Data... and a plan to stop it][marz-slides] (Nathan Marz)

Marz diskuterer på en overbevisende måte først vanlige kilder til kompleksitet i dagens datasystemer, og deretter sine idéer for hvordan fundamentalt bedre datasystemer kan designes.

Kilder til kompleksitet:
- Mangel på toleranse for menneskelige feil
- Sammenblanding av data og spørringer på data
- Schema gjort på feil måte

Et av de store problemene er at systemene ikke er laget for å være tolerante overfor at mennesker gjør feil.

Det værste som kan skje er at en mister eller gjør data korrupt (eller kan problemet enkelt fikses!)  
- Muterbare systemer har en iboende mangel på toleranse overfor slike problemer nettopp fordi det er mulig å gjøre dette. 
- Konklusjon: Lag ikke-muterbare systemer! Lagring bør gjøres ved å legge til nye data/revisjoner, ikke ved å slette/endre gamle.

Normalisering vs ikke-normalisering: 
- Joins koster for mye! 
- Men redundans er også dumt...

> "Queries are fundamentally complected"

Schemaer:
- Et schema er i praksis en funksjon over data, som forteller hvorvidt dataene er gyldige eller ikke.
- Schema gir strukturell integritet.
  - Uten schema vil vi først oppdage at dataene er blitt korrupte lenge etter at det har skjedd.
- Problemene en i dag forbinder med schemaer er knyttet til implementasjoner og ikke den underliggende idéen/verdien.

Kort oppsummert, problemer med dagens RDBMSer:
- Komplekterer lagring av data og spørring over dataene.
- Muterbar tilstand!

En query er i praksis en funksjon over all data som ligger i databasen.
Å gjøre spørring over all data samtidig blir imidlertid for krevende, et problem vi løser ved å lage forhåndsgenererte views spørringene kan jobbe på.

```
      funksjon           funksjon
         ↓                  ↓    
All data → precomputed view → query
```
          
Problemet med å generere viewene er perfekt for map-reduce.

En ideell database bør
- Bare kreve batch writes (ikke random writes)
- Kan være normalisert (men det trenger ikke batch-viewet være!)

Men batch-views er for trege!
- Innen de er ferdig laget vil de allerede være utdaterte.
- Men de er *eventually consistent* (uten at det gjør dem mer komplekse)

De dataene som ikker er up-to-date i batch-view kan ligge i et *realtime view*.
- Lytter til datastrømmen
- Spørringer gjøres mot unionen av batch view og realtime view

Alt dette bygger opp mot arkitekturen Marz jobber med for Twitter, som de har kalt "Lambda Architecture":

![Lambda-arkitekturen](https://raw.github.com/kvalle/strangeloop2012/master/bilder/marz-lambda-architecture.jpg)

For å oppsummere, fordeler med denne arkitekturen er:
- Alle data er lagret → feil kan rettes ved å rekalkulere views
- Separasjon av spørring og lagring (slik at hver kan optimaliseres for seg)
- Uten muterbar tilstand trenger vi ikke bekymre oss for miste/korruptere gamle data
- Kan lagre data normalisert, og ha effektive spørringer mot denormaliserte views

Konklusjon: *Mange spennende tanker rundt data og fremtidens datasystemer. Anbefales å se!*


## Humanity 2.0 (Matthew Taylor)

Underholdende/inspirerende ikke-teknisk foredrag.
Umulig å lage notater fra, se heller fordraget på video når det kommer ut.

Konklusjon: *TED-tilstander på Strange Loop :)*

## Guess lazily! Making a program guess and guess well (Oleg Kiselyov)

Temaet for foredraget var hvordan en kan formulere tanken "gjett verdien av denne variablen" i kode, og tilsvarende "gjett igjen" hvis det første gjettet ikke skulle vise seg å stemme. 
Og dessuten, hvordan får vi datamaskinen til å gjette på en lur måte, ved å bygge inn kunnskap i prosessen?

Det føltes som om det var en hel del brilliante idéer her, men jeg falt dessverre av et godt stykke før jeg skjønte hva de gikk ut på.

Dette er nok et ofredrag jeg vil prøve meg på om igjen om en god stund, når jeg har lest meg opp litt på forhånd.

[Lenke til abstract](https://thestrangeloop.com/sessions/guess-lazily-making-a-program-guess-and-guess-well) for den som vil vite mer om hva foredraget gikk ut på.

Konklusjon: *Se denne hvis du er interessert i probabilistisk/ikke-deterministisk programmering, ikke er redd for å lese en hel del OCaml-kode, og føler deg relativt smart en kveld.*


## Wolfram's data analysis platform (Taliesin Beynon)

Foredrag om Mathematica og demonstrasjoner av [Wolfram|Alpha Pro](http://www.wolframalpha.com/pro/).

Mathematica:
- Editor, språk, programming environment
- Symbolic functional language (pattern based)
- Dynamisk interaktivt environment
- Mange innebygde algoritmer
- Grafikkbibliotek
- Virker som et svært kraftig verktøy for forskere som trenger å resonere rundt data/algoritmer

Bruker Mathematica som presentasjonsverktøy
- Innhold som en "arbeidsbok" i Mathematica
- Tekst, kode, REPL -- side ved side

Formål med Wolfram|Alpha: "Make all human knowledge computable"

Demo W|A Pro: Personlige data fra Facebook
- Presenteres med (nesten skremmende) mye data som kan hentes ut fra FB-profiler
- Stats, timeline-data, nettverksdata over venner, etc

Demo W|A Pro: Laste opp egne datasett
- Kan laste opp egne datasett (foreløpig kun i form av CSV-filer)
- WA cruncher og manipulerer (vasker) dataene slik at de blir enklere å trekke ut informasjon fra
- Trekker deretter ut og viser så mye informasjon som mulig
  - Stats, diagrammer, etc
  - Ganske imponerende til å være automatiske fremstillinger av dataene

Konklusjon: *Helt greit foredrag, men antagelig ikke verdt å bruke på på video.*

## [Expressing abstraction - Abstracting expression][bini-slides] (Ola Bini)

Bini tar for seg forholdet mellom uttrykkbarhet og abstraksjon, og stiller spørsmålet "Why are new languages still being created? Is it worth choosing languages strategically/Does language actually matter?".

Forholdsvis abstrakt (no pun intended) og lite teknisk, med mange quotes og orddefinisjoner. 
Uten noen endelige konklusjoner eller løsninger, men med en del iteressante tanker underveis.

Ble litt langtekkelig etter en stund. Kunne kanskje fungert langt bedre som en lyntale?

Konklusjon: *Tidvis interessant, men ikke noe jeg ville brukt tid på å se på video.*

## [Project Lambda in Java 8][smith-slides] (Daniel Smith)

Interessant foredrag av en av språkdesignerne for Java hos Oracle.
Handlet dels om hva som kommer i Java 8, og dels om hvordan de tenker og går frem for å få implementert det.

Om fokus for utviklingen av Java som språk:
- Adapting to change
- Righting what's wrong
- Maintaining compatability
- Preserving the core

*Project Lambda: Function values in Java* -- Hva er nytt?
- Lambda expressions
- Variable capture (med inferert (implisitt) `final`)
- Function Types -- funksjonelle interface
- Target typing (utvidet fra Java 7)
  - Infererer type fra venstresiden av assignment
- Metodereferanser: referer til metoder som om de var funksjoner
  - Eks: Arrays::asList, File::new
  - Target type provides argument types
- Default metoder: Kode i interfaces som default implementasjon
  - Fører i praksis til multiple inheritance!

Konklusjon: *Spennende foredrag for alle som er interessert i hva som kommer i Java 8.*

## The State of JavaScript (Brendan Eich)

Underholdende foredrag om JavaScript som blant annet omfattet følgende:

- Litt om de historiske røttene. 
- En del om hva som er bra (og ikke så bra) med JS i dag.
  - Inkludert utdrag av [WAT](https://www.destroyallsoftware.com/talks/wat)
- Trend: Språk som kompilerer til JS ← JS med et nytt liv som felles platform i større grad enn å bare være et språk.
- Kompilering av eksisterende (ikke-web) språk til JS.
  - [Emscripten](https://github.com/kripken/emscripten): LLVM-til-JS kompilator
- Spill på web, med JS og WebGL
  - Livespilling av Quake ++
- Mye om hva som kommer i ECMAScript 6!

Konklusjon: *Bør sees av alle med selv den minste interesse for JS.*


[hawkins-slides]: https://github.com/strangeloop/strangeloop2012/blob/master/slides/sessions/Hawkins-ComputingLikeTheBrain.pdf?raw=true
[weirich-slides]: https://github.com/strangeloop/strangeloop2012/blob/master/slides/sessions/Weirich-YNot.pdf?raw=true
[marz-slides]: https://github.com/strangeloop/strangeloop2012/blob/master/slides/sessions/Marz-RunawayComplexityBigData.pdf?raw=true
[bini-slides]: https://github.com/strangeloop/strangeloop2012/blob/master/slides/sessions/Bini-ExpressingAbstractionAbstractingExpression.pdf?raw=true
[smith-slides]: https://github.com/strangeloop/strangeloop2012/blob/master/slides/sessions/Smith-ProjectLambda%28notes%29.pdf?raw=true