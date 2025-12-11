% Processed Transcript: ../course_website/ModellingNeuralSystems/overheads/transcripts/Class3_raw.md
% Date: 2025-12-10

## Introduzione

Ok, ma qui non vedete niente. L'idea di oggi è letteralmente quella di giocare assieme con NEURON, perlomeno a un livello molto elementare. Per "elementare" intendo che non vi mostrerò simulazioni di modelli multicompartimentali con morfologia complessa importata da esperimenti di ricostruzione 3D. Questo perché, in primo luogo, non ne sono un grande esperto e, in secondo luogo, perché la base che spero possa risuonare con la vostra esperienza è quella di paragonare questo tipo di simulazioni con quanto avete potuto magari orecchiare o vedere durante il corso dell'anno scorso con me. In quell'occasione, vi ho mostrato qualche esempio di codice risolto con Eulero o con altri metodi numerici, e vi ho fatto vedere il modello di Hodgkin-Huxley implementato scrivendo le equazioni e provando a integrarle numericamente. Se volete, posso rimandarvi, o spero possiate ritrovare, il codice, il sito web e il repository GitHub in cui sono ancora presenti quei notebook. Vorrei che aveste l'idea di quanto in teoria sia molto più facile usare questo simulatore, un simulatore standard come NEURON, invece di scriversi il codice da sé. Non ci vuole un genio a farlo.

### Vantaggi e Svantaggi dell'Uso di un Simulatore Standard

D'altro canto, qui, come vedremo, la risoluzione numerica delle equazioni e una parte della descrizione del sistema a basso livello vi sono precluse. Mentre in precedenza eravate voi a scrivere le equazioni, a controllare il metodo numerico, a capire quali fossero le variabili di stato e, volendo iniettare una corrente, sapevate dove inserirla, per esempio nel "right-hand side" di un'equazione, nell'equazione del bilancio della carica, la parte destra era la corrente iniettata. Adesso, invece, lo vedrete, è un pochino nascosto. Questo è un pregio quando si ha un modello molto complicato o un modello di rete molto complessa, ma ha degli svantaggi: voi non sapete esattamente se le cose stanno funzionando oppure no. Un motto che ho fatto mio anni fa è "garbage in, garbage out". A meno che non ci sia qualche errore di compilazione o di sintassi, qualcosa da una simulazione esce sempre: vedete delle tracce, vedete dei numeri, vedete dei valori. Come fate a sapere che il risultato è autentico? Non è così banale farlo. In teoria, dovrebbe essere un pochino più facile sviluppare un'intuizione quando il codice lo scrivete da zero voi.

### Storia e Sviluppo del Simulatore NEURON

NEURON è un simulatore di riferimento che ha avuto origine negli anni Ottanta. È, infatti, il risultato di una stratificazione di approcci e tecniche, alcune delle quali anche molto vecchie, molto antiquate, ma non dal punto di vista numerico. L'equazione del cavo, che forse ricordate, era un'equazione alle derivate parziali. Descrivere e risolvere un sistema distribuito, che qui è già stato discretizzato in modo efficace nello spazio e nel tempo (quindi ovviamente c'è il tempo, ma c'è anche lo spazio), richiede una maggiore accortezza. Non ve ne parlerò, comunque, come per l'equazione della diffusione, se vi ricordate l'anno scorso vi dicevo che l'equazione del cavo ha la stessa struttura di un'equazione della diffusione o è simile anche all'equazione della propagazione delle onde. Essendo un'equazione alle derivate parziali sia in tempo ($T$) che in spazio ($X$), il $\Delta T$ e il $\Delta X$, quindi il passo di discretizzazione temporale e spaziale, non possono essere scelti a caso; devono essere uno in funzione dell'altro, altrimenti si incorrono in problemi seri di stabilità numerica. Questo non significa che il computer esploda, ma che la soluzione ottenuta non è quella corretta. Quando la soluzione è corretta, lo si può sapere; figuratevi quando non c'è una soluzione analitica con cui confrontare.

Questo simulatore, NEURON, è cresciuto particolarmente grazie a due personaggi dell'Università di Yale, in particolare Michael Hines, che per primi hanno consolidato un algoritmo numerico molto robusto e accurato per la risoluzione dell'equazione del cavo. Dato che in NEURON tutto è un cavo, perché i neuroni sono assunti essere cavi, questo è stato considerato un buon riferimento. Non è l'unico simulatore, ma è quello che è sopravvissuto più a lungo.

### Evoluzione dell'Interfaccia di NEURON: Da H-Hoc a Python

NEURON è stato scritto in C++. Fino al 2005-2008, per programmarlo si usava un linguaggio chiamato H-Hoc. Questa era una programmazione ad alto livello, in cui si scriveva uno script che descriveva il modello (non si scriveva $C \frac{dV}{dt} = \dots$, ma si diceva: "qui c'è un soma, qui c'è un pezzettino di dendrite, attacca il soma al dendrite"), quindi a un livello di astrazione molto più elevato. Non ricordo cosa significhi la parte "O-C" di H-Hoc, ma la "H" è l'iniziale del cognome di uno dei tizi. Era una cosa oscena, con una sintassi terribile. Nel 2005, 2006, 2007, un ricercatore canadese molto in gamba che conosco bene, Ilif Müller, insieme ad altre persone, ha creato un'interfaccia per Python. Quindi, se si usa Python, si può chiamare NEURON come se fosse una libreria, e questo rende l'utilizzo di NEURON molto più facile.

### Espansione di NEURON con Meccanismi Biofisici Personalizzati

Se si desidera espandere NEURON, è importante sapere che il simulatore include di default l'equazione di Hodgkin-Huxley e il modello di corrente di *leak*, chiamato "meccanismo passivo". Questa corrente passiva è considerata una conduttanza che non dipende dal potenziale o dal tempo.

Quando si vuole creare un soma, in NEURON, tutto è un cilindro, quindi non è possibile creare un soma sferico. Tuttavia, un soma può essere rappresentato come un singolo compartimento, il che significa che è un'unica variabile di stato, $V_1$. Per la maggior parte, con un'eccezione, ci concentreremo su modelli a singolo compartimento.

Di default, NEURON include la capacità di membrana ($C_m$) e la sua derivata rispetto al tempo. Quindi, quando si definiscono i parametri geometrici come il diametro e la lunghezza di una sezione, questi vengono utilizzati per determinare il valore delle variabili, che sono normalmente specificate per unità di superficie. Specificando la geometria, NEURON calcola automaticamente il valore corretto della capacità nelle equazioni.

Successivamente, si può dire a NEURON di "inserire" il meccanismo passivo, e lui aggiungerà questa corrente alla parte destra dell'equazione. Se si chiede di inserire il meccanismo HH (spesso chiamato semplicemente "H" per fantasia), NEURON include il modello di Hodgkin-Huxley con i parametri dell'articolo del 1952. Questi parametri non sono necessariamente adatti per un modello di cellula corticale di ratto o umana, come vedremo più avanti, ma sono utili per iniziare. Quando si usa il comando "insert", NEURON non solo aggiunge le correnti voltaggio-dipendenti di sodio e potassio, ma tiene anche conto delle equazioni che descrivono le variabili di gating ($m^3h$, $n^4$) e le conduttanze ($g_{Na}$ e $g_K$). NEURON gestisce automaticamente le equazioni differenziali per $m$, $n$ e $h$, senza che l'utente debba fare nulla, se non eventualmente specificare le condizioni iniziali, tipicamente solo per la variabile di stato del potenziale di membrana.

Data questa facilità d'uso, sorge spontanea la domanda: perché non utilizzare NEURON per aggiungere altre correnti voltaggio-dipendenti? Ad esempio, esistono correnti di potassio transitorie, che presentano un'inattivazione con una specifica descrizione (che esamineremo in seguito). Nonostante sia una corrente di potassio, la sua conduttanza è diversa da quella del potassio *delay rectifier* o persistente, che non ha inattivazione come il sodio. Questa corrente transitoria ha altre variabili di gating (ad esempio, una variabile di inattivazione che potremmo chiamare $H$). NEURON permette di inserire un'altra corrente con un solo comando, oppure una corrente di sodio persistente, che è stata misurata sperimentalmente e corrisponde a canali sodio voltaggio-dipendenti che non si inattivano. Esistono anche altre correnti, come le correnti $I_H$, attivate dall'iperpolarizzazione, che sono canali permeabili sia a cariche positive che negative.

Poiché è così semplice inserire questi meccanismi, l'appetito vien mangiando. Tuttavia, questi meccanismi aggiuntivi non sono inclusi di default in NEURON. Per ognuno di essi, l'utente può creare un file `.mod` (che sta per "modello"), la cui sintassi, che vedremo, è in un altro linguaggio di programmazione chiamato Model Description Language. Non è particolarmente *user-friendly*.

Quello che si fa, e forse ve l'ho scritto su Teams, è consultare siti come ModelDB (database di modelli), dove si può letteralmente "fare shopping" di meccanismi. Ad esempio, se serve una corrente di potassio che si inattiva ($I_A$), la si può scaricare. Tra il dire e il fare c'è di mezzo il mare: se si ha un esperimento e si vuole modellare una particolare cellula, in teoria si dovrebbero avere dati sperimentali come quelli che hanno permesso a Hodgkin e Huxley di fittare le curve di attivazione e inattivazione ($m_\infty$, $h_\infty$, $\tau$). Se questi dati non sono disponibili, la cosa migliore è vedere se qualcuno ha descritto la stessa corrente, magari in un'altra specie. Si può quindi fare un "mix", che non è l'ideale, ma è meglio che non fare nulla. Ad esempio, si può prendere una corrente $I_A$ identificata nel ratto se non è ancora stata identificata nell'uomo.

Esistono sforzi interessanti in questa direzione, come Channelpedia, un database che in teoria raccoglie informazioni per tutti i canali ionici (potassio, sodio, calcio, cloro, TRP, ecc.). Per ognuno di questi, in teoria, esiste una sorta di database con tracce sperimentali. Ad esempio, per il canale NAV 1.2, si possono trovare articoli che lo utilizzano, tracce e cinetiche, sebbene a volte solo per specifiche specie (es. mouse). Spesso, viene indicata l'implementazione per il simulatore NEURON e la possibilità di scaricare il file `.mod`. Ad esempio, "mcoh25" potrebbe riferirsi a un tipo di cellula CHO (Chinese Hamster Ovary cell). Questo significa che, invece di usare un neurone, i ricercatori hanno utilizzato questa linea cellulare (immortalizzata e originata da un criceto cinese) e, tramite metodiche di trasfezione o trasduzione, l'hanno fatta esprimere solo quel canale. Questo permette di studiarne l'attivazione e l'inattivazione elettrofisiologicamente con una pipetta. Se il file `.mod` esiste già, lo si può scaricare. Siti come ModelDB (.science, precedentemente .org) contengono modelli associati ad articoli scientifici. Il vantaggio di NEURON è proprio la possibilità di "fare shopping" di file `.mod` e combinarli in un modello, cosa che richiederebbe di programmare singole equazioni se fatto a mano.

### Compilazione di Meccanismi Personalizzati e Interfaccia Utente

La complicazione è che, essendo NEURON un programma compilato scritto in C o C++, anch'essi linguaggi compilati, c'è una procedura intermedia che richiede di ricompilare NEURON incorporando i nuovi file `.mod`. Questo genera un nuovo eseguibile che deve essere utilizzato, poiché incorpora i meccanismi aggiuntivi. Questa premessa è necessaria per comprendere il codice che verrà mostrato.

NEURON ha anche una sua interfaccia grafica, che appare molto in stile anni '80-'90 (più '90 che '80). Personalmente non la trovo gradevole; è un retaggio di architetture e sistemi di finestre non moderni, tipici di Unix. Per questi esercizi e demo, ci affideremo esclusivamente a Python, utilizzando le sue librerie per il plotting delle tracce generate. Questo richiede di istruire NEURON, tramite Python, a inserire le tracce prodotte dalla simulazione in un vettore o array di Python, in modo che siano disponibili in Python e non rimangano confinate all'interno di NEURON. Vedrete questo in dettaglio più avanti.

### Configurazione di un Modello a Singolo Compartimento in NEURON con Python

Il primo esercizio, o tutorial, riguarda un singolo compartimento, il cui equivalente circuitale include non solo la capacità di membrana, ma anche la componente di *leak*, assumendo che questa resistenza non sia voltaggio-dipendente, tipica delle proprietà passive.

La configurazione è abbastanza semplice. A parte l'importazione, che è una forma standard in Python per importare una libreria o un sottoinsieme di essa (forse sarebbe più corretto dire un *namespace*), in modo che tutti i comandi, funzioni o oggetti all'interno di questa libreria diventino disponibili. La variabile `n` rappresenta il punto radice di questa libreria.

Per creare una singola sezione, è sufficiente un comando come `Soma = n.Section(name='soma')`. `Soma` è una variabile Python a cui viene assegnato l'output di questo comando. Questo comando è probabilmente un costruttore di oggetti, il che ha perfettamente senso nella programmazione orientata agli oggetti. Anche per chi non ha familiarità con la programmazione a oggetti, Python o NEURON, si può semplicemente accettare che questo comando crei una sezione. Il nome `'soma'` è un nome interno che si sarebbe potuto chiamare "Pippo". La variabile `Soma` in Python è un oggetto con variabili e metodi. Una variabile si chiama `L`, per la lunghezza. Dato che tutto in NEURON è un cilindro, questa sezione (che avrei potuto chiamare "Pippo") avrà una lunghezza definita da `Soma.L = 10` (per default, per convenzione, in micrometri). Allo stesso modo, `Soma.diam = 10` assegna il diametro. Si ottiene così un cilindro tozzo e basso, poiché non è possibile creare una sfera. Se il diametro è 10, moltiplicandolo per $\pi$ e poi per la lunghezza (10), si ottiene la superficie laterale del cilindro ($2\pi r \cdot h = \text{diametro} \cdot \pi \cdot h$), che è dell'ordine di 300 micrometri quadrati (circa 314 $\mu m^2$). Tutte le grandezze saranno scalate in base a questa quantità.

Il comando `insert` è un metodo degli oggetti, una funzione propria e incapsulata nell'oggetto stesso, che agisce internamente sull'oggetto. Quindi, con `Soma.insert('pas')`, in una sola volta, si istruisce NEURON ad aggiungere il meccanismo passivo all'equazione differenziale (che non si vedrà mai perché è nascosta). Questo crea un ulteriore oggetto, `Soma.g_pas`, che rappresenta la conduttanza di *leak* ($\bar{g}_{pas}$) e può essere modificato. È importante poterlo cambiare perché, ricordando che la costante di tempo $\tau = RC = C/G$, dove $C$ è la capacità e $G$ è la conduttanza, è utile impostare valori ragionevoli. Insieme a una capacità di default di $1 \mu F/cm^2$, la scelta di $G$ permette di ottenere costanti di tempo dell'ordine di 20-50 millisecondi, che sono tempi biologici ragionevoli per un neurone corticale umano o di ratto.

Un altro meccanismo facile da inserire è un generatore di corrente, o "pipetta". Anche qui, c'è un oggetto chiamato `n.IClamp`. Se fossi stato io, avrei scritto `soma.insert('IClamp')`. `IClamp` sta per *Current Clamp*, il che significa che si ha un elettrodo che controlla la corrente, non il potenziale. È un generatore ideale di corrente, dove il potenziale è libero di variare. Esiste anche l'opposto (Voltage Clamp), ma qui si usa solo lo stimolo di corrente, proprio come l'anno scorso si è provato a fare a mano, ottenendo la solita equazione differenziale del primo ordine. Qui, invece, è tutto in un colpo. `stim` è una variabile a cui viene assegnato l'oggetto creato dal comando `n.IClamp(Soma(0.5))`. L'elemento `0.5` si riferisce alla posizione lungo la sezione. Sebbene in un singolo compartimento la posizione non sia critica (0.1 o 0.5 sarebbe uguale), in un cavo con più sezioni (come un dendrite), si potrebbe posizionare l'elettrodo in punti diversi, specificando ad esempio il 25% o il 75% della lunghezza. L'elettrodo non è un meccanismo distribuito (che avrebbe dimensioni di microSiemens per centimetro quadrato), ma una corrente puntuale.

Questa parte può sembrare noiosa, ma con un esempio e l'uso di un *large language model* per suggerimenti, si dovrebbe essere in grado di replicare il processo per analogia. Non bisogna spaventarsi all'idea di dover studiare tutta la documentazione; in teoria sì, ma se si è pratici o ci si arrangia, a meno di cose particolarmente sofisticate, di solito gli ingegneri non leggono mai i manuali. La variabile `n` è semplicemente l'oggetto radice dell'intera libreria ed è il meccanismo con cui Python può dialogare con un altro programma. Importando NEURON come libreria, esso espone una serie di strutture, funzioni, ecc. Se si usasse, ad esempio, un comando `plot` senza il prefisso `n.`, Python assumerebbe che `plot` sia una funzione nello *scope* del programma che si sta scrivendo, non la troverebbe e genererebbe un errore. La riga `from neuron import h` (o `n` in questo caso) è fondamentale per questo.

L'elettrodo `IClamp` ha caratteristiche limitate: permette solo di iniettare una corrente costante. Non è possibile inserire una sinusoide o altre forme d'onda complesse con questo oggetto. È un generatore di corrente ideale DC con un valore costante. Le variabili `stim.amp`, `stim.dur` e `stim.delay` definiscono l'ampiezza, la durata e il ritardo della corrente. Ad esempio, `stim.amp = 0.01` (nA), `stim.delay = 20` (ms) e `stim.dur = 100` (ms) indicano che la corrente di 0.01 nA partirà con un ritardo di 20 millisecondi rispetto all'inizio della simulazione e durerà 100 millisecondi.

In questo modo, si sono costruite la geometria e la biofisica del modello (inserendo i meccanismi, qui solo quello passivo) e altri meccanismi non distribuiti, chiamati *point processes*. Mentre il meccanismo passivo, in un singolo compartimento, non fa distinzione, se si avesse un cavo, dire "inserisci pas" o "HH" lo inserirebbe in ogni sezione, trattando ogni sezione come se avesse un condensatore, una resistenza e una batteria. Quindi, quando si applicano proprietà passive a un cavo, si sta inserendo un meccanismo distribuito nello spazio. L'elettrodo, invece, è un *point process*.

### Registrazione dei Dati di Simulazione

Successivamente, c'è la parte, esteticamente orribile, in cui si deve riservare spazio in memoria per acquisire i dati con Python. Questi vettori non sono quelli di NumPy, che sono normalmente usati in Python per le librerie numeriche per le loro prestazioni. Qui, invece, si istruisce NEURON a riservare un tipo di vettore specifico all'interno della sua libreria. Questo vettore viene chiamato `SomaV` (o "Pippo", "Pippo2"), per indicare che conterrà i punti del potenziale di membrana nel soma durante la simulazione. Si definisce come un vettore e poi, con un altro metodo, lo si registra. Si registra una variabile interna, spesso con *underscore* nel nome, perché è "restia" a essere usata dall'esterno e nascosta. Se non si conosce in anticipo, si impiegherebbe mezz'ora a consultare la documentazione online, ma ci sono molti esempi che aiutano a capirlo. La variabile registrata è il potenziale nel compartimento al punto 0.5. Se si avesse un cavo, si potrebbero avere `dendrite1_V`, `dendrite2_V`, ecc., per registrare il potenziale di membrana in diversi punti dei dendriti.

Si fa la stessa cosa per la corrente di stimolo (`stim_vec`) perché si desidera plottare sia lo stimolo che la risposta. Anche se lo stimolo è noto (durata 100 ms, inizio a 20 ms, ampiezza 0.01 nA), è utile registrarlo. `stim` e `Soma` sono le variabili inizializzate con gli oggetti complessi creati in precedenza.

## Registrazione dei Dati di Simulazione

Si procede a registrare anche la variabile del tempo. L'obiettivo è avere un vettore del tempo per l'intera durata della simulazione, in modo da poterlo utilizzare facilmente per il plotting. Immaginando un sistema cartesiano con ascisse e ordinate (asse delle x e asse delle y), dove il tempo è la variabile indipendente e il potenziale di membrana (`soma_v`) la variabile dipendente, si vuole poter specificare `t, soma_v` per ottenere un asse delle x correttamente etichettato con i millisecondi (es. 20 ms, 22 ms, 25 ms) anziché con semplici indici di punti.

[Correzione] Si noti un errore nella trascrizione originale dove "H" è stato usato al posto di "T" per la variabile tempo. Questo è dovuto al fatto che fino all'anno scorso il nome della libreria era "H", ma ora è "N".

### Metodi Numerici in NEURON: Crank-Nicholson

Il `delta T` (passo di integrazione temporale) è un parametro che di solito si può omettere in NEURON. Il simulatore ha la capacità di determinare automaticamente il `delta T` ottimale in alcuni casi. Internamente, NEURON impiega un algoritmo numerico molto stabile, noto come il metodo di **Crank-Nicholson**.

È interessante notare che Crank e Nicholson non erano coinvolti nelle neuroscienze; erano matematici che studiavano le equazioni di diffusione. In matematica, è comune ricondurre problemi a casi già risolti se la forma dell'equazione è la stessa. Se la struttura matematica di un problema è identica a quella di un altro, le soluzioni e i metodi sviluppati per il primo possono essere importati e riutilizzati nel nuovo contesto.

Il comando per avviare la simulazione, che procede da un tempo $t=0$ (non inizializzato esplicitamente qui) fino a un `t_stop` di 220 ms, è `h.run()`. Quando la simulazione viene eseguita, non si vede alcuna output grafico immediato, ma le variabili registrate come `soma_v`, `stim_vec` (corrente di stimolo) e `t` vengono popolate con i dati.

#### Chiarimenti su `dt` e `dx`

Il `dt` (passo di integrazione numerica nel tempo) può essere modificato, ma il `dx` (passo di integrazione spaziale) è calcolato in base al `delta T` e non può essere cambiato direttamente. Nel caso di un modello a singolo compartimento, la situazione è più semplice perché si può conoscere la soluzione analitica e il sistema è relativamente "smooth", senza non linearità o potenziali d'azione rapidi. In questi scenari, si potrebbe "giocare" con il `delta T`.

Normalmente, tuttavia, non si specifica il `delta T` e NEURON lo sceglie automaticamente, specialmente per il `delta X`. Questo è particolarmente rilevante nello schema di risoluzione numerica di Crank-Nicholson.

Il metodo di Crank-Nicholson risolve equazioni differenziali alle derivate parziali che includono derivate rispetto al tempo e allo spazio. Queste equazioni continue possono essere rimpiazzate da equazioni discrete che incorporano un `delta t` e un `delta x`. Le variabili discrete $u_{i}^{j}$ avranno indici per il tempo (es. $i$) e per lo spazio (es. $j$). Questo approccio porta a un sistema di equazioni che può essere scritto in forma matriciale, e la scelta di `delta T` e `delta X` è intrinseca al metodo stesso.

#### Q&A: Vettori e Corrente di Stimolo

**Domanda:** Quando creiamo i vettori, gli elementi del vettore sono i singoli valori di potenziale?
**Risposta:** Sì, lo saranno.

**Domanda:** E in questo caso, qual è il senso di fare questa cosa se abbiamo già creato il gradino di corrente?
**Risposta:** Fondamentalmente, si definisce e inizializza una variabile come un vettore (es. `soma_v = h.Vector()`), ma non è che il vettore "sa" cosa deve contenere. È come definire una variabile in C come `int`, `float` o `double`; poi bisogna "linkarla" a qualcosa. Per la corrente, non si crea un "gradino" direttamente, ma si crea un meccanismo `iClamp`. Questo meccanismo viene poi "attaccato" al vettore di registrazione, che verrà popolato con i valori della corrente durante la simulazione.
Esiste un altro metodo più elaborato, che permette di generare una forma d'onda arbitraria (anche con numeri casuali) usando, ad esempio, NumPy, e poi "riprodurla" attaccandola all'ampiezza del `iClamp`. Tuttavia, questo non è banale e richiede una riga di codice specifica che non è immediatamente ricordabile.
L'approccio attuale consiste nel creare il meccanismo e poi "attaccargli" una variabile che si spera venga riempita durante la simulazione. Se non si registrasse la corrente in un vettore, non si potrebbe plottarla successivamente. Anche la variabile del tempo (`t`) potrebbe teoricamente non essere registrata, ma la sua registrazione facilita enormemente la visualizzazione dei risultati. L'aspetto cruciale è che NEURON risolve l'equazione differenziale per il potenziale di membrana ($V$).

### Visualizzazione dei Dati (Plotting)

Per la visualizzazione dei dati, si utilizza la libreria `matplotlib` di Python, in particolare il sottomodulo `pyplot`. Per brevità, `matplotlib.pyplot` viene importato come `plt`.

Un'operazione comune è la creazione di figure con più subplot. Per esempio, per avere due subplot disposti su due righe e una colonna, con dimensioni relative specifiche (es. uno più largo e uno più stretto), si usa un comando Python specifico. Questo comando restituisce degli "handle" (maniglie) che rappresentano gli assi o i subplot, permettendo di specificare su quale subplot disegnare ciascun grafico.

Ad esempio, nel primo subplot si può plottare il tempo (`T`) contro il potenziale di membrana (`soma_v`) con una linea nera, mentre nel secondo subplot si plotta il tempo contro la corrente di stimolo (`stim_vec`) con una linea grigia, aggiungendo una legenda che indica "corrente in nA".

La simulazione di un modello passivo produce una curva di carica del condensatore, un transitorio esponenziale.

## Implementazione di Modelli Neuronali

NEURON facilita l'implementazione di meccanismi attivi come il modello di Hodgkin-Huxley (HH) o altri modelli. Per un modello multicompartimentale, come un neurone "ball and stick" (soma e dendrite), non basta creare una sezione chiamata "soma". È necessario creare un'altra sezione, ad esempio "dendrite", e poi specificare come queste sezioni sono connesse, ad esempio: "attacca il punto di coordinata interna 1 del Soma al punto di coordinata interna 0 del dendrite". Questo permette di definire con precisione la topologia del cavo neuronale.

Considerando la complessità dell'equazione del cavo, implementare manualmente un modello multicompartimentale risolvendo le equazioni da zero sarebbe estremamente difficile e richiederebbe ore o giorni di lavoro. Con NEURON, operazioni simili si risolvono con poche righe di codice. Questo è il motivo per cui NEURON è ampiamente utilizzato per morfologie neuronali complesse e ramificate.

È possibile importare geometrie neuronali da siti come `neuromorpho.org`. Una volta importate, queste geometrie devono essere popolate con meccanismi biofisici, siano essi passivi o attivi (come Hodgkin-Huxley). Questo processo è relativamente semplice.

Nel corso degli anni, NEURON è notevolmente migliorato, diventando più potente e user-friendly. La precedente reticenza dovuta alla difficoltà di programmazione nel linguaggio HOC è stata superata.

### Meccanismi Ionici: Passivi e Attivi

Quando si passa da un modello passivo a uno attivo, ad esempio aggiungendo il meccanismo di Hodgkin-Huxley (`hh`) e rimuovendo il meccanismo passivo (`pas`), il comportamento del neurone cambia radicalmente. Senza il meccanismo di leak (`pas`), il modello HH con solo canali per il sodio e il potassio può mostrare attività spontanea, come la generazione di spike, specialmente se il sistema non è inizializzato a uno stato stazionario. Ad esempio, se il potenziale iniziale non è a -70 mV ma a -65 mV, il sistema potrebbe depolarizzarsi e generare uno spike a causa dello stato dei gating variables ($m, h, n$) che non sono "spenti" a quel valore.

Il modello di Hodgkin-Huxley, per sua natura, è un modello che genera potenziali d'azione. L'aggiunta di un meccanismo di leak (`pas`) aumenta la conduttanza totale della membrana, riducendo l'impedenza di ingresso e, di conseguenza, la deflessione del potenziale di membrana per una data corrente. Questo può portare a un minor numero di spike o a una soglia di eccitazione più alta.

NEURON utilizza internamente il metodo di Crank-Nicholson per risolvere le equazioni differenziali, anche per i meccanismi HH. Il modello HH è incluso di default in NEURON per la sua importanza e standardizzazione nella neurofisiologia, a differenza di altre correnti come la corrente $I_A$ che potrebbero dover essere scaricate da siti esterni.

### Implementazione di Meccanismi Ionici Custom (.mod files)

Per implementare meccanismi ionici non standard o personalizzati, si utilizzano file `.mod`. Questi file sono scritti in un linguaggio specifico (Model Description Language) e contengono commenti, equazioni e definizioni dei meccanismi.

Ad esempio, un file `.mod` per un potassio transitorio potrebbe definire due variabili di stato ($m$ e $h$), simili a quelle del sodio, ma con valori numerici e cinetiche diverse. La dipendenza della conduttanza dalle variabili di gating potrebbe essere, ad esempio, $m^4h$ anziché $m^3h$, per meglio adattarsi ai dati sperimentali. Il potenziale di inversione sarebbe quello del potassio.

Questi file `.mod` permettono di implementare equazioni dello stesso tipo di Hodgkin-Huxley, ma con parametri e cinetiche specifiche. Le equazioni algebriche e differenziali che descrivono le cinetiche dei canali, la dipendenza dalla temperatura (se inclusa) e la forma della conduttanza massima e la sua dipendenza dalle variabili di gating sono tutte definite in questi file. Questo approccio rende molto facile implementare nuovi meccanismi con poche righe di codice, seguendo lo stile dell'articolo originale di Hodgkin-Huxley.

## Ambiente di Sviluppo e Strumenti (Google Collab)

### Accesso ai Notebook e Installazione di NEURON

I notebook del corso sono disponibili su un repository GitHub (es. `mgiuliano.github.io/neuromodeling`). È possibile accedervi direttamente tramite Google Collab, che apre i notebook nel cloud. In alternativa, si possono scaricare i file `.ipynb` (IPython Notebook, il vecchio nome dei Jupyter Notebook) da GitHub cliccando sul pulsante "download raw file". Questi file possono essere eseguiti localmente con Jupyter se NEURON è installato sul proprio computer, oppure si può copiare e incollare solo il codice delle celle in un ambiente Python.

Quando si utilizza Google Collab, che è un computer remoto senza NEURON preinstallato, è necessario installare la libreria ogni volta. Questo viene fatto utilizzando un comando shell (`!pip install neuron`) all'interno di una cella del notebook. Il punto esclamativo (`!`) è un "escape command" che permette di eseguire comandi della shell (come `pip`) dall'interfaccia Python. `pip` è un gestore di pacchetti Python che scarica le librerie da siti come PyPI. L'installazione richiede qualche secondo e non sono necessari processi di compilazione aggiuntivi.

Le celle di codice in Google Collab possono essere automaticamente nascoste utilizzando la sintassi `#@title` o `#@param` per evitare di sovraccaricare l'utente con troppo codice all'inizio.

### Configurazione Iniziale e Importazioni

Dopo l'installazione, si importano le librerie necessarie: `import neuron as h` e `import matplotlib.pyplot as plt`. Viene anche caricato un file standard di NEURON, `stdrun.hoc`, che è un file scritto in HOC (il linguaggio nativo di NEURON). Questo file fornisce accesso a variabili di alto livello come `delta T`, `t_stop`, `t_start`, che altrimenti non sarebbero esposte.

Un comando importante per la visualizzazione è `plt.ion()`, che configura `matplotlib` per generare i plot "inline" (all'interno del Jupyter notebook) anziché in una finestra separata sul desktop. Questo è essenziale quando si lavora in un ambiente "headless" (senza monitor) come Google Collab, che esegue il codice su server remoti.

Successivamente, si ricrea il modello: si definisce il Soma, si aggiungono i meccanismi passivi, si crea l'elettrodo di current clamp con i valori specificati in precedenza, e si configurano i vettori per registrare il potenziale, la corrente di stimolo e il tempo.

### Controllo della Simulazione e Interattività

Per garantire che la simulazione venga eseguita correttamente ogni volta che una cella viene eseguita, è fondamentale includere il comando `h.finitialize(v_init)`. Questo comando "riavvolge" il tempo a zero e imposta le condizioni iniziali, assicurando che `h.run()` evolva il tempo da $t=0$ fino a `t_stop`. Senza `h.finitialize()`, se `h.t` è già uguale a `t_stop`, `h.run()` non avrebbe alcun effetto.

Google Collab offre funzionalità interattive come gli "slider" (cursori) che permettono di modificare i parametri della simulazione in tempo quasi reale. Questi slider vengono creati aggiungendo commenti speciali come `#@param` all'inizio di una cella, specificando il nome del parametro, il suo intervallo (minimo, massimo) e lo step. Un altro commento specifico di Collab, `run_auto`, posto all'inizio della cella tra parentesi graffe, fa sì che la cella venga rieseguita automaticamente ogni volta che un parametro dello slider viene modificato.

Questo permette di cambiare l'intensità della corrente di stimolo tramite uno slider e osservare immediatamente come cambia la curva del potenziale di membrana. Sebbene non sia strettamente in tempo reale, è sufficientemente rapido per sviluppare un'intuizione sul comportamento del modello. È utile fissare i limiti verticali degli assi (es. tra -80 mV e 0 mV) per evitare che `matplotlib` scali automaticamente il grafico, permettendo di visualizzare chiaramente le variazioni di ampiezza.

Questo approccio interattivo, pur mostrando transitori esponenziali in un modello passivo, diventa estremamente potente quando si introducono meccanismi attivi. Ad esempio, basta aggiungere `soma.insert('hh')` per trasformare il modello in un neurone eccitabile in meno di due secondi.

## Esempio di Ricerca: Oscillazioni Gamma Optogenetiche

### Esperimento di Giuliano e Pulizzi

In un articolo di Giuliano e Pulizzi, è stata studiata una rete di neuroni biologici ingegnerizzati optogeneticamente. Questi neuroni esprimevano opsine, canali ionici sensibili alla luce, introdotti tramite un vettore virale. Mantenendo la luce accesa per tutto il tempo, si è osservato che la rete iniziava a oscillare nella frequenza gamma (30-100 Hz), una banda di frequenza associata alla locomozione e alla cognizione.

Questo risultato è sorprendente perché l'esperimento è stato condotto su un piatto di micro-elettrodi, dove i neuroni sono coltivati in vitro e non presentano la complessa struttura e connettività tipica del cervello in vivo. Nonostante l'assenza di una struttura organizzata, il sistema ha mostrato oscillazioni ritmiche in risposta a uno stimolo costante.

L'analisi spettrale dei dati ha rivelato delle "strisce" nello spettrogramma, indicando una struttura temporale nelle oscillazioni. La media temporale della risposta mostrava chiaramente delle oscillazioni.

### Modellazione delle Oscillazioni Gamma e Prospettive Future

Per comprendere queste oscillazioni, è stato utilizzato un modello matematico semplificato, chiamato "fire and break" (probabilmente una variante di "integrate and fire"). Questo modello, pur non riproducendo gli spike individuali, è riuscito a generare oscillazioni di popolazione, creando un effetto di "ping-pong" tra i neuroni.

L'ambizione futura è quella di sviluppare un modello più fisicamente realistico che possa riprodurre l'esperimento nelle stesse condizioni. È stato scoperto che la luce, oltre ad attivare direttamente le opsine, può anche modificare il funzionamento delle sinapsi. Contrariamente a quanto si pensava, le opsine non sono espresse solo nel soma, ma anche nei bottoni sinaptici. Rocco Pulizzi ha osservato che, in un contesto di stimolazione luminosa prolungata (frazioni di secondo), l'esposizione alla luce aumenta l'ingresso di calcio nelle sinapsi.

L'obiettivo è quindi combinare una rete di modelli neuronali con sinapsi che incorporano una "barriera" al calcio, la cui dinamica è dipendente dalla luce. Questo permetterebbe di simulare come uno stimolo luminoso (non modellato direttamente, ma che influenza il calcio sinaptico) possa alterare le oscillazioni a diverse frequenze. Questo rappresenta un tipo di tesi di dottorato che mira a collegare i meccanismi biofisici a livello sinaptico con il comportamento emergente di una rete neuronale.

## Caratterizzazione di Modelli Neuronali con NEURON

### Curva Corrente-Potenziale (I-V) per Modelli Passivi

Una prima attività che si può svolgere, o provare a svolgere, è la caratterizzazione della cosiddetta curva corrente-potenziale (I-V). Questo implica variare l'ampiezza di un impulso di corrente iniettato e osservare il valore di *steady state* del potenziale di membrana. Questo valore, in un modello passivo, è stato persino derivato analiticamente, "carta e penna", ed è una somma di due termini: uno costante che dipende dalla corrente iniettata e un esponenziale che, a un certo punto, svanisce. Si osserva che il valore di *steady state* si abbassa all'abbassarsi della corrente, fino a diventare zero e poi negativo.

Sperimentalmente, si utilizza esattamente questo protocollo. Se la cellula è eccitabile, a un certo punto la curva non seguirà più un andamento lineare, ma inizieranno a comparire degli *spike*. È utile, e spesso si fa, rappresentare su un piano cartesiano una serie di punti in cui la coordinata X è il valore dell'impulso di corrente (ad esempio, 0.005 nA, ovvero 5 pA) e la coordinata Y è il valore del potenziale. Quando si parla di "valore del potenziale", si intende il potenziale di *steady state*. Per assicurarsi di raggiungere lo *steady state*, la durata della stimolazione elettrica viene scelta in modo tale da permettere alla traccia di raggiungere e permanere per un certo periodo in tale condizione. La soluzione teorica prevede una somma tra una costante e un esponenziale decrescente, dove l'esponenziale svanisce asintoticamente a tempo infinito; in pratica, è sufficiente attendere 2, 3 o 4 costanti di tempo.

Per costruire questo grafico, si può convenzionalmente prendere l'ultimo punto della simulazione prima di spegnere la corrente come migliore approssimazione dello *steady state*, poiché più si attende, più il sistema si avvicina a tale condizione. In un esperimento reale, dove la traccia può essere leggermente rumorosa, anziché prendere l'ultimo punto, si possono considerare gli ultimi 100 millisecondi della risposta e calcolare la media aritmetica di tutti i punti in questo intervallo. Questo fornisce un singolo numero da plottare in funzione della corrente.

Il risultato atteso per un modello puramente passivo è una relazione lineare, che segue la legge di Ohm, $V = R \cdot I$, dove $V$ è il potenziale, $I$ è la corrente e $R$ è la resistenza. La retta tracciata, che rappresenta la legge di Ohm, passa perfettamente attraverso i punti simulati, confermando il comportamento resistivo del modello. Sperimentalmente, una resistenza più grande è correlata a una cellula più piccola. Questo protocollo sperimentale è quindi molto utile per ottenere informazioni sulla dimensione della cellula, poiché la conduttanza di *leak* passiva è nota e il modello cilindrico in simulazione scala l'area del soma per questa grandezza specifica.

### Simulazione di Modelli Attivi a Compartimento Singolo

Per esplorare modelli più complessi, si passa a un secondo *notebook* che implementa un modello a compartimento singolo con meccanismi attivi. Prima di procedere, è necessario disconnettere ed eliminare il *runtime* precedente per evitare conflitti.

#### Configurazione dell'Ambiente Google Colab per NEURON

L'utilizzo di Google Colab per simulazioni NEURON presenta alcune specificità. Colab fornisce un computer remoto con uno spazio disco temporaneo, che viene cancellato ogni volta che si disconnette ed elimina il *runtime*. I programmi in esecuzione, come NEURON, cercano i file `.mod` (che definiscono i meccanismi ionici) nella directory corrente.

Un modo per gestire questi file è trascinarli dal proprio *hard disk* locale all'ambiente Colab. Un metodo alternativo, scoperto di recente, è utilizzare una "meta-cella" in Google Colab con il comando `%%writefile`. Questo comando permette di scrivere direttamente un file di testo nella directory corrente. Ad esempio, si può creare un file chiamato `nav.mod` copiando e incollando il codice di un modello da un *link* esterno.

#### Implementazione di Meccanismi Attivi (.mod files)

Il file `.mod` definisce i meccanismi ionici, come le conduttanze voltaggio-dipendenti. Ad esempio, si può definire un canale sodio voltaggio-dipendente e un canale potassio presi da diversi articoli scientifici. All'interno del file `.mod`, si trova l'equazione familiare per la corrente sodio: $I_{Na} = \bar{g}_{Na} m^3 h (V - E_{Na})$, dove $\bar{g}_{Na}$ è la conduttanza massima, $m$ e $h$ sono le variabili di *gating* (di stato), e $E_{Na}$ è il potenziale di inversione del sodio. La sintassi specifica le equazioni differenziali per le variabili di *gating*. Eseguendo la cella `%%writefile`, il file `.mod` viene creato nella directory locale.

Dopo aver creato i file `.mod` per i meccanismi desiderati (ad esempio, `nav.mod` per il sodio, `k_delay_rectifier.mod` per il potassio, e `ih.mod` per le correnti *funny*), è necessario che NEURON li incorpori. Questo si fa con il comando `!nrnivmodl`, che è un comando di *shell* (non Python) che compila i file `.mod` scritti nel linguaggio MODL. Questo processo utilizza un compilatore C per convertire i meccanismi in file `.cpp` e crea una directory `x86_64` contenente un eseguibile chiamato `special`.

Un passaggio cruciale in Google Colab è che, dopo la compilazione, NEURON non utilizzerà automaticamente il nuovo eseguibile `special`. È necessario andare su `Runtime` e selezionare `Restart session`. Questo permette a NEURON di caricare i meccanismi appena compilati.

Una volta riavviata la sessione, si può creare il modello. In questo caso, oltre al meccanismo passivo, si inseriscono i meccanismi del potassio a rettificazione ritardata (KDR) e del sodio a rettificazione ritardata (NaDR) con specifici valori di conduttanze massime. È possibile anche specificare la temperatura. Il resto del *setup*, inclusa la stimolazione con un elettrodo, è identico al caso passivo.

#### Analisi della Curva Corrente-Potenziale (I-V) in Modelli Attivi

La simulazione di un modello con conduttanze attive mostra una risposta che include il comportamento passivo e poi l'attivazione dei canali sodio e potassio voltaggio-dipendenti. Aumentando gradualmente la corrente, si osserva che il neurone inizia a generare *spike*. A differenza del modello di Hodgkin-Huxley, con queste conduttanze specifiche, potrebbe essere possibile ottenere frequenze di *firing* arbitrariamente basse, anche se questo modello potrebbe iniziare a sparare direttamente a 3 *spike* ogni 220 ms.

Lo studio delle proprietà attive permette di bombardare il neurone con corrente e osservare l'aumento del numero di *spike*. La curva I-V per un modello attivo non segue una retta e la legge di Ohm non è più valida. Il motivo è che le conduttanze attive non sono completamente spente a potenziali vicini al riposo (ad esempio, -80 mV, -78 mV, -76 mV). Le cinetiche delle variabili di *gating* ($m_\infty$, $n_\infty$, $h_\infty$) sono sigmoidi e iniziano a piegare anche a potenziali come -70 mV, attivando le conduttanze voltaggio-dipendenti e creando deviazioni rispetto al caso puramente passivo.

Sperimentalmente, in laboratorio, si cerca comunque di *fittare* una retta almeno nella parte iniziale della curva, dove la relazione è quasi lineare. Se la depolarizzazione è eccessiva (ad esempio, a -72 mV), la linearità si perde. Per ottenere una relazione lineare o quasi lineare, è necessario applicare correnti più piccole, eventualmente anche negative, per non disturbare eccessivamente le conduttanze attive. La pendenza di questa retta viene tipicamente chiamata impedenza d'ingresso, e si assume che sia l'inverso della conduttanza di *leak*.

La procedura di calcolare la media dei punti alla fine della risposta al gradino (negli ultimi 10-20 ms) diventa "scombinata" quando il neurone inizia a sparare. Se, ad esempio, a 0.04 nA il neurone genera *spike*, la media calcolata includerà questi *spike*, non fornendo più una stima accurata del potenziale di *steady state* passivo. Questo spiega perché i punti in questa regione sono disordinati e non riflettono il comportamento passivo atteso.

#### Caratterizzazione della Curva Frequenza-Corrente (F-I)

Un altro esercizio utile è la determinazione della curva frequenza-corrente (F-I), che indica quanti *spike* vengono generati per un dato valore di corrente. Questo richiede un grafico in cui la corrente è sull'asse X e la frequenza di *firing* sull'asse Y. Internamente, il processo implica l'esecuzione di numerose simulazioni, una per ogni punto desiderato, utilizzando un ciclo `FOR` in Python che varia la corrente iniettata a ogni iterazione.

L'elemento aggiuntivo rispetto alla curva I-V è la necessità di contare gli *spike*. NEURON offre un meccanismo specifico per questo, un *point process* chiamato `ActionPotentialCount` (APC). Cercando `n.apcount, action potential count neuron point process`, si può trovare come utilizzarlo per contare i potenziali d'azione fornendo una soglia di *detection*.

In assenza di questo meccanismo, si può implementare un algoritmo di *peak detection*. Questo implica analizzare il vettore dei punti della traccia del potenziale di membrana per trovare gli attraversamenti di una soglia predefinita (ad esempio, 0 mV). È fondamentale rilevare l'attraversamento solo con derivata positiva, cioè dal basso verso l'alto. Per fare ciò, si può iterare attraverso il vettore, controllando se il potenziale è sopra o sotto la soglia. Se il potenziale è sopra la soglia e nell'istante precedente era sotto, si incrementa un contatore. È importante usare una variabile booleana temporanea (o 0/1) per assicurarsi di contare ogni *spike* una sola volta, evitando di contare tutti i punti che rimangono sopra la soglia. Questo esercizio è utile per praticare l'elaborazione elementare del segnale, anche se nelle simulazioni il segnale è pulito e non richiede filtraggio.

### Introduzione ai Modelli Multicompartimentali: "Ball and Stick"

Il terzo *notebook* introduce un modello multicompartimentale di tipo "ball and stick", che rappresenta un neurone con un soma e un dendrite.

#### Definizione della Geometria e dei Meccanismi

Il codice per questo modello inizia con l'importazione delle librerie standard. Per la prima volta, non si definisce solo una sezione chiamata `soma`, ma anche un'altra chiamata `dendrite`. Sebbene le etichette possano essere arbitrarie, il `soma` viene tipicamente definito con lunghezza e diametro uguali, mentre il `dendrite` può avere dimensioni diverse. La lunghezza del dendrite può essere resa variabile tramite uno *slider*.

A differenza dei modelli a compartimento singolo, per il dendrite si specifica esplicitamente che deve essere descritto da 51 singoli compartimenti (`nseg = 51`). Questo è cruciale perché si desidera che il dendrite abbia un'estensione spaziale, non essendo un singolo punto. Il numero di segmenti (`nseg`) determina la finezza della descrizione spaziale; una descrizione più fine permette di posizionare elettrodi, generatori o sinapsi in punti specifici lungo la struttura.

Nel `soma` si inserisce un modello di eccitabilità, come quello di Wang-Buzsáki (simile a Hodgkin-Huxley), che include conduttanze sodio e potassio voltaggio-dipendenti, oltre alla conduttanza di *leak*. Nei dendriti, invece, si inserisce solo il modello passivo (conduttanza di *leak*). Questo configura un modello in cui l'eccitabilità risiede principalmente nel soma, mentre i dendriti sono passivi. Se si fossero inseriti meccanismi eccitabili anche nei dendriti, la struttura si sarebbe comportata più come un assone.

I dendriti della maggior parte delle cellule neuronali sono passivi, sebbene esistano eccezioni notevoli, specialmente nella corteccia e nell'ippocampo, dove sono presenti conduttanze voltaggio-dipendenti anche nei dendriti. Non è detto che queste conduttanze siano espresse uniformemente in tutti i punti. Come menzionato nel concetto di "dendritic democracy", le proprietà del *campus* cellulare dipendono dall'espressione dei canali nell'albero dendritico, e la densità di distribuzione di una particolare conduttanza potrebbe non essere uniforme nello spazio, ad esempio, più elevata al soma e decrescente allontanandosi. NEURON semplifica la gestione di queste complessità, nascondendo i dettagli sottostanti e permettendo di specificare facilmente gradienti di canali.

#### Impatto della Costante di Spazio e dell'Estensione Spaziale

Il parametro `nseg` permette di specificare che una struttura ha un'estensione spaziale e deve essere descritta da più variabili di stato, non da un unico potenziale di membrana, ma da 51 potenziali di membrana in questo esempio. Questo ha un effetto significativo quando la lunghezza del dendrite è elevata rispetto alla "costante di spazio" ($\lambda$). La costante di spazio è legata a parametri biofisici di *default* come la resistenza assiale e il diametro (la capacità di membrana non è direttamente coinvolta nella $\lambda$ ma nella costante di tempo $\tau_m$), e determina quanto rapidamente il potenziale si attenua lungo la struttura. Se la lunghezza è maggiore della costante di spazio, le grandezze non sono istantaneamente allo stesso potenziale, cioè non sono isopotenziali. Anche il soma dovrebbe essere considerato, ma essendo piccolo, l'effetto sarebbe minimo. Se il dendrite fosse molto corto, i potenziali misurati in tre punti diversi tenderebbero a coincidere, dimostrando l'isopotenzialità.

In Python, un ciclo `for` che itera su un oggetto `h.dend` (dove `dend` è una collezione di sotto-oggetti che rappresentano i segmenti del dendrite) permette di navigare lungo tutti i 51 segmenti. Ogni `seg` diventa l'elemento generico su cui si possono impostare valori. Questo approccio conciso permette di impostare lo stesso valore per tutti i segmenti o, in modo altrettanto semplice, di descrivere una densità di distribuzione di canali che cambia nello spazio, come un gradiente.

## Impatto della Costante di Spazio e dell'Estensione Spaziale

### Impostazione di Gradienti Spaziali

È possibile implementare un gradiente per il potenziale di inversione, ad esempio, moltiplicandolo per la coordinata spaziale del segmento. L'obiettivo sarebbe ottenere un valore che vari linearmente, per esempio, da $-65 \text{ mV}$ a $-70 \text{ mV}$. Tuttavia, l'implementazione di questo gradiente all'interno di un ciclo `for` che itera sui segmenti (`h.dend`) richiede di comprendere come accedere alla coordinata spaziale (`seg.x` o una variabile lineare tra 0 e 1) di ciascun segmento. [Inference] L'oratore esprime una difficoltà nell'ottenere direttamente questa coordinata tramite l'oggetto `seg` nel contesto attuale del ciclo `for`, suggerendo che un tentativo diretto potrebbe generare un errore.

### Configurazione delle Registrazioni e Simulazioni Passive

Per le simulazioni, sono stati impostati quattro vettori per la registrazione dei potenziali: `v-soma` (potenziale del soma), `v-dend starting point` (potenziale del dendrite all'inizio), `v-dend midpoint` (potenziale del dendrite a metà) e `v-dend end point` (potenziale del dendrite alla fine). Specificamente, il potenziale del soma viene registrato al 50% della sua lunghezza, mentre per il dendrite i punti di registrazione sono stati fissati a 0.1, 0.5 e 1.0 della sua lunghezza totale. Questi punti possono essere modificati, ad esempio, a 0.25 e altre posizioni. Eseguendo il codice con questa configurazione, è possibile visualizzare e analizzare le quattro grandezze registrate.

### Esplorazione dell'Equazione del Cavo: Dendriti Passivi

L'interfaccia grafica permette di esplorare in modo relativamente semplice il comportamento descritto dall'equazione del cavo. Si è notato un problema con uno slider che non era limitato a un intervallo definito, rendendo difficile la regolazione precisa.

Considerando un dendrite molto corto, ad esempio di $60 \text{ µm}$ (mentre il soma è di circa $20 \text{ µm}$), il potenziale di membrana registrato nei dendriti, generato da uno *step* di corrente iniettato nel soma, mostra un fenomeno noto come potenziale d'azione a retropropagazione (*back-propagating action potential* - BPAP). Questo fenomeno, scoperto nei neuroni corticali più di vent'anni fa, ha rivelato che i potenziali d'azione non si propagano solo verso l'assone, come si pensava in precedenza.

In queste condizioni di dendrite corto, le tracce del potenziale (nera per il soma, rossa per il dendrite al 25%, blu per il 50% e verde per il 100% della lunghezza) sono quasi perfettamente sovrapposte. Questo indica che il sistema è effettivamente isopotenziale, nonostante sia discretizzato in 51 sezioni.

Aumentando gradualmente la lunghezza del dendrite, ad esempio a $200 \text{ µm}$, si osserva non solo un'attenuazione del segnale, ma anche un suo allargamento (*spanciamento*). È importante considerare che in cellule corticali, come quelle di ratto, un dendrite apicale di una cellula piramidale può raggiungere lunghezze significative, anche fino a $600-800 \text{ µm}$ o persino $1 \text{ mm}$. In questi casi, il potenziale d'azione che al soma è di circa $20 \text{ mV}$ risulta molto attenuato e appiattito nel dendrite.

Un aspetto interessante è la chiara evidenza di ritardi nella propagazione passiva del segnale: il potenziale parte prima nel soma (traccia nera), poi nel dendrite al 25% (rossa), poi a metà (blu) e infine all'estremità (verde).

### Introduzione di Meccanismi Attivi e Propagazione degli Spike

Per studiare la propagazione attiva, è possibile aggiungere ai dendriti meccanismi attivi, come il modello di Wang-Guzaki, che per semplicità può essere considerato un insieme di canali al sodio e al potassio voltaggio-dipendenti. Se si abilita questo meccanismo, il dendrite inizia a comportarsi come un assone non mielinato, dove tutti i 51 segmenti sono individualmente capaci di generare un potenziale d'azione, in prima approssimazione.

Anche se lo stimolo di corrente viene iniettato solo nel soma per generare lo *spike*, si osserva che il potenziale d'azione si propaga lungo il dendrite, mantenendo la sequenza temporale (nero, rosso, blu, verde) che indica una propagazione spaziale.

Quando la lunghezza dell'assone aumenta, ad esempio a $500 \text{ µm}$, si notano ritardi di propagazione di qualche millisecondo. È utile ricordare che gli assoni possono essere estremamente lunghi: i neuroni talamo-corticali possono avere assoni di diversi millimetri, mentre nell'uomo, i neuroni del tratto piramidale che proiettano al midollo spinale possono raggiungere lunghezze di mezzo metro o anche un metro. Sebbene queste fibre siano mielinate e la propagazione sia saltatoria (e quindi diversa), già a qualche centinaio di micrometri si possono apprezzare ritardi significativi.

### Analisi della Velocità di Propagazione

Un possibile esercizio consiste nel ripetere la simulazione per diversi valori della lunghezza dell'assone o delle posizioni degli elettrodi di registrazione nei dendriti, al fine di calcolare la velocità di propagazione. Poiché la distanza tra il soma e il dendrite è definita, si può immaginare di utilizzare un dendrite molto lungo per ottenere valori di ritardo spazialmente ben separati.

Ad esempio, con una lunghezza del dendrite di quasi $2 \text{ mm}$ (2000 µm), si può osservare un ritardo di circa $5 \text{ ms}$. La velocità di propagazione può essere calcolata come:
$$v = \frac{\Delta L}{\Delta t}$$
Per i valori citati:
$$v = \frac{2000 \text{ µm}}{5 \text{ ms}} = 400 \text{ µm/ms}$$
Convertendo in metri al secondo:
$$v = 400 \text{ µm/ms} = 0.4 \text{ mm/ms} = 0.4 \text{ m/s}$$
Questa velocità è considerata lenta, soprattutto se paragonata alle velocità di propagazione dei segnali nei dispositivi elettronici artificiali. [Inference] L'oratore ricorda una misurazione simile della velocità di propagazione dei potenziali d'azione nella rana, forse da parte di un ricercatore di nome Elmonds, che aveva riscontrato velocità analoghe e lente anche nell'uomo.

Per una misurazione più precisa della velocità, si suggerisce di effettuare una *peak detection* per identificare i picchi dei potenziali d'azione e le loro coordinate temporali. Plottando la distanza in funzione del tempo di picco, i punti dovrebbero allinearsi su una retta, la cui pendenza rappresenterebbe la velocità di propagazione. Se la velocità è uniforme, il rapporto $\Delta L / \Delta t$ dovrebbe essere costante, risultando in un valore di circa $0.4 \text{ m/s}$. Sebbene la libreria di *plotting* utilizzata non permetta di visualizzare direttamente questi punti, un'alternativa come Plotly potrebbe essere impiegata per questo esercizio.

### Impatto delle Sinapsi Distali e Fenomeni di Saturazione

Un'altra dimostrazione riguarda l'effetto di una sinapsi posizionata a una certa distanza, tipicamente all'estremità distale di un dendrite passivo. Quanto più il dendrite è lungo, tanto più lontana è la sinapsi e tanto più evidente sarà l'attenuazione elettrotonica del segnale sinaptico. Gli *slider* nell'interfaccia permettono di modificare l'ampiezza della sinapsi e il numero di impulsi attivati. I potenziali post-sinaptici eccitatori (EPSP) vengono registrati nel punto distale di emissione, mentre il potenziale del soma è visualizzato come traccia nera.

Si osserva che un EPSP forte, generato in un dendrite distale a $650 \text{ µm}$, si propaga fino al soma, depolarizzandolo di circa $5-10 \text{ mV}$. Con più impulsi, si verifica una sommazione temporale, ma il segnale al soma tende a saturare, impedendo l'emissione di uno *spike*. Pertanto, una sinapsi a $600 \text{ µm}$ potrebbe non essere in grado di innescare un potenziale d'azione nel neurone, anche con una frequenza di attivazione massima di $200 \text{ Hz}$ (che è già molto elevata e potenzialmente irrealistica per una singola sinapsi), o aumentando l'ampiezza in modo sproporzionato.

Inizialmente, si potrebbe pensare che la saturazione sia dovuta all'occupazione completa dei recettori post-sinaptici. Tuttavia, la vera ragione risiede nella natura delle sinapsi basate sulle conduttanze. La corrente sinaptica ($I_{syn}$) è data dall'equazione:
$$I_{syn} = g_{syn}(V_m - E_{rev})$$
dove $g_{syn}$ è la conduttanza sinaptica e $E_{rev}$ è il potenziale di inversione della corrente sinaptica (per una sinapsi AMPA, $E_{rev} \approx 0 \text{ mV}$).

Quando la conduttanza sinaptica ($g_{syn}$) è molto elevata, il potenziale di membrana ($V_m$) nel dendrite distale si avvicina rapidamente al potenziale di inversione ($E_{rev}$). Man mano che $V_m$ si avvicina a $E_{rev}$, il termine $(V_m - E_{rev})$, che rappresenta la forza motrice, diventa sempre più piccolo. Di conseguenza, la corrente sinaptica si riduce, e la depolarizzazione satura, non potendo superare $E_{rev}$. Questo fenomeno è analogo a quello che impedisce ai potenziali d'azione di superare il potenziale di inversione del sodio, tipicamente attestandosi intorno a $+20 \text{ mV}$ e non oltre i $+50 \text{ mV}$ o il valore specifico di $E_{Na}$.

Ricostruire questo fenomeno in un proprio ambiente di simulazione potrebbe essere un esercizio interessante per comprendere l'attenuazione elettrotonica e la saturazione sinaptica.

### Esercizi Avanzati: Assoni Eterogenei e Conduzione Saltatoria

Il meccanismo di Wang-Guzaki, che include canali al sodio ($G_{Na}$) e al potassio ($G_K$), è applicato a tutti i 51 segmenti del modello. È possibile modificare i valori di queste conduttanze per ciascun segmento utilizzando un ciclo `for`. Impostando a zero i valori di $G_{Na}$ e $G_K$ per specifici segmenti, si disattiva di fatto il meccanismo attivo in quelle sezioni, poiché NEURON, pur risolvendo le equazioni, genererà una corrente nulla. Questo permette di creare un assone eterogeneo.

Un esercizio avanzato proposto è la simulazione della conduzione saltatoria, tipica degli assoni mielinati. Questo può essere realizzato creando una distribuzione a "macchie" delle conduttanze: segmenti attivi (con $G_{Na}$ e $G_K$ a valori significativi) alternati a segmenti passivi (con $G_{Na}$ e $G_K$ a zero). La sfida principale consiste nell'ottenere la coordinata spaziale (`seg.x`) di ciascun segmento all'interno del ciclo `for`. Una volta risolto questo problema (ad esempio, tramite ricerca su Google), si potrebbe utilizzare una funzione matematica, come il resto della divisione intera (`numpy.mod`), per definire intervalli spaziali in cui le conduttanze sono attive o nulle.

Per una simulazione più accurata della mielinizzazione, sarebbe necessario anche ridurre o eliminare la conduttanza di *leak* nei segmenti passivi (mielinizzati), poiché la mielina "tappa" la membrana, riducendo la dispersione di corrente. Questo approccio offre un metodo relativamente semplice per modellare la conduzione saltatoria senza ricorrere a implementazioni complesse.

### Lo Zoo dei Canali Ionici e Prospettive Future

Un altro *notebook* di esempio, denominato "The Zoo of Ion Channels", esplora l'impatto di diverse combinazioni di conduttanze ioniche (ad esempio, corrente $I_A$, sodio persistente, corrente $I_H$) sull'attività di *spiking* del neurone. Questo strumento è disponibile per l'esplorazione e la sperimentazione.

Per il futuro, si passerà a discutere modelli neuronali più ridotti. Questi modelli, pur essendo meno dettagliati dei modelli a compartimenti multipli, consentono di derivare considerazioni molto interessanti a livello di rete. Sebbene sia possibile costruire reti di centinaia di migliaia di neuroni (come fatto da Markram e altri), tali sistemi sono enormemente complessi, con un vasto numero di parametri liberi e variabili, e richiedono tempi di simulazione proibitivi per piattaforme come Google Colab.

