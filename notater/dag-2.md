# Dag 2 - Siste konferansedag

## Computing Like the Brain (Jeff Hawkins)

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


## Y Not? - Adventures in Functional Programming (Jim Weirich)

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
- Factorial er fixpoint for factorial-improveren i eksempelet.

Denne typen fixpoint-combinator kalles *applicative order y-combinator*.

Anbefalt videre lesing: [Programming With Nothing](http://experthuman.com/programming-with-nothing).

Konklusjon: *Et foredrag som er veldig verdt å se for spesielt interesserte :)*

## Runaway complexity in Big Data... and a plan to stop it (Nathan Marz)
## Humanity 2.0 (Matthew Taylor)
## Guess lazily! Making a program guess and guess well (Oleg Kiselyov)
## Wolfram's data analysis platform (Taliesin Beynon)
## Expressing abstraction - Abstracting expression (Ola Bini)
## Project Lambda in Java 8 (Daniel Smith)
## The State of JavaScript (Brendan Eich)
