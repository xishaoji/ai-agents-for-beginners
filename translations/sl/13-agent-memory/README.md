# Pomnilnik za AI agente 
[![Agent Memory](../../../translated_images/sl/lesson-13-thumbnail.959e3bc52d210c64.webp)](https://youtu.be/QrYbHesIxpw?si=qNYW6PL3fb3lTPMk)

Ko govorimo o edinstvenih prednostih ustvarjanja AI agentov, se večinoma omenjata dve stvari: sposobnost klicanja orodij za dokončanje nalog in sposobnost izboljševanja skozi čas. Pomnilnik je temelj za ustvarjanje samopopravljenega agenta, ki lahko ustvarja boljše izkušnje za naše uporabnike.

V tej lekciji si bomo ogledali, kaj je pomnilnik za AI agente in kako ga lahko upravljamo ter uporabljamo v korist naših aplikacij.

## Uvod

Ta lekcija bo zajemala:

• **Razumevanje AI pomnilnika agentov**: Kaj je pomnilnik in zakaj je za agente bistven.

• **Implementacija in shranjevanje pomnilnika**: Praktični načini za dodajanje zmožnosti pomnilnika vašim AI agentom, osredotočeno na kratkoročni in dolgoročni pomnilnik.

• **Samopopravljači AI agenti**: Kako pomnilnik omogoča agentom, da se učijo iz prejšnjih interakcij in se skozi čas izboljšujejo.

## Na voljo implementacije

Ta lekcija vključuje dva obširna zvezka z vadnicami:

• **[13-agent-memory.ipynb](./13-agent-memory.ipynb)**: Implementira pomnilnik z uporabo Mem0 in Azure AI Search s Semantic Kernel okvirom

• **[13-agent-memory-cognee.ipynb](./13-agent-memory-cognee.ipynb)**: Implementira strukturiran pomnilnik s Cognee, ki samodejno gradi znanstveni graf podprt z vdelavami, prikazuje graf in omogoča inteligentno iskanje

## Cilji učenja

Po zaključku te lekcije boste vedeli, kako:

• **Razločiti med različnimi vrstami AI pomnilnika agentov**, vključno z delovnim, kratkoročnim in dolgoročnim pomnilnikom, pa tudi specializiranimi oblikami, kot sta persona in epizodni pomnilnik.

• **Implementirati in upravljati kratkoročni in dolgoročni pomnilnik za AI agente** z uporabo Semantic Kernel okvira, pri tem izkoristiti orodja kot so Mem0, Cognee, Whiteboard memory in integracijo z Azure AI Search.

• **Razumeti načela za samopopravljače AI agente** in kako robustni sistemi upravljanja pomnilnika prispevajo k stalnemu učenju in prilagajanju.

## Razumevanje AI pomnilnika agentov

V osnovi **pomnilnik za AI agente pomeni mehanizme, ki jim omogočajo shranjevanje in priklic informacij**. Te informacije so lahko specifični podatki o pogovoru, uporabniške preference, pretekla dejanja ali celo pridobljeni vzorci.

Brez pomnilnika so AI aplikacije pogosto brezstanja, kar pomeni, da se vsak stik začne znova. To vodi v ponavljajočo in frustrirajočo uporabniško izkušnjo, kjer agent »pozabi« prejšnji kontekst ali preference.

### Zakaj je pomnilnik pomemben?

Inteligenca agenta je globoko povezana z njegovo zmožnostjo priklica in uporabe preteklih informacij. Pomnilnik agentom omogoča, da so:

• **Reflektivni**: Učenje iz preteklih dejanj in izidov.

• **Interaktivni**: Ohranjanje konteksta skozi tekoči pogovor.

• **Proaktivni in reaktivni**: Predvidevanje potreb ali ustrezno odzivanje na podlagi zgodovinskih podatkov.

• **Avtonomni**: Delujejo bolj neodvisno z uporabo shranjenega znanja.

Cilj implementacije pomnilnika je narediti agente bolj **zanesljive in zmožne**.

### Vrste pomnilnika

#### Delovni pomnilnik

To si predstavljajte kot list papirja, ki ga agent uporablja med eno tekočo nalogo ali razmišljanjem. V njem so takojšnje informacije, potrebne za izračun naslednjega koraka.

Za AI agente delovni pomnilnik pogosto zajema najpomembnejše informacije iz pogovora, tudi če je celotna zgodovina pogovora dolga ali skrajšana. Osredotoča se na izločanje ključnih elementov, kot so zahteve, predlogi, odločitve in dejanja.

**Primer delovnega pomnilnika**

Pri agentu za rezervacijo potovanj delovni pomnilnik lahko shrani trenutno uporabnikovo zahtevo, kot je »Želim rezervirati potovanje v Pariz«. Ta specifična zahteva je v agentovem takojšnjem kontekstu za usmerjanje trenutne interakcije.

#### Kratkoročni pomnilnik

Ta vrsta pomnilnika shranjuje informacije za trajanje enega pogovora ali seje. Je kontekst tekočega klepeta, ki agentu omogoča, da se v pogovoru sklicuje na prejšnje odzive.

**Primer kratkoročnega pomnilnika**

Če uporabnik vpraša »Koliko bi stal let v Pariz?« in nato doda »Kaj pa nastanitev tam?«, kratkoročni pomnilnik zagotovi, da agent ve, da »tam« pomeni »v Parizu« znotraj istega pogovora.

#### Dolgoročni pomnilnik

To so informacije, ki trajajo skozi več pogovorov ali sej. Omogoča agentom, da si zapomnijo uporabniške preference, zgodovinske interakcije ali splošno znanje skozi daljša obdobja. To je pomembno za personalizacijo.

**Primer dolgoročnega pomnilnika**

Dolgoročni pomnilnik lahko shrani, da »Ben uživa v smučanju in zunanjih aktivnostih, rad pije kavo z razgledom na gore in se želi izogniti zahtevnim smučarskim progama zaradi pretekle poškodbe«. Te informacije, pridobljene iz prejšnjih interakcij, vplivajo na priporočila v prihodnjih načrtih potovanj, zaradi česar so zelo personalizirana.

#### Persona pomnilnik

Ta specializirana vrsta pomnilnika pomaga agentu razviti dosledno »osebnost« ali »persona«. Agentu omogoča, da se spomni podrobnosti o sebi ali svoji vlogi, kar naredi interakcije bolj tekoče in osredotočene.

**Primer persona pomnilnika**

Če je agent za potovanja zasnovan kot »strokovnjak za načrtovanje smučarskih počitnic«, lahko persona pomnilnik okrepi to vlogo in vpliva na njegove odzive, da so skladni s tonom in znanjem strokovnjaka.

#### Delovni/epizodni pomnilnik

Ta pomnilnik shranjuje zaporedje korakov, ki jih agent izvede med kompleksno nalogo, vključno z uspehi in neuspehi. Je kot spomin na specifične »epizode« oziroma pretekle izkušnje, iz katerih se agent uči.

**Primer epizodnega pomnilnika**

Če je agent poskušal rezervirati določen let, vendar je to spodletelo zaradi nedosegljivosti, bi epizodni pomnilnik zabeležil neuspeh, kar agentu omogoča, da pri naslednjem poskusu poskusi alternativne lete ali na bolj informiran način obvesti uporabnika o težavi.

#### Pomnilnik entitet

Vključuje izločanje in pomnjenje specifičnih entitet (kot so ljudje, kraji ali stvari) in dogodkov iz pogovorov. Omogoča agentu, da zgradi strukturiran pogled o ključnih elementih, ki so bili omenjeni.

**Primer pomnilnika entitet**

Iz pogovora o preteklem potovanju bi agent lahko izločil »Pariz«, »Eifflov stolp« in »večerja v restavraciji Le Chat Noir« kot entitete. V prihodnji interakciji bi lahko agent priklical »Le Chat Noir« in ponudil novo rezervacijo tam.

#### Strukturirani RAG (Retrieval Augmented Generation)

Medtem ko je RAG širša tehnika, je »strukturirani RAG« poudarjen kot zmogljiva tehnologija pomnilnika. Izvleče gosto, strukturirano informacijo iz različnih virov (pogovori, elektronska pošta, slike) in jo uporablja za izboljšanje natančnosti, priklica in hitrosti odgovorov. Za razliko od klasičnega RAG, ki se zanaša samo na semantično podobnost, Strukturirani RAG deluje z inherentno strukturo informacij.

**Primer strukturiranega RAG**

Namesto samo ujemanja ključnih besed bi strukturirani RAG lahko izluščil podatke o letu (destinacija, datum, čas, letalska družba) iz e-pošte in jih shranil strukturirano. To omogoča natančna vprašanja kot »Kateri let sem rezerviral v Pariz v torek?«

## Implementacija in shranjevanje pomnilnika

Implementacija pomnilnika za AI agente vključuje sistematičen proces **upravljanja pomnilnika**, ki zajema ustvarjanje, shranjevanje, priklic, integracijo, posodabljanje in celo »pozabljanje« (ali brisanje) informacij. Priklic je posebej ključni vidik.

### Specializirana orodja za pomnilnik

#### Mem0

Eden od načinov shranjevanja in upravljanja pomnilnika agenta je uporaba specializiranih orodij, kot je Mem0. Mem0 deluje kot trajni pomnilniški sloj, agentom omogoča priklic relevantnih interakcij, shranjevanje uporabniških preferenc in dejanskega konteksta ter učenje iz uspehov in neuspehov skozi čas. Namen je preoblikovati brezstanje agente v stanje.

Deluje prek **dvofazne pomnilniške cevi: ekstrakcije in posodobitve**. Najprej se sporočila, dodana v nit agenta, pošljejo v storitev Mem0, ki uporablja Velik jezikovni model (LLM) za povzetek zgodovine pogovora in izločanje novih spominov. Nato LLM vodena faza posodobitve določi, ali dodati, spremeniti ali izbrisati te spomine, ki se shranjujejo v hibridno podatkovno bazo, ki lahko vključuje vektorske, grafične in ključ-vrednost podatkovne baze. Sistem podpira tudi različne vrste pomnilnika in vključuje grafični pomnilnik za upravljanje odnosov med entitetami.

#### Cognee

Drugi močan pristop je uporaba **Cognee**, odprtokodnega semantičnega pomnilnika za AI agente, ki preoblikuje strukturirane in nestrukturirane podatke v poizvedljive znanstvene grafe, podprte z vdelavami. Cognee ponuja **arhitekturo dveh skladišč**, ki združuje vektorsko iskanje podobnosti z grafičnimi povezavami, kar agentom omogoča razumevanje ne samo podobnosti informacij, ampak tudi, kako so koncepti povezani.

Izstopa pri **hibridnem priklicu**, ki združuje vektorsko podobnost, grafično strukturo in LLM sklepanje – od iskanja po kosih podatkov do odgovorov na vprašanja, ki zavedajo grafa. Sistem ohranja **živ pomnilnik**, ki se razvija in raste ter ostaja poizvedljiv kot enoten povezan graf, podpira tako kratkoročni kontekst seje kot dolgoročni trajni pomnilnik.

Tutorial v zvezku Cognee ([13-agent-memory-cognee.ipynb](./13-agent-memory-cognee.ipynb)) prikazuje gradnjo tega enotnega pomnilniškega sloja s praktičnimi primeri vnosa različnih virov podatkov, prikaza znanstvenega grafa in poizvedovanja z različnimi strategijami iskanja, prilagojenimi posebnim potrebam agenta.

### Shranjevanje pomnilnika z RAG

Poleg specializiranih orodij za pomnilnik, kot je mem0, lahko izkoristite zmogljive iskalne storitve, kot je **Azure AI Search**, kot podporo za shranjevanje in priklic spominov, še posebej za strukturirani RAG.

To vam omogoča, da podkrepite odgovore vašega agenta z lastnimi podatki, s čimer zagotovite bolj relevantne in natančne odgovore. Azure AI Search se lahko uporablja za shranjevanje spominov na uporabnikova potovanja, produktne kataloge ali katerokoli drugo znanje, specifično za domeno.

Azure AI Search podpira zmogljivosti, kot je **strukturirani RAG**, ki odlično izvleče in prikliče gosto, strukturirano informacijo iz velikih zbirk podatkov, kot so zgodovine pogovorov, e-pošta ali celo slike. To zagotavlja »popolno človeško natančnost in priklic« v primerjavi s tradicionalnimi pristopi razdelitve besedila in vdelavami.

## Samopopravljači AI agenti

Pogosta praksa za samopopravljače agente vključuje uvedbo **»agenta za znanje«**. Ta ločeni agent opazuje glavni pogovor med uporabnikom in primarnim agentom. Njegova vloga je:

1. **Identificirati dragocene informacije**: Ugotoviti, ali je kateri koli del pogovora vreden shranitve kot splošno znanje ali specifična uporabniška preferenca.

2. **Izluščiti in povzeti**: Izluščiti bistveno učenje ali preference iz pogovora.

3. **Shranjevati v bazo znanja**: Ohraniti te izluščene informacije, pogosto v vektorski podatkovni bazi, da jih je mogoče pozneje priklicati.

4. **Obogatiti prihodnje poizvedbe**: Ko uporabnik uvede novo poizvedbo, agent za znanje prikliče relevantne shranjene informacije in jih doda v uporabnikov poziv, s tem primarnemu agentu zagotovi ključen kontekst (na podoben način kot RAG).

### Optimizacije za pomnilnik

• **Upravljanje zakasnitve**: Da se ne upočasnijo uporabniške interakcije, se lahko sprva uporablja cenejši, hitrejši model za hiter pregled, ali je informacija vredna shranjevanja ali priklica, pri čemer se bolj zapleteni postopek izvlečenja/priklica sproži le, ko je to potrebno.

• **Vzdrževanje baze znanja**: Za naraščajočo bazo znanja se manj pogosto uporabljene informacije lahko premaknejo v »hladno shrambo« za upravljanje stroškov.

## Imate več vprašanj o pomnilniku agentov?

Pridružite se [Azure AI Foundry Discord](https://aka.ms/ai-agents/discord), da se srečate z drugimi učenci, udeležite uradnih ur in dobite odgovore na vaša vprašanja o AI agentih.

---

<!-- CO-OP TRANSLATOR DISCLAIMER START -->
**Izjava o omejitvi odgovornosti**:
Ta dokument je bil preveden z uporabo storitve za prevajanje z umetno inteligenco [Co-op Translator](https://github.com/Azure/co-op-translator). Čeprav si prizadevamo za natančnost, vas opozarjamo, da lahko avtomatizirani prevodi vsebujejo napake ali netočnosti. Izvirni dokument v njegovem maternem jeziku velja za avtoritativni vir. Za ključne informacije priporočamo strokovni človeški prevod. Za kakršnekoli nesporazume ali napačne interpretacije, ki izhajajo iz uporabe tega prevoda, ne prevzemamo odgovornosti.
<!-- CO-OP TRANSLATOR DISCLAIMER END -->