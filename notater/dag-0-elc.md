# Dag 0 - Emerging Languages Camp

ELC-dagen besto av en rekke foredrag om forskjellige språk. Noen relativt modne språk og konsepter -- og enkelte som knapt nok var påbegynt enda. Kvaliteten på foredragene var stort sett veldig bra, med kun enklete unntak.

[Emerging Languages Camp](http://emerginglangs.com/) er egentlig en frittstående event som ble arrangert i samarbeid med Strange Loop for første gang. ELC har tidligere to ganger vært arrangert på OSCON.


## Symbiotic Languages: Transpiling into JavaScript (Jeremy Ashkenas)

Jeremy (skaperen av CoffeeScript, Backbone.js ++) snakker om hva som kjennetegner mange av de nye språkene som dukker opp om dagen -- at de kompilerer til andre høynivåspråk som allerede er etablerte (som oftest JS).

- Trend: språk kompileres til andre språk i stedet for å starte fra scratch
- *Transpiled*: kompilere fra et språk til et annet språk med omtrent samme abstraksjonsnivå
- CoffeeScript eksempel på transpilert språk
  - Ble presentert på Emerging Languages Camp for 2 år siden.
- Lett å benytte det underliggende språkets semantikk, vanskelig å definere ny
  - Vanskelig (nytt ift js): negative indexer
  - enkelt (kan bruke js-semantikk): alt som uttrykk, scoping, klasser
- fordel med transpilering: alt er på samme stack; gjør et enklere å få lov av kunde å ta i bruk det nye språket siden det kompilerer "ned" til noe kjent
- Ved å fokusere på å bygge på eksisterende språk kan språkdesignere fokusere på selve språket i stedet for å bygge en ny runtime

*Inneholdt ingen veldig banebrytende idéer, men verdt å se for den som er interessert i tankene bak design av CoffeeScript og liknende språk*


## Bandicoot: code reuse for the relational model (Ostap Cherkashin, Julius Chrobak)

- Bandicoot: språk og runtime for å forbedre grensesnittet mot den relasjonelle modellen
- Use case: Spørringer mot datasett (eksempel: csv-filer)
- Spørringer kan minne om det en er vant med fra SQL, med relasjonell algebra og sett-baserte operasjoner
- Spørringer kan defineres som funksjoner, hvilke kan komponeres
- Funksjoner tildeles URL-er og kan aksesseres som et REST API
- Støtter ACID
- Detaljer: http://bandilab.org/specification.html
- Prøv ut interaktivt test-miljø online: mingle.io
- Ser for meg at jeg kommer til å leke litt med dette, men tror ikke det er modent for å bruke til noen seriøse applikasjoner enda

*Artig presentasjon/språk for alle som er nyskjerrige på sett-basert programmering/datamanipulasjon.*


## Elm: Making the Web Functional (Evan Czaplicki)

- Funksjonelt og typesikkert språk som kompierer til HTML, CSS og JS
- Målsetning: Gjøre programmering (inkludert GUI) enklere
- http://elm-lang.org/
  - Har en interaktiv editor for testing online: http://elm-lang.org/edit/examples/Reactive/Transforms.elm
- Basert på reaktiv programmering som gjør at enddringer propageres ut til GUI
  - Verdier kan ikke endre tilstand, men det finnes såkalte "time-varying values" kalt `signals` som til en hver tid holder snapshot-verdien til bruker-inputs.
  - Signals kan sees på som en strøm av events, og koden oppdateres eller "kjøres på nytt" hver gang noe endres.
  - Eksempler på signals: `Mouse.click`, `Time.every 4`

Skal definitivt se nærmere på dette.

*Vell verdt å se for alle som er interessert i programmering på web eller funksjonell programmering*


## Plan: a new dialect of Lisp (David Kendal)

Plan er en høyst uferdig dialekt av Lisp, og foredraget handlet hovedsaklig om foredragsholders planer om å lage en Lisp med "akkurat passe" abstraksjonsnivå.

*Ikke verdt å se på video*


## Clever, Classless and Free? (Håkan Råberg)

Foredrag om Råbergs erfaringer som har tatt han fra objektorientert programmering i Java, via nye språk som ruby Ruby (og en implementasjon av Rubys Enumerable for Java), og til sist til funksjonell programmering og shen (http://shenlanguage.org/)

Ikke veldig fokusert, men med noen interessante tanker og poenger her og der. Hovedbudskapet var kanskje at det er viktig å utfordre seg selv, og ikke bli for komfortabel der en er.

*Ville ikke brukt tid på å se denne på video*


## The Reemergence of Datalog (Michael Fogus)

- Datalog er et programmeringspråk for å definere deklarative regler og spørringer på data
- Gammel idé, stammer fra 1970-tallet.
- Ikke turing-komplett
- Basert på Prolog
- Forsøker å løse (blant annet) følgende problemer med Prolog:
  - Rekkefølge på statements har signifikans
  - Er ikke garantert å returnere
  - Deklarativ infeksjon: Prolog er ikke deklarativt nok!

### Eksempel:

facts:

	parent(bill,mary).
	parent(mary,john).

relasjoner:

	ancestor(X,Y) :- parent(X,Y).
	ancestor(X,Y) :- parent(X,Z),ancestor(Z,Y).

spørringer: (`x` vil her returneres som john og mary)

	?- ancestor(bill,X).

Datalog er et språk, men ingen implementasjon (på samme linje som SQL). Det finnes flere implementasjoner, slik som Datomic datomic Cascalog.

*Grei innføring til konseptet. Verdt å se om en, som meg, ikke er kjent med Datalog eller noen av implementasjonene fra før.*


## Roy (Brian McKenna)

- Statisk typet funksjonelt språk som er skrevet i og transpilerer til JS
- Fortsatt under utvikling
- Ment for å gjøre utviklere i stand til å skrive tryggere og mer testbar kode på web
- Type-inferens og compile-time type-sjekking
- Signifikant whitespace 
- Dokumentasjon, enkelt test-miljø, og noen eksempler på roy.brianmckenna.org
- Best beskrevet som et kryssprodukt av CoffeeScript og Ocaml eller Haskell
- (Foredraget har dagens hittil største #ithipster-faktor)

*Interessant, selv om språket ikke er fullstendig ferdig enda. Verdt å se på video.*


## Julia: A Fast Dynamic Language For Technical Computing (Michael Homer)

- Problem: Det finnes mange tekniske språk, men ingen general purpose. Løsning: Julia?
  - Med tekniske språk menes språk som Matlab, SciPy, etc.
- Har som mål å oppnå uttrykk-/lesbarheten til dynamiske språk som Ruby og Python med noe som nærmer seg ytelsen til C++ eller Java
- Dynamisk, multiple dispatch, sofistikert parametrisk typesystem
- Syntaxen likner på MatLab
- Rammer/føringer for språket:
  - "Interactive, productive, tangible"
  - Helhetlig typesystem (ingen skille mellom "primitiver" og "objekter")
  - Effektve tall-arrays
  - Matte-operatorer som funksjoner
- Mangler gode grafikk-biblioteker
  - Finnes visstnok integrasjoner med matplotlib, gnuplot, og andre

*Interessant foredrag om en er interessert i slike språk. På video ville jeg nok skipppet rastk igjennom selve foredraget, og kun sett demo-biten for å få et inntrykk av språket.*


# Rust (David Herman)

- Nytt programmeringspråk laget av Mozilla Research
- Skapt for å møte behovet for et godt språk for å implementere browsere
- "Safe, concurrent, fast"
- Ment for å utvikle pålitlige og effektive systemer, designet for å støtte samtidighet og utnytte moderne hardware
- Høyt fokus på isolasjon og minnesikkerhet
- "The love child of C++, Erlang and Haskell"
- Henter ideer til henholdsvis ytelse, concurrency og typer fra de over nevnte
- Har støtte for 3 forskjellige typer pekere, inspirert av C++
- Funksjonalitet/kode kan deles opp i "tasks" som alle har sin egen heap og ikke kan aksessere andre minnet til andre tasks.
  - Fint for å, for eksempel, implementere tabs i en browser
  - Tasks kan kommunisere vha meldinger (åpenbart inpirert av Erlang)
- rust-lang.org
- Virker som Mozillas svar på Go
  - En del mer low-level og mer komplekst, men gir samtidig mer kontroll over minne, etc

*Foredraget er definitivt verdt å se for den som er interessert i språket, men er tidvis forholdsvis teknisk.*



## Elixir: Modern Programming for the Erlang VM (Jose Valim)

- Språk rettet mot webutvikling laget for å eksponere Erlang VM på en god måte, og samtidig forbedre noen av svaketene til Erlang
- Støtter hot code-swap
- Mål:
  - Produktivitet
    - alt er uttrykk + makroer -> DSLer
  - Utvidbarhet gjennom moduler
  - Kompatibilietet med Erlang.
    - Elixir og Erlang deler typer og bytecode og kan derfor kalle hverandre uten ytelsestap
- Syntaktisk lettere inspirert av Ruby
- Dokumentasjon som førsteklasses element i språket, og lett tilgjengelig i REPL

*Spennende foredrag. Verdt å sjekke ut for alle som kjenner litt Erlang fra før, eller som liker webutvikling og vil lage noe som skal kjøre distribuert og skalere bra.*


## Visi: Cultured & Distributed (David Pollak)

- David Pollak
  - Lang fartstid innen utvikling av spreadsheets: Mesa, Integer BASIC
  - Kjent som grunnleggeren av Lift for Scala
  - Jobber nå med nye ideer for å implementere og jobbe med regneark
- Visi.io
  - Språk, runtime og IDE
  - Implementert i Haskell
  - Ment for å kjøre i skyen og på mobile enheter
  - Språket er ment for å være enkelt å bruke også for folk som ikke programmerer til daglig
  - Formål med prosjektet: regneark-koding er den mest utbredte formen for programmering i verden, men språket som brukes er buggy og ødelagt.

> "Basically, I want to make it easy for folks who don’t self-identify as developers to access data across the cloud and make that data available"

- Tanker/ideer Visi er basert på:
  - Selvhostet miljø gjør det lett å vokse (insiprert av Smalltalk)
  - Åpen deling av modeller (inspirert av Excel) deles via Git
  - God dokumentasjon, inspirert av JavaDoc. Literate programmering; modeller er markdown, med kode embedded
  - Testing
- Språket består av funksjoner og relasjoner mellom "sources" og "sinks" (tenk celler som input og output i regneark)
- Unntak og feil skal i størst mulig grad skjules fra brukeren (ingen ønsker å se stacktraces i regnearket sitt!)

*Greit og lite teknisk foredrag på slutten av dagen, men ikke noe jeg ville brukt tid på å se på video.*