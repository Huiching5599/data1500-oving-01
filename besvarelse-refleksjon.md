# Besvarelse på refleksjonspørsmålene

Ditt brukernavn på Oslomet: s898091@oslomet.no

## Refleksjonsspørsmål Oppgave 1 

S1: Hva er fordelene med å lagre data i et slikt format (CSV - Comma Separated Values)?
- Ditt svar: Det er enkelt og raskt å behandle, og kan leses linje for linje,samt det støttes av mange typer programmer som Google sheets, Python og databaser. 
  
S2: Hva skjer hvis et av feltene, for eksempel et navn, inneholder et komma? Hvilke problemer skaper det for din parsing-logikk?
- Ditt svar:Hvis et felt inneholder et komma og man bruker split, kan det føre til feil tolkning av data.

S3: Beregning av lagringsbehov

S3.1: La oss lage en forenklet modell for vår fil. Anta at hvert tegn (character) er 1 byte (dette er en forenkling, f.eks. husk UTF-8). Regn ut den omtrentlige størrelsen i bytes for én linje i din fil studenter.csv (f.eks., en linje er 101,Mickey,CS). Ikke glem å telle med kommaene og et tegn for linjeskift.
- Ditt svar: 101 = 3 tegn
- , = 1 tegn
- Mickey = 6 tegn
- , = 1 tegn
- CS = 2 tegn
- Sum = 3 + 1 + 6 + 1 + 2 = 13 bytes
- Total = Sum + 1 tegn for linjeskift = 14 bytes

S3.2 Basert på denne beregningen, hva ville den teoretiske filstørrelsen vært for 1 million studenter? Hvor stor for 1 milliard studenter? Uttrykk svarene i MB, GB eller TB.
- Ditt svar: 
- 1 million Studenter = 14 x 1,000,000 = 14,000,000 bytes
- 1 miliard Studenter = 14 x 1,000,000,000 = 14,000,000,000 bytes

## Refleksjonsspørsmål Oppgave 2

S1: Sammenlign tidene fra de tre testene (liten fil, tidlig treff i stor fil, og sent treff i stor fil). Hva forteller dette deg om ytelsen til en lineær skanning?
- Ditt svar:
  Det viser at liten fil tok cirka 1 ms, som er raskest. En stor fil med tidlig treff tok cirka 2 ms, mens et sent treff i stor fil tok cirka 24 ms. Dette viser at lineær skanning blir tregere jo mer av filen som må leses, og derfor øker kjøretiden med størrelsen.

S2: Hvilken rolle spiller **BufferedWriter** (en klasse i Java standard I/O biblioteket), hvis den brukes i datageneratoren? Forklar konsepter fra forelesningen, som IO Blocks og bufring.
- Ditt svar: BufferedWriter gjør filskriving mer effektiv ved å samle data i en buffer før de skrives til disk.
  Ved å skrive data i større IO-blokker reduseres disktilgangene, noe som gir bedre ytelse ved generering av store filer.

S3: Teoretisk lesetid:  

S3.1 Hva blir den faktiske filstørrelsen hvis det er 1 millioner rader i filen med brukerdata (format spesifisert i Oppgave 2.1).
- Ditt svar: Hvis vi antar ca. 30-40 bytes per rad, vil en fil med 1 000 000 rader ha en størrelse på omtrent 30-40 MB.
  
S3.2 Bruk tabellen i sliden *“Lagringsmedier – fra rask til billig”* fra forelesningen. Hva ville den teoretiske tiden vært for å lese hele denne filen (en full "scan") fra henholdsvis en **HDD (Hard Disk)** og en **High-end SSD**?
- Ditt svar: Med utgangspunkt i tabellen "Lagringsmedier fra rask til billing" og en antatt filstørresle på rundt 40MB, kan den teoretiske lesetiden estimeres for en HDD,med en sekvensiell lesehastighet på omtrent 100 MB/s, vil en full gejnnomlesning av filen ta cirka 0,4 sekunder.
og for en high-end SSD ,som kan lese data med rundt 3000 MB/s,vil samme operasjon ta omtrent 0,01 sekunder.

S3.3 Anta at følgende formel gjelder: 
```
Total Time \= AccessLatency \* M \+ DataSize/ScanThroughput 
```
For en full, sekvensiell skanning kan vi anta at M=1 (dataene leses som én stor, sammenhengende blokk). Sammenlign de teoretiske tidene med den faktiske tiden du målte i Oppgave 2.3 (Stor fil, sent treff). Hvorfor kan tidene være forskjellige? (Hint: Tenk på operativsystemets fil-caching, CPU-bruk for parsing i Java, etc.).
- Ditt svar: Den teoretiske tiden beregnes kun ut fra disktilgang og lesehastighet. Den målte tiden kan være anneledes fordi operativsystemet kan bruke fil-caching,og fordi Java bruker CPU-tid på parsing og strengsammenligninger.

## Refleksjonsspørsmål Oppgave 3

S1: Hvorfor er det mer effektivt å lese **studenter.csv** og **kurs.csv** inn i HashMap først, i stedet for å søke gjennom filene for hver eneste linje i **paameldinger.csv**?
- Ditt svar: Det er mer effektivt å bruke HashMap fordi oppslag i minnet har forventet kjøretid. Hvis vi måtte søke i filene for hver linje i paameldinger.csv, ville deet føre til mange diskoperasjoner og betydelig dårligere ytelse.

S2: Lagringsplass og minnebruk:

La oss si vi har 1 million studenter, 1000 kurs, og hver student tar i gjennomsnitt 5 kurs (totalt 5 millioner påmeldinger).
**Scenario A (Ikke-normalisert):** Vi lagrer \`studentID, fornavn, etternavn, kursID, kursnavn\` på hver linje. Anta at studentinfo trenger 100 bytes og kursinfo trenger 50 bytes for lagring. Hva blir den totale filstørrelsen for de 5 millioner påmeldingene?
- Ditt svar: Hver linje lagrer 150 bytes (100 bytes student + 50 bytes kurs).Med 5 millioner påmeldinger blir total størrelse ca.750 MB.


**Scenario B (Normalisert):** Vi har tre filer. Beregn den totale størrelsen for **studenter.csv** (1M rader), **kurs.csv** (1000 rader), og **paameldinger.csv** (5M rader, inneholder kun to ID-er, f.eks. 8 bytes totalt per rad).  
Sammenlign total lagringsplass i Scenario A og B. Hvor mye plass sparer vi på normalisering?
- Ditt svar: studenter.csv = 1,000,000 x 100 bytes = 100MB , kurs.csv =1000 x 50 bytes = 0.05 MB ,paameldinger.csv = 5,000,000 x 8 bytes = 40 MB.
totalt cirka 140 MB


S3: I din Java-løsning lastet du data inn i RAM. Se på tabellen i sliden *“Lagringsmedier – fra rask til billig”*. Hva er den typiske kostnaden per TB for RAM sammenlignet med SSD og HDD? Hvorfor er det en viktig vurdering om en datastruktur (som din HashMap-indeks) passer i RAM eller ikke?
- Ditt svar: RAM er mye dyrere per TB enn SSD og HDD, men gir langt raskere tilgang til data. Derfor er det viktig at datastrukturer som HashMap får plass i RAM for å oppnå god ytelse.

## Refleksjonsspørsmål Oppgave 4 

S1: Hva er den observerte tidsforskjellen mellom det lineære søket og det indekserte oppslaget? Hvorfor er forskjellen så enorm?
- Ditt svar: Lineært søk må lese gjennom filen linje for linje, noe som gir kjøretid O(n). Indeksert oppslag bruker HashMap i minnet og har forventet kjøretid O(1).Forskjellen blir derfor svært stor, spesielt når datasettet er stort.

S2: Hvilken lagringstype (RAM, SSD, HDD) opererer HashMap\-oppslaget på, og hva er den tilhørende “Aksesstid” ifølge tabellen fra forelesningene i sliden *“Lagringsmedier – fra rask til billig”*?
- Ditt svar: HashMap-oppslag skjer i RAM. Ifølge tabellen har RAM en aksesstid på rundt 100 nanosekunder,som er betydelig raskere enn både SSD og HDD.

Kostnaden ved en indeks:  
S3: Hvor stor ble din brukere.idx\-fil? Sammenlign størrelsen med brukere.csv.
- Ditt svar: brukere.idx er mindre enn brukere.csv, men det tar likevel ekstra lagringsplass. Indeksen lagrer kun nøkler og filposisjoner, ikke full brukerdata.
  
S4: En indeks er ikke gratis, den tar opp ekstra lagringsplass. Hvis indeksen din var 200 MB, hva ville den kostet å lagre den på en HDD? Hva med en SSD? Bruk tallene fra forelesningen, slide *“Lagringsmedier – fra rask til billig”*. Er dette en akseptabel kostnad for den ytelsesgevinsten du får? (Hint: *“tid/plass trade-off”*)
- Ditt svar: Hvis indeksen er 200 MB, tilsvarer det 0,2 GB så basert på tabellen fra sliden
- HDD ($25/TB)=  0.2 GB = 0.005 USD 
- SDD ($75/TB) = 0.2 GB = 0.015 USD
- Denne kostnaden er ubetydelig sammenlignet med den store ytelsesforbedringen indeksen gir. Indeksering illustrerer dermed tydelig et tid/plass-kompromiss, hvor litt ekstra lagringsplass gir langt raskere søk.

## Vedlegg Lagringshierarki: Hastighet, kostnad og kapasitet
Tabellen er relevant for å besvare flere av refleksjonsspørsmålene. Tabellen er kopiert fra `https://cs145-bigdata.web.app/Section2-Systems/storage-paging.html`.
|Storage Level 	|Access Latency 	|Throughput 	|Cost per TB 	|Typical Capacity 	|Use Case|
|-|-|-|-|-|-|
|CPU Registers 	|1 cycle 	|- 	|- 	|< 1KB 	|Immediate values|
|L1/L2 Cache 	|1-10ns 	|- 	|- 	|64KB - 8MB 	|Hot instructions|
|RAM (Buffer Pool) 	|100ns 	|100 GB/s 	|$3,500 	|16GB 	|Working set pages|
|SSD 	|10μs 	|5 GB/s 	|$75 	|512GB 	|Active tables|
|HDD 	|10ms 	|100 MB/s 	|$25 	|4TB 	|Cold storage|
|Network Storage 	|1μs 	|10 GB/s 	|Variable 	|∞ 	|Distributed cache|

SLUTT.
