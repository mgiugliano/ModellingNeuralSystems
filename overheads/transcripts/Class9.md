## Introduzione e Riepilogo: Dall'Equazione della Corrente Sinaptica all'Approccio Statistico

In quella lezione (online) vi ho mostrato come, prendendo un caso specifico, sia possibile analizzare una particolare equazione differenziale. Anche se non l'avete ancora guardata o approfondita, il concetto è autoconsistente. L'idea è capire come trattare questa equazione: date certe ipotesi relative a un suo termine, è possibile estrarre informazioni statistiche e sostituirlo con qualcos'altro.

### Il Processo di Stein e l'Approssimazione di Diffusione

Il termine in questione, che ho descritto tramite il cosiddetto **processo di Stein**, può essere analizzato statisticamente. Questo permette di sostituirlo, in quella che viene chiamata **approssimazione di diffusione**, con un processo stocastico che non è più puntuale (un *point process*, che genera uno *shot noise*), ma è invece un processo stocastico a tempo continuo. Un esempio di tale processo è un rumore bianco, che potreste teoricamente inserire nell'equazione.

Perché questo ha senso e importanza? Perché questa equazione, che ora riprendo e discuto brevemente, è l'equazione della corrente sinaptica totale che un neurone generico, all'interno di una rete, riceve. Questa equazione, unitamente alla curva frequenza-corrente (o, più in generale, alla dinamica con cui gli spike vengono generati data una certa corrente, come in un modello *integrate-and-fire* o *conductance-based*), potrebbe dirci tutto su come una rete evolve, almeno in una certa approssimazione detta di **campo medio**. Lo studio si riduce a osservare come un singolo neurone generico si comporta quando riceve una corrente media.

Così com'è scritta, l'equazione descrive una corrente specifica: quella totale ricevuta dal neurone *i-esimo*. Essa include quindi tutte le caratteristiche specifiche della rete, come la matrice di connettività, l'efficacia sinaptica e l'esatta sequenza di attivazione delle fibre afferenti che arrivano a quel neurone. Infatti, la somma è estesa a tutti i neuroni *j-esimi* che sono presinaptici rispetto al neurone *i-esimo* (per i neuroni non connessi, il valore corrispondente nella matrice di connettività sarà semplicemente zero).

Con l'approccio statistico, invece, si riesce a sostituire questo termine con un processo stocastico diffusivo e continuo. Questo è vantaggioso perché, nonostante il mio interesse per le delta di Dirac, sono oggetti matematicamente complessi da trattare. Al contrario, un processo stocastico a valori e tempo continui, senza impulsi, dove istante per istante si ha una variabile aleatoria (magari Gaussiana, che ha altre piacevoli proprietà), rende le cose più semplici.

### Media e Varianza: Il Campo Medio e il Campo Medio Esteso

Nei due video, vi ho mostrato qual è il processo diffusivo da inserire nell'equazione: si tratta, in sostanza, di un rumore bianco con media e varianza note. Queste ultime sono determinate dalle caratteristiche statistiche del termine originale, in particolare dalla statistica degli istanti di spike. A questo punto, abbiamo fondamentalmente due strade. La prima è utilizzare sia la media che la varianza. Sapendo che il processo approssimante è Gaussiano, media e varianza sono sufficienti per caratterizzarlo completamente, poiché determinano in modo univoco la sua densità di distribuzione di probabilità (cosa non vera per altre distribuzioni).

Tuttavia, in questi discorsi sull'approssimazione di campo medio, l'enfasi è sul "campo di forze" medio. Questa terminologia deriva dalla fisica dei vetri di spin, dove il campo di ingresso a una singola unità di un sistema complesso è descritto, appunto, in termini medi. Quando la settimana scorsa ho illustrato i passaggi per derivare l'approssimazione di campo medio, ho applicato l'operatore valore atteso per calcolare solo la media della corrente. A questo punto potreste obiettare: "Aspetta, nei video hai insistito molto sul fatto che si ottiene anche la varianza". Esatto, e quello si chiama **campo medio esteso**, poiché include anche la varianza. Per la trattazione odierna, e per rendere la lezione odierna più indipendente da quella registrata, mi basterà descrivere il campo medio semplice, non quello esteso. Il vantaggio di questa descrizione semplificata è che tra poco potremo analizzare i risultati con strumenti grafici, senza bisogno di calcoli numerici complessi.

Per farla breve, l'equazione ottenuta la settimana scorsa, che descrive il valore medio della corrente, alla luce dei video extra rappresenta solo una parte della storia. In teoria, potrei scrivere un'equazione analoga per la varianza della corrente, sfruttando la corrispondenza tra il processo puntuale di tipo Poisson e la sua approssimazione diffusiva. Per ora, però, consideriamo solo la media. L'unica differenza rispetto alla trattazione online è che qui mancherebbe la costante di tempo $\tau$, data dall'inverso di $\beta$, ma l'equazione resta ugualmente rappresentativa.

## Dalla Simulazione Microscopica alla Dinamica di Campo Medio

Ricapitoliamo l'idea di fondo. Immaginiamo di avere una rete complessa. Per ora, ipotizziamo una rete senza connessioni riverberanti e ricorrenti, sebbene l'approccio funzioni anche quando questa ipotesi è violata. Anziché descrivere e simulare ogni singolo neurone — supponiamo qualche milione o miliardo, ciascuno con le proprie variabili di stato (ad esempio, un modello *integrate-and-fire*) e una matrice di connettività sparsa — evitiamo un approccio di "forza bruta". Questo approccio microscopico richiederebbe di risolvere, istante per istante, una o due equazioni differenziali per ciascuno dei milioni di neuroni, oltre all'equazione per la corrente sinaptica totale di ogni unità.

Invece di procedere così, passiamo a una descrizione media, approssimata, in cui scriviamo una sola equazione. Questa equazione descrive l'evoluzione della corrente che un neurone generico sperimenta, dato che per ipotesi tutti i neuroni sono indistinguibili. Per semplicità, ignoriamo il contributo della varianza. In questo modo, passiamo da un sistema con milioni di equazioni differenziali a uno con una sola equazione differenziale. A questa si aggiunge una condizione ulteriore: la **curva frequenza-corrente**, ovvero una relazione che ci dice come questa corrente media viene convertita in una frequenza di sparo (spike al secondo). Si tratta di una non-linearità statica, una funzione data, che nel caso del modello *integrate-and-fire* abbiamo persino calcolato analiticamente.

### Ipotesi Fondamentali del Modello

Alla base di questa riduzione ci sono alcune ipotesi chiave:
1.  **Tutti i neuroni hanno parametri intrinseci identici**. Se così non fosse, non potremmo accorparli in un unico "blob" omogeneo.
2.  **Tutti i neuroni sparano in modo indipendente dagli altri**. Questa è un'ipotesi forte e potreste essere in disaccordo, specialmente in una rete con connessioni ricorrenti, dove esiste un feedback che inevitabilmente influenza l'attività reciproca. Tuttavia, anche rilassando questa condizione, l'approccio continua a funzionare bene.
3.  **Ciascun neurone spara in modo irregolare**. Senza questa ipotesi, la trattazione matematica che si basa su processi stocastici e variabili aleatorie non sarebbe applicabile. Abbiamo bisogno di un certo grado di disordine per poter utilizzare gli strumenti della teoria della probabilità.

Consideriamo la rete arbitrariamente complicata nel diagramma. In questo cartone, non ci sono connessioni ricorrenti, quindi i neuroni sparano in modo quasi indipendente, sebbene l'attività di un neurone sia ovviamente influenzata da quella dei suoi neuroni presinaptici. La teoria del campo medio funziona meglio quando viene estesa per includere la varianza. Ma anche nella sua forma più semplice, il suo potere è notevole. Invece di risolvere, ad esempio, sei equazioni differenziali per sei neuroni (o dodici, se includiamo l'adattamento), descriviamo il campo medio che un neurone generico di questo "blob" sperimenta con un'unica equazione.

### Nascita dei Modelli di "Firing Rate"

Il modello si riduce quindi a due elementi essenziali: l'equazione differenziale per la corrente media e la curva frequenza-corrente. Quest'ultima è una non-linearità statica che mappa la corrente d'ingresso sinaptica $I$ in una frequenza di uscita $f$. L'equazione differenziale, a sua volta, descrive come la corrente sinaptica media varia al variare della frequenza presinaptica.

Questa coppia di condizioni suggerisce la possibilità di descrivere l'attività della rete in modo compatto attraverso il cosiddetto **firing rate** (frequenza di sparo). In questa descrizione non ci sono più singoli spike; l'unica variabile che descrive l'attività della rete è la frequenza $F$, che può cambiare nel tempo in funzione della corrente sinaptica. Questo approccio, derivato come una riduzione del modello più dettagliato, dà origine a una classe di modelli noti come modelli di *firing rate*.

### Relazione con altri Modelli Neuronali

Nei libri di testo, come quello di Gerstner, la trattazione è spesso compartimentata: ci sono i modelli *conductance-based*, i modelli *integrate-and-fire*, e i modelli *firing rate*, senza che venga necessariamente esplicitata una derivazione formale da un tipo all'altro. Tra i modelli *conductance-based* e quelli *integrate-and-fire*, ho cercato di farvi sviluppare l'intuizione che l'equazione fondamentale del bilancio della carica, $C \frac{dV}{dt} = \dots$, continui a valere, ma che nel secondo caso si siano semplicemente trascurate le correnti ioniche voltaggio-dipendenti, come quelle del sodio e del potassio. Si potrebbe obiettare che i valori di capacità $C$ e di conduttanza di *leak* $g_L$ rimangono comunque gli stessi.

Anche in questo caso, ho cercato di mostrarvi una derivazione. Se avete una popolazione di neuroni connessi sinapticamente, descritti da un modello marcoviano a due stati per i canali (aperto e chiuso), con alcune approssimazioni (come ignorare la saturazione e considerare gli impulsi di neurotrasmettitore come delta di Dirac), è possibile, applicando l'operatore media, arrivare a questo tipo di modelli di *firing rate*.

In altri libri, invece, l'approccio è più assiomatico: il *firing rate* di una popolazione è descritto da un'equazione differenziale che definisce una quantità (la corrente media) la quale viene poi data in input a una non-linearità istantanea (la curva frequenza-corrente). Questo ci dice come cambia il *firing rate* del "blob" quando, ad esempio, modifichiamo l'attività presinaptica, il numero di neuroni, la connettività media, l'efficacia sinaptica media, o persino la cinetica di chiusura dei recettori (AMPA, NMDA, GABA, etc.).

Personalmente, preferisco questo approccio basato sulla riduzione, perché ci costringe a riflettere su cosa stiamo "buttando dalla finestra". Qui è evidente che non abbiamo più i singoli spike e, in modo ancora più radicale, non abbiamo più nemmeno i singoli neuroni.

L'elemento cruciale di questa riduzione è la descrizione della corrente sinaptica, più ancora del modello neuronale specifico (*integrate-and-fire* o altro). Il neurone stesso è stato ridotto a una semplice scatola nera con una relazione input-output: se mi fornisci una corrente sinaptica, ti rispondo con un certo numero di spike al secondo.

### Una Semplificazione Necessaria: Dalla Teoria Estesa a quella Semplice

A questo punto, potreste giustamente osservare che le espressioni per la curva frequenza-corrente che abbiamo derivato si applicavano al caso di un firing regolare, non irregolare. Questa è una semplificazione che adotto per non appesantire eccessivamente la trattazione. In realtà, la vera funzione di trasferimento (la curva frequenza-corrente) che permetteva al modello *integrate-and-fire* di predire i dati sperimentali che vi ho mostrato settimane fa, teneva conto non solo della media della corrente, ma anche della sua varianza e del suo tempo di correlazione.

Qui sto facendo una cosa più semplice, in linea con quanto abbiamo derivato insieme per il caso di un input costante e quindi di un firing regolare. Sto assumendo che, grosso modo, la relazione funzioni in modo simile. Per fare le cose in modo rigoroso, bisognerebbe avere un'equazione per la media, un'altra equazione per la varianza, e una funzione di trasferimento che dipenda da entrambe. Queste forme analitiche esistono per alcuni modelli neuronali (come l'*integrate-and-fire* leaky, quadratico o esponenziale), ma sono complicate. Per facilità, per il momento, ci limitiamo alla condizione in cui consideriamo solo la media. Questa è la **teoria di campo medio**, non la **teoria di campo medio esteso**, ed è storicamente la prima ad essere stata proposta negli anni '70.

### Approssimazione della Curva Frequenza-Corrente: La Funzione *Threshold-Linear*

Vi chiedo ora di adottare un approccio un po' più "sportivo". Farò una semplificazione ulteriore: non mi importerà più della forma esatta della curva frequenza-corrente, che per l'**integrate-and-fire* coinvolgeva un logaritmo. Assumerò, a grandi linee, che essa abbia un comportamento *threshold-linear*. Questo significa che al di sotto di una certa soglia di corrente, il *firing rate* in uscita è nullo, mentre al di sopra di essa ha un andamento lineare. Dopotutto, abbiamo derivato analiticamente un comportamento simile come limite per correnti di ingresso molto elevate. Ho anche insistito sull'effetto del tempo di refrattarietà assoluto, che tende a piegare e a far saturare la curva, ma per ora lo ignoreremo.

Per questa trattazione, che svolgerò in modo prevalentemente grafico per rendere i concetti più intuitivi, assumerò che la funzione frequenza-corrente sia descritta da una funzione *threshold-linear*. Eventualmente, aggiungerò una saturazione e vi farò riflettere sull'importanza di questo dettaglio. Mi sto quindi ponendo nel tipico quadro dei modelli di *firing rate*, dove si definisce un'equazione dinamica per il campo medio e una funzione di trasferimento ingresso-uscita, che viene assunta come *threshold-linear*, sigmoidale o a gradino.

### Connessione con il Machine Learning: Il Percettrone

Vi faccio notare che questa trattazione, a questo livello di astrazione, è identica a quella che si trova nel machine learning per il modello elementare del **percettrone**. Questi modelli, chiamati anche *rate models* o *firing rate models*, nel caso di un circuito *feed-forward*, sono matematicamente molto simili al percettrone. In entrambi i casi, abbiamo un'unità (nel nostro caso, una popolazione di neuroni) che riceve degli input, li somma, e produce un'uscita. Esiste una corrente minima (una soglia) al di sotto della quale il neurone non spara; al di sopra, sì. Nel caso più semplice, se la corrente è sotto soglia la frequenza è zero, se è sopra soglia i neuroni sparano.

Esiste un'interpretazione geometrica molto interessante. Consideriamo una rete *feed-forward* in cui l'input sinaptico a una popolazione post-sinaptica è generato da altre due popolazioni presinaptiche (due "blob"). Ciascun blob presinaptico spara con una frequenza diversa, $F_1$ e $F_2$. L'interazione è mediata da pesi sinaptici medi, che chiamo $W_1$ e $W_2$. Se consideriamo il sistema allo stato stazionario (*steady state*), immaginando che la costante di tempo sinaptica $\tau = 1/\beta$ sia quasi istantanea, possiamo ignorare la dinamica e scrivere che la corrente sinaptica totale è semplicemente la somma pesata delle frequenze in ingresso: $I_{sin} = W_1 F_1 + W_2 F_2$.

Questo è esattamente quanto accade nel percettrone, dove la relazione è puramente algebrica e non dinamica. Nel machine learning, non è sempre chiaro quale vantaggio porti introdurre una dinamica per le correnti sinaptiche, specialmente nella fase di "inferenza" (un termine che non amo, ma che oggi è comune), ovvero la propagazione degli input per calcolare un output. L'idea è sempre quella: fare una media pesata degli input ($F_1$ e $F_2$), darla in pasto a un'unità, e questa unità decide se la sua uscita è zero o non-zero. La formulazione è la stessa, con la differenza che nel nostro caso le unità non sono singoli neuroni, ma intere sottopopolazioni. Questo non è un problema concettuale, anzi, potrebbe essere verosimile che le proprietà computazionali di tipo percettrone nel cervello non siano legate a un singolo neurone, ma a gruppi di neuroni.

La condizione per cui la popolazione post-sinaptica si trova esattamente alla soglia di attivazione, $I_{sin} = I_{\theta}$, si traduce nell'equazione $W_1 F_1 + W_2 F_2 = I_{\theta}$. Nello spazio degli input $(F_1, F_2)$, questa è l'equazione di una retta. Le variabili indipendenti sono $F_1$ e $F_2$ (le "x" e "y" del piano), mentre $I_{\theta}$ è il termine noto, che definisce la soglia di eccitabilità. Questa retta divide il piano in due regioni: da un lato, dove $I_{sin} > I_{\theta}$, la popolazione è attiva; dall'altro, dove $I_{sin} < I_{\theta}$, è inattiva. I pesi $W_1$ e $W_2$ determinano la pendenza di questa retta di separazione. Nel gergo neurobiologico, questi pesi sinaptici possono cambiare grazie alla plasticità sinaptica. È stata proprio la biologia a ispirare l'idea che la pendenza di questa retta possa modificarsi, permettendo al sistema di risolvere problemi di classificazione lineare. Spero che questo, sebbene appesantito da nozioni di neurobiologia, risulti un concetto familiare e mostri il legame diretto con le fondamenta del machine learning.

## Reintroduzione della Dinamica Sinaptica

La differenza fondamentale con il modello del percettrone è che ora abbandoneremo l'ipotesi di una risposta istantanea. Nella realtà, la corrente sinaptica non risponde immediatamente, ma impiega un certo tempo a cambiare. Questa latenza, o inerzia, è dovuta alla cinetica di dissociazione del neurotrasmettitore e di chiusura dei recettori sinaptici, catturata dalla costante di tempo $\tau = 1/\beta$. I recettori possono essere molto veloci, ma non sono mai istantanei. Un'altra approssimazione che stiamo facendo, e che vedremo meglio domani, è che anche la descrizione della relazione tra corrente d'ingresso e frequenza d'uscita come istantanea potrebbe non essere del tutto adeguata.

Nonostante le sue semplificazioni, il percettrone, con i suoi due elementi chiave — la somma ponderata degli input e l'esistenza di una soglia (o reobase) nella curva frequenza-corrente — riesce a funzionare come un classificatore. Da qui, come sapete, nasce tutta la questione delle sue limitazioni: non può classificare pattern che non siano linearmente separabili, come nel celebre problema dello XOR (OR esclusivo). Questa incapacità contribuì al cosiddetto "primo inverno dell'intelligenza artificiale". La ricerca riprese vigore negli anni '80 con un'intuizione fondamentale: se un singolo percettrone è limitato, si possono usare più strati. Dopotutto, anche nel cervello le popolazioni neuronali non sono isolate, ma organizzate in gerarchie. Da questa idea discendono i *multilayer perceptrons*, il *deep learning* e tutto ciò che ne consegue.

## Ancorare i Modelli a Popolazione nella Biologia

Tornando alla biologia, questa caratterizzazione dell'attività neuronale in "blob" o popolazioni non è una novità. Già dagli anni '90, ricercatori come Douglas e Martin proposero dei modelli per descrivere l'attività dei neuroni della corteccia visiva raggruppando cellule con proprietà intrinseche e sinaptiche omogenee. Nel loro celebre modello del "circuito canonico corticale", identificavano diverse popolazioni: neuroni eccitatori degli strati 2-3, neuroni inibitori (raggruppati insieme), e neuroni piramidali degli strati 5-6. Questa suddivisione si basa sul fatto che strati diversi ricevono input distinti e hanno topologie sinaptiche differenti. Se un gruppo di neuroni ha una configurazione di connessioni diversa, non viene accorpato agli altri, ma trattato come una popolazione a sé stante.

Il modello di Douglas e Martin è più complicato di quello che esamineremo noi. Per i nostri scopi, sarà sufficiente considerare un caso più semplice, ovvero dividere tutti i neuroni in due grandi famiglie: quelli **eccitatori** e quelli **inibitori**.

### Il Modello a Due Popolazioni: Eccitatori e Inibitori

Utilizzando una notazione grafica, immaginiamo un "blob" che contiene un numero arbitrario di neuroni eccitatori, tutti omogenei per proprietà intrinseche e perché, quando emettono un'afferenza sinaptica, rilasciano glutammato. Non possiamo accorparli con altri neuroni che, oltre ad avere magari una curva frequenza-corrente diversa o una capacità di membrana differente, rilasciano un neurotrasmettitore diverso (come il GABA).

Pertanto, il modello più semplice di un modulo corticale prevede una popolazione di neuroni eccitatori (E) e una popolazione di neuroni inibitori (I). Le possibili connessioni tra queste popolazioni sono:
1.  **Connessioni ricorrenti eccitatorie (E → E):** Un neurone eccitatorio generico proietta ad altri neuroni dello stesso tipo. Vi ricorderete dall'anno scorso che un neurone piramidale ha una probabilità di circa il 10% di formare una sinapsi con un altro neurone piramidale vicino. In questa notazione a popolazioni, questo si traduce nel fatto che la popolazione E "auto-eccita" se stessa.
2.  **Proiezione eccitatoria verso gli inibitori (E → I):** La popolazione eccitatoria proietta, eccitandola, alla popolazione inibitoria.
3.  **Proiezione inibitoria di feedback (I → E):** La popolazione inibitoria proietta a sua volta sulla popolazione eccitatoria, inibendola.

In questo schema semplificato manca, ad esempio, l'auto-inibizione della popolazione I (I → I), che invece era presente nel modello di Douglas e Martin.

Il nostro obiettivo ora è scrivere le equazioni di campo medio per questo sistema. Avremo due campi medi da considerare: uno per la corrente sinaptica totale ricevuta da un neurone generico nella popolazione E, e un altro per la corrente ricevuta da un neurone generico nella popolazione I. Non è necessario scrivere equazioni per più neuroni all'interno della stessa popolazione, perché per definizione sperimentano tutti, in media, lo stesso campo. Le fluttuazioni potranno essere diverse da neurone a neurone, ma il campo medio sarà identico.

L'equazione che descrive l'evoluzione della corrente media è, nella sua forma, esattamente quella di prima. Diventiamo ora più specifici e scriviamo l'equazione per la corrente sinaptica, $I_{sin,E}$, che un neurone generico della popolazione eccitatoria (E) sperimenta. Il membro di sinistra dell'equazione differenziale dipenderà da una costante di tempo $\beta_E$, che riflette la cinetica dei recettori glutammatergici (es. AMPA e NMDA) presenti su questi neuroni.

$$
\frac{dI_{sin,E}}{dt} = -\beta_E I_{sin,E} + \dots
$$

Ora analizziamo i termini che contribuiscono a questa corrente. Guardando lo schema, vediamo che un neurone nella popolazione E riceve input da tre fonti: un input esterno, un input ricorrente dalla popolazione E stessa, e un input inibitorio dalla popolazione I.

1.  **Input Esterno ($I_{ext}$):** Questo è un termine di corrente aggiuntivo che potrebbe rappresentare un input sensoriale o una stimolazione sperimentale (es. optogenetica). Lo sommiamo direttamente.

2.  **Input Ricorrente Eccitatorio (E → E):** Seguendo la freccia che parte da E e torna su se stessa (indicata con un triangolo, a significare una sinapsi eccitatoria), aggiungiamo un termine positivo. Questo termine dipende dal numero di neuroni presinaptici ($N_E$), dalla probabilità di connessione ($C_{EE}$, da E a E), dall'efficacia sinaptica media ($W_{EE}$) e, crucialmente, dalla frequenza di sparo della popolazione presinaptica, che è la popolazione E stessa ($F_E$).

3.  **Input Inibitorio (I → E):** Seguendo la freccia che parte da I e arriva a E (indicata con un pallino, per una sinapsi inibitoria), aggiungiamo un termine negativo. Il segno meno deriva dal fatto che le sinapsi inibitorie (GABAergiche) hanno un potenziale di inversione molto basso. Sebbene abbiamo astratto via il termine esplicito della *driving force* ($E_{rev} - V$), il suo effetto è stato assorbito nel peso sinaptico $W$, rendendolo di fatto negativo per l'inibizione. Questo termine dipenderà dalla numerosità della popolazione inibitoria ($N_I$), dalla probabilità di connessione da I a E ($C_{EI}$), dall'efficacia sinaptica media ($W_{EI}$) e dalla frequenza di sparo della popolazione I ($F_I$).

Mettendo tutto insieme, l'equazione per la corrente media nella popolazione eccitatoria è:

$$
\frac{dI_{sin,E}}{dt} = -\beta_E I_{sin,E} + \beta_E (N_E C_{EE} W_{EE} F_E - N_I C_{EI} W_{EI} F_I + I_{ext})
$$

Ora dobbiamo fare lo stesso per un neurone generico nella popolazione inibitoria (I). Avremo un'altra equazione per la sua corrente media, $I_{sin,I}$, con una sua costante di tempo $\beta_I$, che potrebbe essere diversa se i recettori sulla popolazione I hanno una cinetica differente.

$$
\frac{dI_{sin,I}}{dt} = -\beta_I I_{sin,I} + \dots
$$

Guardando lo schema, vediamo che la popolazione I riceve un solo tipo di input sinaptico: quello proveniente dalla popolazione E.

1.  **Input Eccitatorio (E → I):** Seguendo la freccia da E a I, aggiungiamo un termine positivo. Questo dipenderà dalla numerosità della popolazione presinaptica ($N_E$), dalla probabilità di connessione ($C_{IE}$), dall'efficacia sinaptica media ($W_{IE}$) e dalla frequenza di sparo della popolazione presinaptica, che è $F_E$.

L'equazione completa per la popolazione inibitoria è quindi:

$$
\frac{dI_{sin,I}}{dt} = -\beta_I I_{sin,I} + \beta_I (N_E C_{IE} W_{IE} F_E)
$$

Quello che abbiamo fatto è di un'importanza capitale. Una rete che microscopicamente potrebbe essere estremamente complessa e richiedere una simulazione numerica pesante — con l'esplorazione di un vasto spazio di parametri per capire i suoi regimi di funzionamento — è stata ridotta a un sistema di due equazioni differenziali accoppiate, completate da due relazioni algebriche (le curve frequenza-corrente):

$$
F_E = \Phi_E(I_{sin,E})
$$
$$
F_I = \Phi_I(I_{sin,I})
$$

Con una trattazione di una semplicità matematica sconvolgente, abbiamo ottenuto un modello compatto. Come vedremo tra poco, questo sistema ci permetterà di capire il comportamento della rete senza dover eseguire simulazioni numeriche che potrebbero richiedere ore o l'accesso a supercalcolatori, ma semplicemente analizzando le proprietà di queste equazioni.

## Semplificazione della Notazione e Analisi del Sistema

Riprendendo il discorso, abbiamo ottenuto un sistema di due equazioni differenziali che descrivono l'evoluzione delle correnti medie nelle popolazioni eccitatoria e inibitoria, e due funzioni che mappano queste correnti nelle rispettive frequenze di sparo. Per semplicità, possiamo pensare che i neuroni delle due popolazioni abbiano la stessa curva frequenza-corrente, ma dobbiamo applicarla separatamente, poiché la corrente sperimentata da un neurone eccitatorio contribuisce al firing rate della popolazione E, mentre quella di un neurone inibitore contribuisce al firing rate della popolazione I.

Per rendere la notazione più maneggevole, introduciamo alcuni cambiamenti. Anziché scrivere "valore medio di I", useremo il simbolo $H$ per rappresentare il campo medio (es. $H_E$ e $H_I$), in onore dei fisici che per primi hanno usato questi metodi nello studio dei vetri di spin, dove H rappresenta il campo magnetico. Inoltre, sostituiamo $1/\beta$ con la costante di tempo $\tau$. Infine, accorpiamo tutti i parametri che descrivono le connessioni ($N$, $C$, $W$) in un unico termine, $J$, che rappresenta l'efficacia sinaptica complessiva tra due popolazioni. Con questa notazione, le nostre equazioni diventano molto più pulite. Per esempio, l'equazione per la popolazione E (che per semplicità chiamo E invece di $F_E$) diventa:

$$
\tau_E \frac{dH_E}{dt} = -H_E + J_{EE} E - J_{EI} I + I_{ext}
$$

E per la popolazione I:

$$
\tau_I \frac{dH_I}{dt} = -H_I + J_{IE} E
$$

Le frequenze E e I sono a loro volta funzioni dei rispettivi campi medi H:

$$
E = \Phi_E(H_E) \quad \text{e} \quad I = \Phi_I(H_I)
$$

Vedrete tra un attimo la potenza di questo formalismo.

### Caso 1: Una Singola Popolazione Eccitatoria Ricorrente

Iniziamo con il caso più semplice: un'unica popolazione omogenea di neuroni eccitatori connessi in modo ricorrente. Qui abbiamo una sola frequenza di sparo, che chiamo $E$, e un solo campo medio, $H$. L'equazione differenziale che descrive l'evoluzione di $H$ si scrive immediatamente. Usando la notazione con il punto per la derivata temporale ($\dot{H}$), abbiamo:

$$
\tau \dot{H} = -H + J \cdot E + I_{ext}
$$

Dove $J$ rappresenta l'efficacia sinaptica ricorrente all'interno della popolazione. Questa equazione va completata con la curva frequenza-corrente, che, come detto, assumiamo essere di tipo *threshold-linear*. La possiamo scrivere matematicamente in modo compatto usando la notazione $[x]_+$, che indica la "parte positiva" di $x$ (cioè, $[x]_+ = x$ se $x > 0$ e $[x]_+ = 0$ altrimenti):

$$
E = [\alpha(H - \theta)]_+
$$

Qui, $\alpha$ è un coefficiente che rappresenta la pendenza (il guadagno) della curva, e $\theta$ è la soglia di corrente al di sotto della quale la popolazione è silente.

### Il Comportamento Dinamico della Rete

Questa è un'equazione differenziale non lineare, perché $H$ compare all'interno della funzione non lineare $[ \cdot ]_+$. L'analisi di questo sistema dinamico ci può rivelare comportamenti computazionali molto interessanti, semplicemente studiando le proprietà di questa singola equazione. Un sistema dinamico come questo può essere simulato numericamente in modo estremamente semplice (molto più dell'integrate-and-fire), ma prima ancora, possiamo chiederci: esistono dei **punti di equilibrio**? Esistono cioè delle configurazioni di attività per cui, se la rete le raggiunge, vi rimane stabile?

Questo concetto è legato a fenomeni neurobiologici osservati, come l'**attività persistente**. In esperimenti di elettrofisiologia, si è visto che dopo la presentazione di uno stimolo, anche quando questo viene rimosso, l'attività in alcune aree corticali (come la corteccia prefrontale) può persistere per secondi. Questo suggerisce che la dinamica della rete, come quella di una pallina che si muove su un profilo di potenziale, possa avere dei punti di equilibrio stabili, delle "valli" in cui la pallina (lo stato della rete) può rimanere intrappolata.

Un equilibrio stabile implica che se l'attività della rete (es. 12 spike/s) viene leggermente perturbata da una fluttuazione, essa tenderà a ritornare al valore di equilibrio. Un equilibrio instabile, invece, è come la cima di una collina: una minima perturbazione farà "cadere" la pallina in uno degli stati stabili vicini. L'esistenza di più equilibri stabili (ad esempio, uno a bassa frequenza, "down state", e uno ad alta frequenza, "up state") è un meccanismo candidato per la memoria di lavoro (*working memory*).

Oltre all'esistenza di equilibri multipli, analizzeremo altri due fenomeni emergenti:

1.  **Velocità di risposta (Reaction Times):** La rete agisce come un filtro. Cambiando la connettività $J$ (ad esempio, tramite plasticità sinaptica), possiamo alterare la relazione ingresso-uscita della rete, e in particolare la rapidità con cui risponde a un cambiamento dell'input.
2.  **Amplificazione:** Sotto certe condizioni, la rete può amplificare un segnale esterno, proprio come un amplificatore operazionale in elettronica.

Questi tre fenomeni — equilibri multipli, filtraggio e amplificazione — possono essere compresi in modo profondo analizzando questa semplice equazione.

### Analisi del Sistema: Amplificazione e Velocità di Risposta

Per iniziare l'analisi, concentriamoci sull'amplificazione e sulla velocità di risposta della rete. Sappiamo, da discipline come l'elettronica, che questi due aspetti sono spesso legati da un compromesso (*trade-off*): un sistema con un guadagno molto elevato tende ad avere una banda passante limitata, ovvero risponde lentamente. Vediamo se un principio simile emerge qui.

Il feedback positivo, fornito dalle connessioni ricorrenti eccitatorie, è il meccanismo alla base dell'amplificazione, proprio come negli amplificatori operazionali. Per analizzare questo effetto, facciamo un'ipotesi di lavoro: supponiamo di trovarci in un regime in cui il campo medio $H$ è sempre al di sopra della soglia $\theta$. Questa assunzione ci permette di liberarci della non-linearità della funzione "parte positiva", che diventa semplicemente una retta:

$$
E = \alpha(H - \theta) \quad (\text{per } H > \theta)
$$

Sostituendo questa espressione lineare nell'equazione differenziale, otteniamo:

$$
\tau \dot{H} = -H + J[\alpha(H - \theta)] + I_{ext}
$$

Ora possiamo riarrangiare i termini per raggruppare tutte le occorrenze di $H$:

$$
\tau \dot{H} = -H + J\alpha H - J\alpha\theta + I_{ext}
$$
$$
\tau \dot{H} = -(1 - J\alpha)H + (I_{ext} - J\alpha\theta)
$$

Questa è la nostra cara, vecchia equazione differenziale lineare del primo ordine, la cui dinamica è interamente determinata dal coefficiente che moltiplica $H$. Sappiamo che il sistema è stabile solo se questo coefficiente è negativo. Affinché $-(1 - J\alpha)$ sia negativo, la quantità $(1 - J\alpha)$ deve essere positiva. Questo impone una condizione critica:

$$
J\alpha < 1
$$

Se il prodotto dell'accoppiamento ricorrente $J$ e del guadagno neuronale $\alpha$ dovesse diventare maggiore di 1, il termine che moltiplica $H$ diventerebbe positivo. La soluzione dell'equazione omogenea associata sarebbe un'esponenziale crescente, portando a un'esplosione dell'attività. Questo è un modello matematico per la cosiddetta *runaway excitation*, un'attività incontrollata simile a una crisi epilettica. È affascinante come da una semplice equazione possiamo derivare una condizione precisa per la stabilità della rete. Se l'efficacia sinaptica eccitatoria ricorrente aumenta a dismisura, la rete può diventare instabile.

#### Meccanismo di Amplificazione

Assumendo di essere nel regime stabile ($J\alpha < 1$), analizziamo come la rete risponde a un input costante. Per trovare lo stato stazionario ($H_{\infty}$), poniamo la derivata $\dot{H}$ a zero e risolviamo per $H$:

$$
0 = -(1 - J\alpha)H_{\infty} + (I_{ext} - J\alpha\theta)
$$
$$
H_{\infty} = \frac{I_{ext} - J\alpha\theta}{1 - J\alpha}
$$

Guardiamo il denominatore: $1 - J\alpha$. Finché $J$ è positivo, questo termine è minore di 1. Dividere per un numero più piccolo di 1 equivale a moltiplicare per un numero più grande di 1. Man mano che il prodotto $J\alpha$ si avvicina a 1, il denominatore diventa sempre più piccolo, e il valore di $H_{\infty}$ (e quindi la frequenza di uscita $E_{\infty}$) viene amplificato in modo non lineare. Abbiamo creato un amplificatore biologico.

#### Il Trade-off: Guadagno vs. Velocità

Tuttavia, questa amplificazione ha un costo. Riscriviamo la nostra equazione differenziale per isolare la "vera" costante di tempo del sistema. Dividendo entrambi i membri per $(1 - J\alpha)$, otteniamo:

$$
\left(\frac{\tau}{1 - J\alpha}\right) \dot{H} = -H + \frac{I_{ext} - J\alpha\theta}{1 - J\alpha}
$$

Questa è la forma standard $\tau_{eff} \dot{H} = -H + H_{\infty}$. Vediamo che la costante di tempo effettiva del sistema non è più $\tau$, ma:

$$
\tau_{eff} = \frac{\tau}{1 - J\alpha}
$$

Poiché il denominatore è minore di 1, la costante di tempo effettiva $\tau_{eff}$ è sempre **maggiore** della costante di tempo sinaptica originale $\tau$. Ciò significa che la rete impiega più tempo per raggiungere il suo stato stazionario. Man mano che aumentiamo l'amplificazione (facendo avvicinare $J\alpha$ a 1), la costante di tempo effettiva diventa sempre più grande, tendendo all'infinito. La rete diventa sempre più lenta.

Questo è un *trade-off* fondamentale e inevitabile: l'accoppiamento ricorrente permette di amplificare i segnali, ma al prezzo di una risposta più lenta. Se un sistema visivo usasse questo meccanismo per amplificare il debole segnale di una tigre in lontananza, un'amplificazione troppo alta potrebbe rendere il sistema così lento da non riuscire a reagire al suo rapido balzo. Ancora una volta, un'intuizione profonda sulla funzione e sui limiti computazionali di un circuito neuronale emerge da una manciata di passaggi matematici.

### Visualizzando il Trade-off tra Amplificazione e Velocità

Per rendere questi concetti più concreti, possiamo esaminare i risultati di una semplice simulazione numerica di questa equazione differenziale. Nel primo scenario, forniamo alla rete una corrente esterna a gradino (*step*). Osservando le risposte del sistema per valori crescenti dell'accoppiamento ricorrente $J$, notiamo due effetti distinti. Innanzitutto, all'aumentare di $J$, l'ampiezza della risposta allo stato stazionario cresce in modo significativo e non lineare, confermando l'effetto di amplificazione previsto dalla nostra analisi (poiché $J$ compare al denominatore dell'espressione per $H_{\infty}$).

Tuttavia, osservando la dinamica, vediamo che più grande è $J$, più la risposta è "lenta", ovvero impiega più tempo per raggiungere il suo valore di regime. Questo effetto diventa ancora più evidente se normalizziamo tutte le risposte al loro valore di picco. In questo modo, eliminiamo l'effetto dell'amplificazione e isoliamo la sola dinamica temporale. Il risultato è chiaro: all'aumentare di $J$, la curva di salita (e di discesa) diventa progressivamente più lenta, a dimostrazione del fatto che la costante di tempo effettiva del sistema, $\tau_{eff}$, sta aumentando.

Un secondo esempio illustra lo stesso principio con un input che varia arbitrariamente nel tempo (ad esempio, una somma di sinusoidi). Quando l'accoppiamento ricorrente $J$ è nullo, l'uscita è semplicemente una versione filtrata (passa-basso) dell'ingresso, con la costante di tempo intrinseca $\tau$. Man mano che aumentiamo $J$, la risposta viene amplificata. Tuttavia, per valori di $J$ elevati, la rete perde la capacità di seguire le fluttuazioni rapide dell'input. L'uscita diventa una versione molto amplificata ma anche molto "lisciata" (*smoothed*) dell'ingresso, evidenziando il suo comportamento da filtro passa-basso la cui frequenza di taglio diminuisce all'aumentare del guadagno.

È importante ricordare che questa analisi si basa sull'ipotesi che la corrente esterna sia sufficientemente forte da mantenere il campo medio $H$ al di sopra della soglia $\theta$. Se l'input è troppo basso, la rete rimane silente e l'amplificazione non avviene. Inoltre, bisogna fare attenzione a non aumentare $J$ al punto da superare la soglia di stabilità ($J\alpha \ge 1$), altrimenti la soluzione diverge, portando a un overflow numerico nella simulazione, che corrisponde all'attività epilettiforme nel nostro modello.

### Il Caso Critico: L'Integratore Neurale Perfetto

Esiste un caso intermedio, un punto critico esattamente al confine tra stabilità e instabilità, che dà origine a una computazione estremamente interessante: l'**integrazione temporale**. Questo concetto è particolarmente rilevante per spiegare un fenomeno osservato nel sistema oculomotorio, che controlla i movimenti degli occhi.

Nel tronco encefalico (*brainstem*), alcuni neuroni codificano i comandi per i movimenti oculari saccadici attraverso brevi raffiche di spike (*bursts*). Ogni volta che l'occhio si muove, questi neuroni emettono un "burst". A valle, altri neuroni mostrano un'attività tonica (sostenuta) la cui frequenza di sparo codifica la posizione attuale dell'occhio. Sperimentalmente, si osserva che dopo ogni burst dei neuroni di comando, la frequenza di sparo di questi neuroni "a valle" aumenta di un certo livello e poi **persiste** a quel nuovo livello. Se arriva un altro burst, la frequenza sale ulteriormente e si stabilizza di nuovo. Se il movimento è nella direzione opposta, la frequenza diminuisce.

Questo comportamento suggerisce l'esistenza di un meccanismo che **integra** nel tempo i burst di comando. Concettualmente, è come se ci fosse un condensatore che accumula la carica fornita da ogni burst e mantiene il suo potenziale (che rappresenta la posizione dell'occhio), perdendolo molto lentamente o per nulla. Un'integrazione perfetta, senza perdite (*leak*), è difficile da immaginare a livello di singola cellula. Tuttavia, ci si può chiedere: è possibile che questa funzione emerga a livello di popolazione, proprio dall'architettura di una rete eccitatoria ricorrente?

Torniamo alla nostra equazione e consideriamo il caso speciale in cui l'efficacia sinaptica è finemente regolata (*fine-tuned*) per assumere un valore molto preciso. Immaginiamo che qualche meccanismo omeostatico mantenga il prodotto $J\alpha$ esattamente uguale a 1. Questo è il punto critico che prima ci preoccupava. Se sostituiamo $J\alpha = 1$ nella nostra equazione dinamica:

$$
\tau \dot{H} = -(1 - J\alpha)H + (I_{ext} - J\alpha\theta)
$$
$$
\tau \dot{H} = -(1 - 1)H + (I_{ext} - \theta)
$$
$$
\tau \dot{H} = I_{ext} - \theta
$$

Il termine $-H$, che rappresenta la "perdita" (*leak*), è magicamente scomparso! L'equazione è diventata quella di un **integratore perfetto**. La variazione nel tempo del campo medio $\dot{H}$ è ora direttamente proporzionale all'input (meno una costante). Integrando entrambi i membri rispetto al tempo, otteniamo:

$$
H(t) = H(0) + \frac{1}{\tau} \int_0^t (I_{ext}(s) - \theta) ds
$$

Il campo medio $H$ (e quindi la frequenza di sparo della popolazione) accumula nel tempo l'input esterno. La critica ovvia a questo modello è l'ipotesi del *fine-tuning*: è biologicamente plausibile che l'efficacia sinaptica sia regolata con una precisione così assoluta da annullare esattamente la perdita? Probabilmente no. Anche nei sistemi artificiali, ottenere un *fine-tuning* perfetto dei parametri è quasi impossibile a causa della variabilità dei componenti. Tuttavia, questo modello fornisce un'ipotesi computazionale potente su come una rete neuronale possa, almeno in approssimazione, realizzare una funzione di memoria a breve termine come l'integrazione.

### Equilibri Multipli e Memoria di Lavoro

Il terzo e ultimo fenomeno che analizziamo riguarda i **punti di equilibrio**. Modificando il valore dell'accoppiamento ricorrente $J$, è possibile alterare qualitativamente il numero e la natura di questi equilibri. Questo concetto è strettamente legato a un'importante funzione cognitiva: la **memoria di lavoro** (*working memory*). Pensate all'esempio, che ho fatto più volte, di dover tenere a mente per un breve periodo un'informazione, come una carta da gioco che vi viene mostrata (es. l'asso di cuori). Questo tipo di memoria a breve termine non può basarsi su meccanismi di plasticità strutturale, come la sintesi di nuove proteine o la crescita di spine dendritiche, che richiedono minuti od ore per manifestarsi.

Si ipotizza invece che il substrato neurale di questa funzione sia un'**attività auto-sostenuta** o di **riverberazione** all'interno di una popolazione neuronale, resa possibile proprio dalle connessioni ricorrenti. L'idea è che, se devo ricordare un numero di telefono senza poterlo scrivere, continuo a ripeterlo mentalmente. Un meccanismo analogo potrebbe avvenire nella corteccia inferotemporale, dove una popolazione di neuroni eccitatori mantiene attiva la rappresentazione di uno stimolo anche dopo la sua scomparsa. Se, attraverso l'apprendimento, le sinapsi ricorrenti vengono potenziate, la rete può acquisire la capacità di "riverberare", mantenendo l'informazione attiva.

Una rete con un'efficacia sinaptica ricorrente sufficientemente elevata può esibire esattamente questo comportamento. A differenza delle aree sensoriali primarie dove l'attività tende a seguire l'input, nelle aree associative come la corteccia prefrontale o inferotemporale si osserva un fenomeno diverso:
1.  In assenza di stimolo, l'attività è bassa o nulla (stato di base).
2.  Durante la presentazione dello stimolo, l'attività aumenta.
3.  Crucialmente, quando lo stimolo viene rimosso, l'attività **non torna allo stato di base, ma persiste** a un livello elevato (ad esempio, a 5 spike/s o più).

Possiamo visualizzare questa dinamica utilizzando di nuovo l'analogia della pallina in un profilo di potenziale. In questo caso, il profilo ha **due valli**, ovvero due punti di equilibrio stabili (o **attrattori**): uno a bassa attività e uno ad alta attività, separati da una "collina" (un punto di equilibrio instabile). Lo stimolo esterno agisce come una "spinta" che muove la pallina dalla valle a bassa attività, le fa superare la barriera, e la fa cadere nella valle ad alta attività. Una volta rimosso lo stimolo (la spinta), la pallina rimane intrappolata nel nuovo bacino di attrazione, mantenendo così in "memoria" lo stimolo passato. Per "resettare" la memoria, sarebbe necessario un input inibitorio che "spinga" la pallina fuori dalla valle ad alta attività e la riporti allo stato di base.

### Dalla Simulazione Microscopica alla Teoria

Questo comportamento non è solo un'astrazione teorica. In una simulazione microscopica di una rete di neuroni *integrate-and-fire* con connessioni ricorrenti casuali, possiamo osservare l'emergere di questo fenomeno. Aumentando progressivamente l'efficacia sinaptica (il parametro $J$ nel nostro modello, o il $g_{max}$ sinaptico nella simulazione):
-   Per valori di $J$ bassi: La rete risponde allo stimolo (la barra grigia nel raster plot), ma la sua attività cessa immediatamente non appena lo stimolo viene rimosso.
-   Per valori intermedi di $J$: La rete amplifica la risposta. Dopo la rimozione dello stimolo, l'attività persiste per un po' ma poi decade, suggerendo che l'attrattore ad alta attività non è ancora sufficientemente stabile.
-   Per valori di $J$ alti: Dopo la rimozione dello stimolo, l'attività della rete si stabilizza a un livello elevato e persiste indefinitamente, esattamente come osservato negli esperimenti di memoria di lavoro.

Il punto di equilibrio qui non significa che la rete è silente. Al contrario, è un equilibrio dinamico: i singoli neuroni continuano a sparare in modo irregolare, ma la frequenza media della popolazione rimane statisticamente costante nel tempo. La grande forza del modello di campo medio è che ci permette di calcolare analiticamente il valore critico di $J$ necessario per creare questo sistema **bistabile**, ovvero con due punti di equilibrio stabili.

Per fare ciò, dobbiamo analizzare i punti di equilibrio dell'equazione differenziale. Un punto di equilibrio è uno stato stazionario ($H_{SS}$) in cui la derivata temporale è nulla ($\dot{H}=0$). A differenza di prima, ora non faremo alcuna ipotesi sul fatto di essere sopra o sotto soglia. Affronteremo direttamente la non-linearità dell'equazione:

$$
0 = -H_{SS} + J \cdot E + I_{ext}
$$

Poiché $E = \Phi(H_{SS})$, otteniamo un'equazione algebrica implicita per $H_{SS}$:

$$
H_{SS} = J \cdot \Phi(H_{SS}) + I_{ext}
$$

Non possiamo semplicemente isolare $H_{SS}$ con passaggi algebrici. Tuttavia, possiamo risolvere questa equazione graficamente. Le soluzioni (i punti di equilibrio) sono i valori di $H_{SS}$ per cui il grafico della funzione a sinistra, $y_1 = H_{SS}$ (che è la bisettrice del primo e terzo quadrante), interseca il grafico della funzione a destra, $y_2 = J \cdot \Phi(H_{SS}) + I_{ext}$ (che è la curva frequenza-corrente, scalata da $J$ e traslata da $I_{ext}$). Come vedremo domani, il numero di intersezioni tra queste due curve ci dirà quanti punti di equilibrio esistono nel sistema.

### La Necessità della Saturazione per la Bistabilità

Per ottenere questo comportamento bistabile, è necessario un ingrediente aggiuntivo: la curva frequenza-corrente deve avere una **saturazione**. Una funzione *threshold-linear* che cresce all'infinito non permette la creazione di un secondo punto di equilibrio stabile ad alta attività. La saturazione, che biologicamente è dovuta all'effetto del periodo refrattario, è matematicamente cruciale. Lo vedremo meglio domani, ma per ora, volevo mostrarvi quanto sia relativamente semplice riprodurre i risultati della simulazione microscopica utilizzando il modello di campo medio.

Questa è la simulazione del modello di *firing rate*. All'aumentare del valore di $J$:
-   Per $J$ basso, vediamo una risposta amplificata che decade rapidamente.
-   Per un valore intermedio di $J$, la "coda" della risposta diventa sempre più lenta (la costante di tempo effettiva diverge). Questa dinamica è molto simile al caso intermedio della simulazione microscopica, in cui l'attività persisteva solo per un breve periodo.
-   Per un valore di $J$ sufficientemente alto (ma non troppo, per evitare l'instabilità epilettiforme), l'attività persistente viene mantenuta indefinitamente dopo la rimozione dello stimolo. Per uscire da questo stato "bloccato" e tornare allo stato di base, sarebbe necessario un forte input inibitorio per iperpolarizzare i neuroni.

L'unica differenza tra queste tre simulazioni è il valore del parametro $J$.

### Analisi dei Punti di Equilibrio: L'Approccio Grafico

Domani riprenderemo questa equazione algebrica per i punti di equilibrio ($H_{SS}$) e la studieremo in dettaglio. Come detto, non è possibile risolverla analiticamente isolando $H_{SS}$. Un'equazione implicita della forma $x = f(x)$ può essere risolta in due modi. Il primo è numerico, utilizzando algoritmi come il metodo di Newton-Raphson per trovare gli zeri della funzione $g(x) = f(x) - x$. Questi metodi possono trovare più di una soluzione, se esistono.

Il secondo modo, più intuitivo, è quello **grafico**. Questo approccio, che sicuramente ricorderete dalla geometria analitica, consiste nel trasformare una singola equazione in un sistema di due funzioni, e trovare i punti in cui i loro grafici si intersecano. Nel nostro caso, l'equazione $H_{SS} = J \cdot \Phi(H_{SS}) + I_{ext}$ può essere scomposta in:

1.  $y_1 = H_{SS}$
2.  $y_2 = J \cdot \Phi(H_{SS}) + I_{ext}$

Le soluzioni, ovvero i punti di equilibrio del nostro sistema, corrispondono ai valori di $H_{SS}$ dove $y_1 = y_2$, cioè dove i due grafici si incontrano.

Il primo grafico, $y_1 = H_{SS}$, è semplicemente la bisettrice del primo e terzo quadrante del piano $(H_{SS}, y)$. Il secondo grafico, $y_2$, è la nostra curva frequenza-corrente $\Phi(H_{SS})$, ma modificata: la sua pendenza è scalata dal fattore $J$, e la curva è traslata verticalmente dall'input esterno $I_{ext}$.

Studiando come queste due curve si intersecano al variare del parametro $J$, potremo capire come e perché emergono uno, due o tre punti di equilibrio, e quindi come la rete possa passare da un comportamento di semplice amplificazione a un comportamento di memoria bistabile. Faremo questa analisi grafica domani.

