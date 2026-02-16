# Memorija za AI agente  
[![Agent Memory](../../../translated_images/hr/lesson-13-thumbnail.959e3bc52d210c64.webp)](https://youtu.be/QrYbHesIxpw?si=qNYW6PL3fb3lTPMk)

Kada se raspravlja o jedinstvenim prednostima stvaranja AI agenata, uglavnom se govore dvije stvari: sposobnost pozivanja alata za izvršavanje zadataka i sposobnost poboljšavanja tijekom vremena. Memorija je temelj stvaranja samopoboljšavajućeg agenta koji može stvarati bolje iskustvo za naše korisnike.

U ovoj lekciji pogledat ćemo što je memorija za AI agente i kako je možemo upravljati i koristiti za dobrobit naših aplikacija.

## Uvod

Ova lekcija će obuhvatiti:

• **Razumijevanje memorije AI agenta**: Što je memorija i zašto je bitna za agente.

• **Implementacija i pohrana memorije**: Praktične metode dodavanja sposobnosti memorije vašim AI agentima, s fokusom na kratkoročnu i dugoročnu memoriju.

• **Kako AI agenti postaju samopoboljšavajući**: Kako memorija omogućuje agentima učenje iz prošlih interakcija i poboljšavanje tijekom vremena.

## Dostupne implementacije

Ova lekcija uključuje dva opsežna notebook tutorijala:

• **[13-agent-memory.ipynb](./13-agent-memory.ipynb)**: Implementira memoriju koristeći Mem0 i Azure AI Search sa Semantic Kernel frameworkom

• **[13-agent-memory-cognee.ipynb](./13-agent-memory-cognee.ipynb)**: Implementira strukturiranu memoriju koristeći Cognee, automatski gradi graf znanja potpomognut embeddingima, vizualizira graf i inteligentno dohvaćanje

## Ciljevi učenja

Nakon završetka ove lekcije, znat ćete kako:

• **Razlikovati različite vrste memorije AI agenata**, uključujući radnu, kratkoročnu i dugoročnu memoriju, kao i specijalizirane oblike kao što su persona i epizodna memorija.

• **Implementirati i upravljati kratkoročnom i dugoročnom memorijom za AI agente** koristeći Semantic Kernel framework, koristeći alate poput Mem0, Cognee, Whiteboard memoriju i integraciju s Azure AI Search.

• **Razumjeti principe iza samopoboljšavajućih AI agenata** i kako robusni sustavi upravljanja memorijom doprinose kontinuiranom učenju i prilagodbi.

## Razumijevanje memorije AI agenta

U svojoj suštini, **memorija za AI agente odnosi se na mehanizme koji im omogućuju zadržavanje i prisjećanje informacija**. Te informacije mogu biti specifični detalji o razgovoru, korisničke preferencije, prošli postupci ili čak naučeni obrasci.

Bez memorije, AI aplikacije često su bez stanja, što znači da svaki razgovor počinje iznova. To vodi do ponavljajućeg i frustrirajućeg korisničkog iskustva gdje agent "zaboravlja" prethodni kontekst ili preferencije.

### Zašto je memorija važna?

Inteligencija agenta duboko je povezana s njegovom sposobnošću prisjećanja i korištenja prošlih informacija. Memorija omogućuje agentima da budu:

• **Reflektivni**: Učeći iz prošlih postupaka i ishoda.

• **Interaktivni**: Održavajući kontekst tijekom tekućeg razgovora.

• **Proaktivni i reaktivni**: Predviđajući potrebe ili odgovarajući prikladno na temelju povijesnih podataka.

• **Autonomni**: Djelujući samostalnije koristeći pohranjeno znanje.

Cilj implementacije memorije je učiniti agente pouzdanijima i sposobnijima.

### Vrste memorije

#### Radna memorija

Zamislite ovo kao komad papira koji agent koristi tijekom jedne, tekuće zadaće ili misaonog procesa. Ona drži neposredne informacije potrebne za izračun sljedećeg koraka.

Za AI agente, radna memorija često hvata najvažnije informacije iz razgovora, čak i ako je čitava povijest chata duga ili skraćena. Fokusira se na izvlačenje ključnih elemenata poput zahtjeva, prijedloga, odluka i radnji.

**Primjer radne memorije**

U agentu za rezervaciju putovanja, radna memorija može zadržati trenutni zahtjev korisnika, poput "Želim rezervirati put u Pariz". Taj specifični zahtjev držan je u neposrednom kontekstu agenta kako bi usmjerio trenutnu interakciju.

#### Kratkoročna memorija

Ova vrsta memorije zadržava informacije tijekom trajanja jedne konverzacije ili sesije. To je kontekst trenutnog chata, dopuštajući agentu da se pozove na prethodne dijelove dijaloga.

**Primjer kratkoročne memorije**

Ako korisnik pita: "Koliko bi koštao let za Pariz?" a zatim nastavi s "A što je s smještajem tamo?", kratkoročna memorija osigurava da agent zna da se "tamo" odnosi na "Pariz" u istom razgovoru.

#### Dugoročna memorija

To su informacije koje traju kroz više konverzacija ili sesija. Omogućava agentima da se sjećaju korisničkih preferencija, povijesnih interakcija ili općeg znanja kroz duži vremenski period. Ovo je važno za personalizaciju.

**Primjer dugoročne memorije**

Dugoročna memorija može pohraniti podatak da "Ben voli skijanje i aktivnosti na otvorenom, voli kavu s pogledom na planine i želi izbjegavati zahtjevne skijaške staze zbog prošle ozljede". Te informacije, naučene iz prethodnih interakcija, utječu na preporuke u budućim sesijama planiranja putovanja, čineći ih vrlo personaliziranim.

#### Persona memorija

Ova specijalizirana vrsta memorije pomaže agentu razviti dosljednu "osobnost" ili "personu". Omogućuje agentu da pamti detalje o sebi ili svojoj namjeravanoj ulozi, čineći interakcije fluidnijima i fokusiranijima.

**Primjer persona memorije**

Ako je agent za putovanje dizajniran kao "stručnjak za planiranje skijanja," persona memorija može pojačati ovu ulogu, utječući na njegove odgovore da budu u skladu s tonom i znanjem stručnjaka.

#### Radni / Epizodnička memorija

Ova memorija pohranjuje niz koraka koje agent poduzima tijekom složenog zadatka, uključujući uspjehe i neuspjehe. Kao da pamti specifične "epizode" ili prošla iskustva kako bi iz njih učio.

**Primjer epizodičke memorije**

Ako je agent pokušao rezervirati određeni let, ali nije uspio zbog nedostupnosti, epizodna memorija može zabilježiti taj neuspjeh, dopuštajući agentu da pokuša alternativne letove ili korisnika informira o problemu na informiraniji način pri sljedećem pokušaju.

#### Memorija entiteta

Ovo uključuje izvlačenje i pamćenje specifičnih entiteta (kao što su ljudi, mjesta ili stvari) i događaja iz razgovora. Omogućuje agentu da izgradi strukturirano razumijevanje ključnih elemenata o kojima se razgovaralo.

**Primjer memorije entiteta**

Iz razgovora o prošlom putovanju, agent može izvući "Pariz," "Eiffelov toranj," i "večeru u restoranu Le Chat Noir" kao entitete. U budućoj interakciji agent može prisjetiti se "Le Chat Noir" i ponuditi da napravi novu rezervaciju tamo.

#### Strukturirani RAG (Retrieval Augmented Generation)

Dok je RAG šira tehnika, "Strukturirani RAG" istaknut je kao moćna tehnologija memorije. Izvlači gusto, strukturirano znanje iz različitih izvora (razgovora, mailova, slika) i koristi ga za poboljšanje preciznosti, poziva i brzine u odgovorima. Za razliku od klasičnog RAG-a koji se oslanja samo na semantičku sličnost, Strukturirani RAG radi s inherentnom strukturom informacija.

**Primjer strukturiranog RAG-a**

Umjesto samo podudaranja ključnih riječi, Strukturirani RAG može pročitati detalje leta (odredište, datum, vrijeme, zrakoplovna tvrtka) iz mejla i pohraniti ih na strukturiran način. To omogućuje precizna pitanja poput "Koji sam let rezervirao za Pariz u utorak?"

## Implementacija i pohrana memorije

Implementacija memorije za AI agente uključuje sustavan proces **upravljanja memorijom**, koji uključuje generiranje, pohranu, dohvaćanje, integraciju, ažuriranje, pa čak i "zaboravljanje" (brisanje) informacija. Dohvaćanje je posebno važan aspekt.

### Specijalizirani alati za memoriju

#### Mem0

Jedan od načina pohrane i upravljanja memorijom agenta je korištenje specijaliziranih alata poput Mem0. Mem0 radi kao sloj trajne memorije, omogućujući agentima da se prisjećaju relevantnih interakcija, pohranjuju korisničke preference i činjenični kontekst te uče iz uspjeha i neuspjeha tijekom vremena. Ideja je da stateless agenti postanu stateful.

Radi kroz **dvodijelni memorijski proces: ekstrakcija i ažuriranje**. Prvo, poruke dodane u agentovu nit šalju se Mem0 servisu koji koristi veliki jezični model (LLM) za sažimanje povijesti razgovora i izvlačenje novih memorija. Zatim, faza ažuriranja kojom upravlja LLM odlučuje hoće li ih dodati, promijeniti ili izbrisati, pohranjujući ih u hibridnu bazu podataka koja može uključivati vektorske, grafičke i ključ-vrijednost baze. Sustav također podržava različite vrste memorije i može uključiti graf memoriju za upravljanje odnosima između entiteta.

#### Cognee

Drugi snažan pristup je korištenje **Cognee**, open-source semantičke memorije za AI agente koja pretvara strukturirane i nestrukturirane podatke u upitne grafove znanja potpomognute embeddingom. Cognee nudi **arhitekturu dual-store** koja kombinira pretraživanje po vektorskoj sličnosti s grafičkim odnosima, omogućujući agentima razumijevanje ne samo što je slično, nego i kako su koncepti povezani.

Izvrsno je u **hibridnom dohvaćanju** koje spaja vektorsku sličnost, grafičku strukturu i LLM rezoniranje – od jednostavnog pretraživanja do odgovaranja na pitanja svjesna grafa. Sustav održava **živu memoriju** koja evoluira i raste dok ostaje upitna kao povezani graf, podržavajući i kratkoročni kontekst sesije i dugoročnu trajnu memoriju.

Cognee notebook tutorijal ([13-agent-memory-cognee.ipynb](./13-agent-memory-cognee.ipynb)) demonstrira izgradnju ovog objedinjena sloja memorije, s praktičnim primjerima ingestije raznovrsnih izvora podataka, vizualizacije grafa znanja i upita s različitim strategijama pretraživanja prilagođenim potrebama agenta.

### Pohrana memorije s RAG-om

Osim specijaliziranih memorijskih alata poput Mem0, možete iskoristiti robusne servise pretraživanja poput **Azure AI Search kao backend za pohranu i dohvat memorija**, posebno za strukturirani RAG.

Ovo omogućuje da ukorijenite odgovore vašeg agenta u vlastite podatke, osiguravajući relevantnije i točnije odgovore. Azure AI Search može se koristiti za pohranu korisničkih memorija o putovanjima, katalozima proizvoda ili bilo kojem drugom znanju specifičnom za domenu.

Azure AI Search podržava značajke poput **Strukturiranog RAG-a**, koji izvrsno izvlači i dohvaća gusto, strukturirano znanje iz velikih skupova podataka kao što su povijesti razgovora, mailovi ili čak slike. To pruža "nadljudsku preciznost i poziv" u usporedbi s tradicionalnim pristupima fragmentiranju teksta i embediranja.

## Kako učiniti AI agente samopoboljšavajućima

Uobičajeni model za samopoboljšavajuće agente uključuje uvođenje **"agenta znanja"**. Taj zaseban agent promatra glavnu konverzaciju između korisnika i glavnog agenta. Njegova uloga je:

1. **Identificirati vrijedne informacije**: Odrediti je li bilo koji dio razgovora vrijedan spremanja kao opće znanje ili specifična korisnička preferencija.

2. **Izvući i sažeti**: Destilirati bitno učenje ili preferenciju iz razgovora.

3. **Pohraniti u bazu znanja**: Sačuvati ove informacije, često u vektorskoj bazi, kako bi se kasnije mogle dohvatiti.

4. **Proširiti buduće upite**: Kada korisnik pokrene novi upit, agent znanja dohvaća relevantne pohranjene podatke i dodaje ih u korisnički prompt, pružajući ključni kontekst glavnom agentu (slično RAG-u).

### Optimizacije za memoriju

• **Upravljanje latencijom**: Kako ne bi usporili korisničke interakcije, može se isprva koristiti jeftiniji, brži model za brzo provjeravanje je li informacija vrijedan podatak za pohranu ili dohvat, aktivirajući složeniji proces ekstrakcije/dohvata samo kada je potrebno.

• **Održavanje baze znanja**: Za rastuću bazu znanja, manje često korištene informacije mogu se premjestiti u "hladnu pohranu" kako bi se smanjili troškovi.

## Imate još pitanja o memoriji agenta?

Pridružite se [Azure AI Foundry Discord](https://aka.ms/ai-agents/discord) gdje možete upoznati druge učenike, sudjelovati u office hours i dobiti odgovore na svoja pitanja o AI agentima.

---

<!-- CO-OP TRANSLATOR DISCLAIMER START -->
**Izjava o ograničenju odgovornosti**:  
Ovaj je dokument preveden korištenjem AI prevoditeljskog servisa [Co-op Translator](https://github.com/Azure/co-op-translator). Iako nastojimo postići točnost, imajte na umu da automatski prijevodi mogu sadržavati pogreške ili netočnosti. Izvorni dokument na izvornom jeziku treba smatrati autoritativnim izvorom. Za važne informacije preporučuje se profesionalni ljudski prijevod. Ne snosimo odgovornost za bilo kakve nesporazume ili pogrešne interpretacije koje proizlaze iz korištenja ovog prijevoda.
<!-- CO-OP TRANSLATOR DISCLAIMER END -->