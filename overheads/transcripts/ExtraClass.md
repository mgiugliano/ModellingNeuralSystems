## Introduzione all'Approssimazione di Diffusione e Richiami di Teoria delle Probabilità

In questa lezione supplementare, vorrei illustrarvi e fornirvi qualche elemento in più riguardo i processi stocastici e, in particolare, la parte identificata come **approssimazione di diffusione**. Sarà più chiaro nel prosieguo di cosa si tratta. Inizierò semplicemente rinfrescandovi le idee sulla teoria delle probabilità.

### Richiami Fondamentali sulla Teoria delle Probabilità

Ricordo in particolare il contributo essenziale di **Andrei Kolmogorov** all'inizio del secolo scorso, il quale ha sistematizzato la teoria ed è stato un campione da tanti punti di vista, anche nella definizione dei processi stocastici. In generale, ricorderete che la teoria delle probabilità, come già discusso in classe, ha a che fare con la **deduzione**. Si tratta quindi di una teoria, un principio, da cui si deduce qualcosa legato all'esperienza, e non il viceversa. Non si procede dall'esperienza alla teoria, dal particolare al generale; quell'approccio è l'ambito della statistica, o inferenza.

Nel contesto della teoria delle probabilità, fondamentalmente, si parte dall'esistenza di un esperimento. Questo esperimento produce delle uscite, e tali uscite sono rappresentate utilizzando il linguaggio della teoria degli insiemi. L'insieme di tutte le possibili uscite di un esperimento viene chiamato **$\Omega$** (Omega); qui è semplicemente indicato con dei quadratini, ma la notazione non è fondamentale. Ciascun esperimento può avere un'uscita all'interno di questo insieme.

Per esempio, quando si lancia una moneta, che sia una moneta "buona" (con probabilità 50-50 per testa o croce) o una moneta non equa, si avranno comunque delle uscite, indipendentemente dal valore di probabilità. Queste uscite sono testa ($H$, Head) o croce ($T$, Tail). Se l'esperimento consiste nel lanciare due monete (allo stesso momento o in momenti diversi, non importa, purché sia la definizione dell'esperimento), l'insieme di tutte le possibili uscite (lo spazio campionario, $\Omega$) avrà a che fare non con due, ma con **quattro** eventi: entrambe siano testa ($HH$), una testa e l'altra croce ($HT$), l'esatto contrario ($TH$), oppure che tutte e due siano croce ($TT$).

Mi è scappato l'uso comune del termine *evento* e *uscita* dell'esperimento, ma l'evento è anche una cosa più generale, ovvero può essere una qualunque collezione, un qualunque **sottoinsieme** dell'insieme $\Omega$. Io potrei definire l'evento che voglio studiare, ad esempio, come quello in cui la prima delle due monete esce testa. In questo caso, sto considerando un sottoinsieme composto dalle uscite $HH$ e $HT$. Questo sottoinsieme lo chiamo pure evento. L'evento può essere l'insieme $\Omega$ stesso oppure può essere l'insieme nullo. Vedremo poi perché questo ha una sua rilevanza.

### Esempio Neurofisiologico: Risposta Neuronale Stocastica

Cerchiamo di fissare le idee con un altro esempio. Supponete che io abbia una scimmia e le faccia vedere la foto di un cane, presentandola per un periodo molto, molto breve. Durante la presentazione e immediatamente dopo (qualche millisecondo dopo), misuro l'attività elettrica nella corteccia inferotemporale di questa scimmia. In particolare, cerco di caratterizzare l'attività elettrica di un neurone specifico, vicino all'elettrodo. Questo neurone può rispondere emettendo un potenziale d'azione, oppure può rimanere silenzioso. In questo contesto, l'insieme delle possibili uscite di questo esperimento (l'insieme $\Omega$) è: **il neurone spara** oppure **il neurone non spara**.

Qui vedete una simulazione che ho incluso, basata su un modello con interpretazione stocastica dell'eccitabilità ionica (eccitabilità neuronale), che mi permetteva di generare risposte non deterministiche. Notate cinque realizzazioni, o **prove**, ciascuna delle quali ha visto l'applicazione dello stesso stimolo (in questo caso, uno stimolo di corrente). Vedete che tre volte su cinque il neurone non ha sparato, nonostante sia stato depolarizzato, mentre in due occasioni il neurone ha sparato. Questo esempio, al di là del fatto che la simulazione abbia una componente non deterministica, aleatoria o casuale, è estremamente rappresentativo di una situazione che si verifica in qualunque sistema complesso descritto con una formulazione stocastica.

### Assiomi di Probabilità e Frequenza Relativa

Di fronte a un risultato come quello appena descritto (due risposte su cinque prove), sareste tentati di dire che questo rapporto, $2/5$, abbia qualcosa a che fare con una probabilità o con una **frequenza relativa**. Effettivamente, l'idea è, da un punto di vista assiomatico per Kolmogorov e più empirico per Bernoulli, che la probabilità sia un qualche numero che noi associamo a un evento.

Quello a cui siete abituati nell'esperienza comune è rappresentare la probabilità con il concetto di frequenza relativa. Questa è, in effetti, il limite su un numero enorme di tentativi $N$, in cui un certo evento $A$ si verifica ($n_A$), rispetto al numero totale di prove ripetute:
$$P(A) = \lim_{N \to \infty} \frac{n_A}{N}$$
Questa definizione è stata utilizzata per molto tempo, forse per un centinaio d'anni, e richiede un numero elevato di osservazioni. Il calcolo è semplice, è una frequenza relativa, ma ha una natura **a posteriori**, derivata dall'esperienza.

Potrei, invece, desiderare di calcolare la probabilità *a priori*, per esempio nel caso di una moneta completamente simmetrica dal punto di vista geometrico, che non presenta alcuna caratteristica fisica che privilegi un lato rispetto all'altro. In tal caso, potrei dire che non c'è ragione per privilegiare una parte o l'altra. Se, al contrario, una moneta avesse più massa su un lato (ad esempio, a causa della raffigurazione di un re o una regina), potrebbe essere ragionevole assumere *a priori* un valore diverso per la probabilità. Sebbene il calcolo o l'utilizzo della frequenza relativa sia utile nel caso di problemi reali (come per le compagnie di assicurazione che devono stimare le probabilità di incidenti dai dati), questa definizione presenta dei problemi.

Se la frequenza relativa è $0$, l'evento è impossibile o quasi impossibile, perché si tratta di un processo al limite. Allo stesso tempo, un valore di $1$ significa che l'evento è quasi certo. [Inference] La definizione basata sul limite ha problemi più profondi; per esempio, il limite potrebbe matematicamente non esistere, e dipende da avere una quantità di dati notevole. Se non si dispone di dati, o se si vuole **dedurre** anziché **indurre**, questa definizione diventa problematica.

#### Gli Assiomi di Kolmogorov
Kolmogorov ci propone una possibilità diversa: la probabilità è una funzione che assegna un numero a ogni evento. Dato un esperimento, si definiscono gli eventi (che possono essere sottoinsiemi delle uscite, non necessariamente la singola uscita). Nel nostro caso, gli eventi sono *il neurone spara* e *il neurone non spara*, mutuamente esclusivi. A questi eventi, io assegno *a priori* un certo numero.

Questa assegnazione deve soddisfare tre assiomi fondamentali:
1.  **Non-negatività:** La probabilità è un numero positivo o nullo.
    $$P(A) \ge 0$$
2.  **Normalizzazione:** La probabilità che viene riferita a un evento che comprende tutti gli elementi di un esperimento, cioè l'evento associato a tutto lo spazio campionario $\Omega$, è unitaria.
    $$P(\Omega) = 1$$
    Nel nostro esempio, l'evento "il neurone spara oppure il neurone non spara" deve avere probabilità $1$ per definizione.
3.  **Additività:** Per eventi **mutuamente esclusivi** (disgiunti), la probabilità dell'unione è la somma delle probabilità.
    $$P(A \cup B \cup C) = P(A) + P(B) + P(C)$$
    dove $A, B, C$ sono eventi disgiunti (la loro intersezione è l'insieme nullo). L'interpretazione insiemistica è utile: la probabilità è come se fosse l'area sottesa dai *blob* degli insiemi. Se gli insiemi sono disgiunti, l'area dell'unione è la somma delle aree. È cruciale che gli eventi siano disgiunti, altrimenti l'intersezione verrebbe contata due volte.

Da queste definizioni segue che, poiché la probabilità associata all'evento nullo è $0$ (non ha area sottesa), la probabilità del complementare di un evento $A$, indicato con $A^c$, è:
$$P(A^c) = 1 - P(A)$$
Nel caso della moneta, se $H$ è testa e $T$ è croce, $T$ è il complementare di $H$ e viceversa. Se $P(H) = 1/3$, allora $P(T) = 1 - 1/3 = 2/3$. La matematica funziona, purché si applichino correttamente le definizioni.

## Variabili Aleatorie e Descrizione Numerica degli Eventi

### Variabili Aleatorie (Random Variables)

È una definizione molto utile in quanto permette di collegare l'astrazione della probabilità a esperimenti concreti, attribuendo una quantità, chiamata **variabile**, ai risultati dell'esperimento. Una variabile aleatoria, denotata con una lettera maiuscola come $X$, è il risultato dell'applicazione di una funzione che assegna un numero reale (o intero, o complesso) a ciascun evento. È una sorta di riassunto numerico dell'esito dell'esperimento.

La notazione non è particolarmente difficile: la variabile aleatoria è denotata con un simbolo in maiuscolo ($X$), mentre un simbolo in minuscolo ($x$) indica il suo valore potenziale o una specifica **realizzazione**. Se $X$ è il costo dell'esperimento, $x$ è il particolare valore (ad esempio, $1000$ euro o $0$). La realizzazione è ciò che è effettivamente capitato in un momento specifico, mentre la variabile aleatoria è un concetto astratto.

### Funzione di Distribuzione Cumulativa (CDF)

Dato il legame con la probabilità che abbiamo assegnato a ogni evento, possiamo definire una funzione chiamata **Funzione di Distribuzione Cumulativa** (CDF, *Cumulative Distribution Function*), che è una definizione puramente convenzionale ma utile.
$$F_X(x) = P(X \le x)$$
Essa attribuisce un valore (una probabilità) all'evento in cui la variabile aleatoria $X$ sia minore o uguale a un valore numerico $x$ prefissato.

Per esempio, in Python, MATLAB, o Julia, avete la possibilità di generare numeri pseudo-casuali. Se avete un comando $R = \text{RAND}$ che assegna a $R$ un numero pseudo-casuale compreso tra $0$ e $1$, potrebbe essere interessante chiedersi qual è la probabilità che $R$ sia minore o uguale a $0.5$. Per una variabile uniforme (dove tutti i punti hanno la stessa probabilità in quell'intervallo), questa probabilità è proprio $0.5$. Se $x=1$, la probabilità che $R \le 1$ è $1$; se $x=0$, la probabilità che $R \le 0$ è $0$. Questa CDF, in questo caso, è uniforme, ma in generale il concetto è che si definisce una funzione attribuita a caratteristiche elementari di quella variabile aleatoria.

Tornando all'esempio neurofisiologico (la scimmia), potrei voler monitorare, nello stesso esperimento, il valore numerico del potenziale di membrana ($V_m$) in un particolare istante, ad esempio $4$ millisecondi dopo l'inizio dello stimolo. Ho eseguito $50$ di questi esperimenti. Il valore numerico del potenziale di membrana in quel punto è una variabile aleatoria che posso definire. Se io guardo i puntini (che intersecano le diverse realizzazioni dell'esperimento), noto che nella maggior parte delle volte i valori di $V_m$ sono bassi (attorno a $-50$ mV), mentre sono rari i valori alti (attorno a $+20$ mV). I valori bassi sono molto più **densi**.

Se calcolo la probabilità che $V_m$ sia sotto un certo valore, ad esempio $+40$ mV, la probabilità è unitaria, perché tutti i puntini stanno sotto quella soglia. Al contrario, se abbasso il valore, per esempio a $-40$ mV, vedo che molti puntini stanno sotto e pochi stanno sopra. Quindi, la probabilità $P(V_m \le -40 \text{mV})$ sarà alta (ad esempio, $0.8$ o $80\%$).

#### Proprietà della CDF

Per come è definita, la CDF $F_X(x)$ è una funzione **monotona crescente** e **sempre positiva**. Quando l'argomento $x$ è molto piccolo, $F_X(x)$ tende a $0$; quando il valore $x$ è molto grande, $F_X(x)$ tende a essere unitaria ($1$).

Un'altra proprietà interessante, che deriva sempre dall'insiemistica, è che la probabilità che $X$ sia compresa in un intervallo $(a, b]$, ovvero $P(a < X \le b)$, può essere espressa come la differenza di due probabilità cumulative:
$$P(a < X \le b) = P(X \le b) - P(X \le a) = F_X(b) - F_X(a)$$
Questa relazione (la differenza delle funzioni di distribuzione cumulativa calcolate agli estremi dell'intervallo) ci ricorda un po' il rapporto incrementale, ed è lì che andiamo a parare.

### Funzione di Densità di Probabilità (PDF)

Sebbene la CDF sia utilissima, spesso è più comodo sapere qual è la probabilità di trovare $X$ in un intervallo discretizzato. Se l'intervallo è molto, molto piccolo, la probabilità sarà nulla, perché per una variabile continua, la probabilità che essa cada esattamente in un punto (un intervallo di misura nulla) è zero. Per passare dalla cumulativa (CDF) a questa nuova descrizione, in cui di fatto si conta quante volte la realizzazione è capitata in un *bin* o *cestello*, si utilizza il concetto di **istogramma**. Per ogni intervallo, si conta la frazione di realizzazioni che cadono tra gli estremi $A$ e $B$.

Questo processo ci porta alla **Funzione di Densità di Probabilità** (PDF, *Probability Density Function*), indicata con $f_X(x)$. Si parla di **densità** perché si tratta di un numero per unità della variabile $x$. Sto dicendo che per ogni millivolt (o decina di millivolt) ho una certa frequenza di occorrenza per unità di misura. Questa densità può cambiare nello spazio di $x$ (essere più densa in certi punti, come si vede nell'istogramma del $V_m$).

Per ritornare alla definizione, devo probabilmente dividere la probabilità per un certo $\Delta x$. Se considero l'intervallo infinitesimo $[x, x+\Delta x]$, possiamo scrivere:
$$P(x < X \le x + \Delta x) = F_X(x + \Delta x) - F_X(x)$$
Permettendomi di dividere entrambi i membri per $\Delta x$ e applicando il limite per $\Delta x \to 0$, definisco la PDF come la derivata rispetto a $x$ della CDF:
$$f_X(x) = \lim_{\Delta x \to 0} \frac{F_X(x + \Delta x) - F_X(x)}{\Delta x} = \frac{d F_X(x)}{d x}$$
Se la CDF per una variabile uniforme tra $0$ e $1$ era una rampa (lineare), la sua derivata (la PDF) sarà un rettangolo con ampiezza $1$ e supporto (il *range* dove la funzione è diversa da zero) tra $0$ e $1$.

Il significato della PDF è dirvi qual è la probabilità che la variabile aleatoria si trovi in un certo intorno infinitesimo, **pesata** da questo $\Delta x$. È una densità.

#### Proprietà della PDF

1.  **Normalizzazione:** Poiché la cumulativa raggiunge $1$ per $x \to +\infty$, per la definizione di derivata e integrale, l'area sottesa dalla PDF deve essere unitaria:
    $$\int_{-\infty}^{+\infty} f_X(x) d x = 1$$
    Questo è facile da memorizzare, ad esempio pensando all'area sottesa dalla Gaussiana o dalla distribuzione uniforme.
2.  **Non-negatività:** Dato che la CDF $F_X(x)$ è una funzione monotona crescente, la sua derivata $f_X(x)$ non può essere altro che positiva, ovvero **$f_X(x) \ge 0$**.

### Variabili Aleatorie a Valori Discreti

Le variabili aleatorie a valori discreti sono funzioni che assumono solo un numero finito o, al più, infinito ma **numerabile** (*countable*), non con la cardinalità del continuo. Devono essere quantità mappabili all'insieme degli interi.

In questo caso, le probabilità assumono la forma di probabilità che la variabile assuma un particolare valore $x_i$: $P(X=x_i) = p_i$. Per le proprietà che derivano dagli assiomi, la somma di queste probabilità è unitaria:
$$\sum_{i} p_i = 1$$
Questa somma è l'analogo discreto dell'integrale continuo che impone l'unitarietà dell'area sottesa dalla PDF.

## La Necessità e l'Evidenza Empirica della Stocasticità in Neuroscienze

La caratteristica di **rumorosità**, o di descrizione aleatoria, stocastica, non deterministica e non prevedibile, è un fenomeno onnipresente in neuroscienze, manifestandosi a tantissimi livelli.

-   **Livello Molecolare/Canale:** La permeabilità di membrana è un processo stocastico e presenta fluttuazioni (il cosiddetto "rumore di membrana").
-   **Livello Subcellulare:** Tutte le reazioni biochimiche, come quelle che coinvolgono le proteine G, i recettori sensoriali (opsine nella retina, meccanocettori), non sono processi rigorosamente deterministici, ma sono descritte meglio in termini statistici.
-   **Livello Neuronale:** L'eccitabilità neuronale. Come ho mostrato con la simulazione, il *firing* di un neurone può avvenire con *failure* (non sparo) anche se le condizioni sono identiche. Anche la **latenza** (il ritardo) con cui avviene lo *spike* può cambiare e può essere descritta come una variabile aleatoria.
-   **Livello Sinaptico:** Il rilascio sinaptico è un processo stocastico, sia per la natura intrinseca stocastica dei canali ionici (ad esempio, il Calcio) sia per l'aleatorietà della diffusione del neurotrasmettitore nel *cleft* sinaptico.
-   **Livello di Attuazione:** Tutto ciò continua fino all'attuazione motoria, dove il rilascio di neurotrasmettitore alla giunzione neuromuscolare è nuovamente una quantità aleatoria.

### Evidenza Storica e Sperimentale

Uno degli articoli pionieristici, di quasi 100 anni fa, è quello di **Katz** (premio Nobel), il quale registrò l'attività nei muscoli (l'effetto dell'innervazione alla giunzione neuromuscolare) e descrisse una serie di segnali elettrofisiologici aleatori che chiamò **Miniature End Plate Potential** (mEPP), oggi li chiameremmo potenziali post-sinaptici in miniatura. Questi eventi sono in miniatura rispetto ai normali eventi sinaptici innescati da un'attivazione nervosa (un atto volontario o una stimolazione elettrica). Katz osservò le **ampiezze** di questi mEPP in diverse realizzazioni e calcolò il loro istogramma (la PDF). Il risultato non era un unico valore, ma una distribuzione variabile, descritta meglio come una variabile aleatoria.

In un ulteriore articolo pioneristico del 1998, **Shadlen e Newsome** portarono all'attenzione della comunità il fatto che l'attività corticale *in vivo* (registrata, ad esempio, in scimmie a cui viene mostrato lo stesso stimolo visivo) non è per niente riproducibile su esperimenti successivi e indipendenti. C'è una dispersione notevole, una forte impredicibilità. La distribuzione degli intervalli tra gli spike (*Inter-Spike Interval*, ISI) è una funzione che ricorda una **densità di distribuzione esponenziale**, e la relazione tra la media e la varianza della scarica sembra essere proporzionale. Queste non sono caratteristiche di un processo deterministico.

Un esperimento che abbiamo già discusso assieme mostra come la risposta di un neurone in una fettina di cervello (*in vitro*) a un *input* di corrente sia regolarissima e quasi perfettamente riproducibile, mentre la stessa iniezione di corrente nello stesso tipo di neurone *in vivo* (all'interno della corteccia intatta) produce un treno di *spike* completamente diverso, **irregolare**. Questo è dovuto alle fluttuazioni nel potenziale di membrana sottosoglia, che assomiglia a un *random walk* (cammino casuale), varia in modo casuale ed è asincrona. L'attività elettrica del neurone sembra essere profondamente alterata dall'attività sinaptica di rete.

### Introduzione ai Processi Stocastici Neuronali

L'idea è quella di inquadrare rapidamente i processi stocastici per raccontarvi di due processi specifici: il processo di **Ornstein-Uhlenbeck** e il processo di **Stein**.

Il processo di Ornstein-Uhlenbeck, in questo contesto, è strettamente collegato al modello **integra e spara** (*Integrate-and-Fire*). Esso ha a che fare con l'equazione di bilancio della carica e l'integrazione di un segnale nel tempo. Il potenziale di membrana $V$ è approssimativamente l'integrale di tutte le correnti sinaptiche. Il processo di Stein, invece, è più legato all'attività sinaptica impulsiva.

L'osservazione di una corrispondenza tra processi stocastici e componenti essenziali nella descrizione neurobiologica delle caratteristiche elettriche dei neuroni ci permette di spiegare in modo chiaro l'**approssimazione di diffusione**. Questa approssimazione, che vi menziono per la seconda o terza volta, ha a che fare con il progressivo cambiamento di un processo che sembra essere **impulsivo** (legato all'attivazione casuale e irregolare delle sinapsi) in un processo **continuo**. Man mano che queste sinapsi si sommano (perché il comportamento di tante sinapsi è integrato dalla membrana post-sinaptica), il processo che ne deriva, anche con un numero non spaventosamente elevato di sinapsi (ad esempio, $100, 200, 500$), tende a diventare un processo **continuo**, ovvero un **processo di diffusione** (*diffusion process*), anziché un **processo puntiforme** (*point process*).



## Formalismo dei Processi Stocastici

Riprendiamo ora la discussione sui processi stocastici, necessari per modellare fenomeni come le fluttuazioni del potenziale di membrana o la variabilità degli intervalli inter-spike. Un **processo stocastico** non è altro che una famiglia di variabili aleatorie $X(t)$, indicizzate da un parametro $t$, che nel nostro caso rappresenta il tempo.

### La Natura della Traiettoria

Mentre una singola variabile aleatoria $X$ fornisce un valore (la realizzazione) per un singolo esperimento, un processo stocastico fornisce un'intera **traiettoria** (o *sample path*) nel tempo, $X(t)$, per ogni specifica realizzazione dell'esperimento. Quindi, una singola prova del nostro esperimento, come la misurazione del potenziale di membrana $V_m(t)$ nel tempo in una singola scimmia, produce una singola traiettoria (una **realizzazione**) del processo stocastico $V_m(t)$. Ripetere l'esperimento genera un *ensemble* di traiettorie diverse, come quelle che abbiamo osservato nella figura di Shadlen e Newsome.

Il formalismo più generale per descrivere l'evoluzione di questi processi è dato dalla probabilità congiunta che la variabile assuma una sequenza di valori in istanti di tempo successivi:
$$P(X(t_1)=x_1, X(t_2)=x_2, \ldots, X(t_n)=x_n)$$
Tuttavia, questo formalismo è estremamente complesso.

### Processi di Markov e Equazioni di Master

In neuroscienze e in biologia, siamo spesso interessati a particolari classi di processi stocastici. In particolare, consideriamo i **processi di Markov**, che sono caratterizzati dalla proprietà che l'evoluzione futura del sistema dipende **solo** dallo stato presente, e non dalla sua storia passata. Questa è la cosiddetta **proprietà di Markov**, che dal punto di vista matematico semplifica enormemente la descrizione:
$$P(X(t_{n+1})=x_{n+1} | X(t_n)=x_n, \ldots, X(t_1)=x_1) = P(X(t_{n+1})=x_{n+1} | X(t_n)=x_n)$$
[Inference] In neuroscienze, questa è un'ipotesi molto forte ma spesso valida in prima approssimazione, specialmente per i canali ionici che non mostrano un'isteresi a lungo termine.

Per i processi stocastici in tempo discreto (come il processo di Stein, che vedremo), si utilizzano le **equazioni di Master**, che descrivono l'evoluzione temporale della probabilità che il sistema si trovi in un certo stato:
$$\frac{d P_i(t)}{d t} = \sum_{j \neq i} \left[ W_{i j} P_j(t) - W_{j i} P_i(t) \right]$$
dove $P_i(t)$ è la probabilità che il sistema sia nello stato $i$ al tempo $t$, e $W_{i j}$ è la probabilità di transizione per unità di tempo dallo stato $j$ allo stato $i$. [Inference] Questo è esattamente il formalismo che abbiamo utilizzato, per esempio, per descrivere la dinamica di *gating* di un canale ionico, dove gli stati $i$ e $j$ sono gli stati chiusi, aperti o inattivati.

### Il Processo di Wiener (Moto Browniano)

Il processo più elementare che è alla base di tutta la teoria della diffusione è il **processo di Wiener** (o **moto Browniano**). Questo è il limite del *Random Walk* (*cammino casuale*), ed è la descrizione del movimento di una particella soggetta a un numero enorme di urti rapidi e indipendenti.

Il processo di Wiener, denotato con $W(t)$, è caratterizzato da:
1.  $W(0)=0$.
2.  Incrementi stazionari e indipendenti: $W(t+\Delta t) - W(t)$ è indipendente dai valori precedenti e dipende solo da $\Delta t$.
3.  Gli incrementi sono distribuiti Gaussianamente con media nulla e varianza proporzionale a $\Delta t$:
    $$W(t+\Delta t) - W(t) \sim \mathcal{N}(0, \sigma^2 \Delta t)$$
Il processo di Wiener, pur essendo formalmente continuo, ha la peculiare proprietà che le sue traiettorie sono **ovunque non differenziabili**. [Inference] Dal punto di vista della modellistica neuroscientifica, questo significa che la "velocità" del processo non è ben definita, ma le fluttuazioni sono così rapide da essere, in un certo senso, "infinite".

### Il Processo di Ornstein-Uhlenbeck (OU) come Modello per il Rumore di Membrana

Per modellare il potenziale di membrana $V_m(t)$, la semplice diffusione (Wiener) non è sufficiente. Dobbiamo incorporare una caratteristica fondamentale dei neuroni: la **corrente di perdita** (*leak current*), che tende a riportare il potenziale di membrana verso il potenziale di riposo $E_L$.

[Inference] Per i neuroni non-spiking (sotto soglia), l'equazione del potenziale di membrana in presenza di rumore può essere derivata dall'equazione del circuito equivalente $RC$ (Resistenza-Capacità):
$$C_m \frac{d V_m}{d t} = -g_L (V_m - E_L) + I_{\text{syn}}(t)$$
dove $I_{\text{syn}}(t)$ è la corrente sinaptica totale fluttuante, che è la sorgente del rumore.

Il **Processo di Ornstein-Uhlenbeck (OU)** è la soluzione stocastica di un'equazione differenziale che incorpora un termine di "deriva" (*drift*) lineare, che tira la variabile verso un valore medio, e un termine di "rumore" diffusivo:
$$d X(t) = -\lambda X(t) d t + \sigma d W(t)$$
[Inference] Se identifichiamo $X(t)$ con la deviazione del potenziale di membrana dal potenziale di riposo, $V_m(t) - E_L$, e scegliamo $\lambda = 1/\tau_m$ (dove $\tau_m$ è la costante di tempo di membrana, $C_m/g_L$), l'equazione di Ornstein-Uhlenbeck diventa il **modello Integrate-and-Fire stocastico sottosoglia**.

Il termine $-\lambda X(t) d t$ modella il ritorno esponenziale al potenziale di riposo, che ha un ruolo stabilizzante, mentre $\sigma d W(t)$ modella le fluttuazioni aleatorie continue (il rumore). [Inference] Il parametro $\sigma$ (spesso $\sigma^2$) è il coefficiente di diffusione, che quantifica l'intensità delle fluttuazioni.

Questo processo è caratterizzato da una distribuzione stazionaria gaussiana:
$$X \sim \mathcal{N}\left(0, \frac{\sigma^2}{2\lambda}\right)$$
[Inference] Questo è estremamente importante: in assenza di *spike* e in regime stazionario, il potenziale di membrana del neurone sarà distribuito in modo **Gaussiano** attorno a $E_L$, con una varianza che dipende sia dall'intensità del rumore ($\sigma^2$) sia dalla costante di tempo ($\lambda$) che lo attenua.

### Il Processo di Stein (Il Modello del Tasso di Spike)

Il processo di Ornstein-Uhlenbeck descrive la dinamica sottosoglia come un processo continuo di diffusione. Tuttavia, il *vero* rumore in un neurone è causato dagli eventi sinaptici discreti, che sono **impulsi**.

Il **Processo di Stein** (o *Integrate-and-Fire* con ingresso di Poisson) si basa sull'idea che gli *input* sinaptici (eccitatori ed inibitori) arrivano al neurone come **Processi di Poisson**, ovvero come eventi discreti, indipendenti, che avvengono con una certa frequenza costante (tasso $\lambda_{\text{exc}}$ o $\lambda_{\text{inh}}$).

Formalmente, questo è un modello di **processo puntiforme** (*point process*). Il potenziale di membrana $V_m(t)$ evolve secondo l'equazione $RC$ ma l'ingresso sinaptico $I_{\text{syn}}(t)$ è una somma di impulsi (detti impulsi di **Dirac**), che avvengono in tempi aleatori $t_k$:
$$C_m \frac{d V_m}{d t} = -g_L (V_m - E_L) + \sum_{k} q_k \delta(t-t_k)$$
dove $q_k$ è l'ampiezza (peso) del singolo *input* sinaptico e $\delta(t-t_k)$ è la funzione Delta di Dirac che modella l'impulso istantaneo.

[Inference] Questo modello è più fedele alla biologia del singolo *input* sinaptico, ma è matematicamente più complesso da risolvere analiticamente, specialmente quando si deve calcolare la distribuzione del tempo di *first passage* (il tempo necessario per raggiungere la soglia di *firing* $\theta$).

## L'Approssimazione di Diffusione: Dal Discreto al Continuo

L'osservazione fondamentale, che collega il processo di Stein (impulsi di Poisson) al processo di Ornstein-Uhlenbeck (rumore Gaussiano continuo), è l'**Approssimazione di Diffusione**.

L'approssimazione è valida quando il neurone riceve:
1.  Un **numero molto grande** di sinapsi (ad esempio, $N \to \infty$).
2.  Con **ampiezze sinaptiche molto piccole** (ad esempio, $q_k \to 0$).
3.  Con un **tasso di arrivo molto elevato** (ad esempio, $\lambda_{\text{syn}} \to \infty$).

Quando queste condizioni sono soddisfatte, il teorema del limite centrale (in particolare, una sua generalizzazione, nota come *Teorema di Donsker* o *invarianza funzionale*) ci dice che la somma degli impulsi sinaptici discreti e indipendenti, ognuno con un effetto trascurabile singolarmente, può essere approssimata da un rumore continuo e Gaussiano, che è proprio il termine diffusivo $\sigma d W(t)$.

In termini di parametri, l'integrale degli *input* impulsivi discreti si approssima al termine stocastico dell'equazione di Ornstein-Uhlenbeck:
$$\sum_{k} q_k \delta(t-t_k) \approx \mu_{\text{eff}} + \sigma_{\text{eff}} \frac{d W(t)}{d t}$$
dove $\mu_{\text{eff}}$ (la deriva efficace) e $\sigma^2_{\text{eff}}$ (il coefficiente di diffusione efficace) dipendono dalla media e dalla varianza dei pesi sinaptici $q_k$ e dai tassi di arrivo $\lambda_{\text{syn}}$ degli *input* di Poisson.

[Inference] La formula che ne deriva e che lega i parametri del processo sinaptico discreto a quelli del processo di diffusione continuo è la seguente:
$$\mu_{\text{eff}} = \lambda_{\text{exc}} \langle q_{\text{exc}} \rangle - \lambda_{\text{inh}} \langle q_{\text{inh}} \rangle$$
$$\sigma^2_{\text{eff}} = \lambda_{\text{exc}} \langle q^2_{\text{exc}} \rangle + \lambda_{\text{inh}} \langle q^2_{\text{inh}} \rangle$$
Queste sono le condizioni di **continuità** che permettono di sostituire la dinamica discreta e complessa del potenziale di membrana di Stein con la dinamica continua e analiticamente più trattabile del potenziale di membrana di Ornstein-Uhlenbeck. [Inference] Questo passaggio è la giustificazione teorica per l'utilizzo del rumore Gaussiano nelle simulazioni Integrate-and-Fire in presenza di un'attività sinaptica intensa.


## Equivalenza tra Processi Continui e Discreti in Neuroscienze

Si riprende la discussione sul piano della lezione, finalizzato alla dimostrazione dell'**approssimazione di diffusione**. Il processo di Ornstein-Uhlenbeck (OU) e il processo di Stein (Shot Noise) presentano non solo somiglianze intriganti, ma le loro espressioni formali sembrano rispecchiare le formule matematiche esatte che abbiamo analizzato in termini di dinamica Integrate-and-Fire e corrente sinaptica.

L'approssimazione di diffusione ci condurrà a stabilire la relazione tra la media ($\mu$) e la deviazione standard ($\sigma$) del processo stocastico Gaussiano continuo (o diffusivo) e i parametri del processo sottostante. I parametri chiave del processo discreto che modella l'input sinaptico sono:
1.  L'ampiezza del salto ($j$).
2.  La costante di tempo di *decay* ($\tau$), che caratterizza la cinetica sinaptica (ad esempio, $\tau \approx 1-2$ ms per i recettori AMPA, $20-50$ ms per NMDA, o $10$ ms per $\text{GABA}_\text{A}$).
3.  La frequenza degli eventi ($\lambda$).

Il parametro $\lambda$ si riferisce alla quantità di afferenti: in presenza di un numero elevato di afferenti ($10, 20, 50, 100$ ecc.), la situazione può essere approssimata statisticamente a un unico afferente con una frequenza di arrivo molto maggiore, come una sovrapposizione algebrica di treni di $\Delta$ di Dirac. La media del processo continuo (che emula il discreto) dipende in modo proporzionale da $j, \lambda, \tau$, mentre la varianza ($\sigma^2$) ha la forma $\sigma^2 = j^2 \lambda \tau / 2$, dove si nota che l'ampiezza del salto ($j$) compare al quadrato e il termine contiene il fattore $1/2$. Il nostro obiettivo è dimostrare questa equivalenza.

### Il Rumore Gaussiano Bianco ($\xi$)

Introduco formalmente il concetto di **Rumore Gaussiano Bianco** ($\xi(t)$), con una notazione che, sebbene abusiva in senso matematico rigoroso, è essenziale per il nostro contesto ingegneristico. Il processo è definito **Gaussiano** in quanto ad ogni istante $t$ è una variabile aleatoria distribuita secondo una **distribuzione Gaussiana** (con una certa media $\mu$ e varianza $\sigma^2$).
[Inference] La Funzione di Densità di Probabilità (PDF) per una variabile normale con media $\mu$ e varianza $\sigma^2$ è:
$$f_X(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-(x-\mu)^2/(2\sigma^2)}$$
Il processo è definito **Bianco** perché, nel dominio trasformato delle frequenze, la sua **densità spettrale di potenza (PSD)** è costante per tutte le frequenze (analogia con la luce bianca). Nel dominio del tempo, questo implica che il rumore bianco ha **fluttuazioni arbitrarie** e i suoi valori $\xi(t_1)$ e $\xi(t_2)$ a due istanti $t_1$ e $t_2$ distinti sono **variabili aleatorie indipendenti**, ovvero non hanno alcuna correlazione.

Questa assenza di correlazione è espressa quantitativamente dalla **funzione di autocorrelazione** $R_{\xi}(\tau)$:
$$R_{\xi}(\tau) = \mathbb{E}[\xi(t) \xi(t+\tau)] = A \cdot \delta(\tau)$$
dove $\tau = t_2 - t_1$ è l'intervallo temporale. L'autocorrelazione è zero ovunque tranne quando i due punti sono esattamente coincidenti ($\tau=0$), dove assume il valore della varianza del processo. Il fatto che la PSD sia costante nel dominio delle frequenze è equivalente al fatto che l'autocorrelazione sia una $\Delta$ di Dirac nel dominio del tempo (Teorema di Wiener-Khinchin). [Inference] In un contesto biologico, il rumore non è mai puramente bianco e ha sempre una scala temporale di autocorrelazione finita, che risulta in un **rumore colorato**.

### Il Processo di Ornstein-Uhlenbeck (OU) e l'Integrazione Stocastica

Introduco ora la definizione didatticamente conveniente del **Processo di Ornstein-Uhlenbeck (OU)**, che è un processo a tempo e valori continui, noto anche come *rumore colorato*. Esso è definito come la soluzione dell'equazione differenziale stocastica che modella l'integrazione di un input additivo:
$$\tau \frac{d X}{d t} = -\left( X(t) - \mu \right) + \sigma \sqrt{2\tau} \xi(t)$$
Si tratta di un'equazione differenziale ordinaria del primo ordine con un termine di ingresso stocastico $\xi(t)$. Si noti che questo è formalmente un abuso, poiché il rumore bianco $\xi(t)$ non è differenziabile, e l'integrazione di tale equazione richiederebbe l'uso dell'**integrale di Itô**, una complicazione matematica che qui tralasciamo. La presenza del fattore $\sqrt{2\tau}$ nel termine diffusivo è convenzionale per assicurare che il processo OU abbia in regime stazionario una varianza $\sigma^2$.

#### Soluzioni Statistiche Stazionarie del Processo OU
Per calcolare la **media** $m(t) = \mathbb{E}[X(t)]$, si applica il valore atteso $\mathbb{E}[\cdot]$ a entrambi i membri dell'equazione di OU, sfruttando la sua linearità e il fatto che $\mathbb{E}[\xi(t)] = 0$. In questo modo si ottiene l'equazione differenziale deterministica per la media:
$$\tau \frac{d m}{d t} = -\left( m(t) - \mu \right)$$
La soluzione in **regime stazionario** ($\frac{d m}{d t} = 0$) è semplicemente $m_{\infty} = \mu$.

Il calcolo della **varianza** $S^2(t)$ e della **funzione di autocovarianza** $C_X(\tau) = \mathbb{E}[(X(t) - \mu)(X(t+\tau) - \mu)]$ richiede una manipolazione più attenta (ad esempio, moltiplicando l'equazione per $X(t)$ e applicando il valore atteso). Il risultato fondamentale in regime stazionario è:
$$\text{Var}[X] = \sigma^2$$
$$C_X(\tau) = \sigma^2 e^{-|\tau|/\tau}$$
Questa autocovarianza è un esponenziale decrescente e simmetrico, caratteristico di un rumore **colorato** (non bianco) e non correlato istantaneamente.

#### La Sfida della Simulazione Numerica del Processo OU

Per simulare il processo OU, è possibile utilizzare la **soluzione analitica esatta** scritta in forma iterativa. Se si fa un'analisi corretta, invece di ricorrere all'approssimazione di Eulero (che è il metodo peggiore in questo contesto), si ottiene un'espressione iterativa che, per un passo temporale $\Delta t$, garantisce l'esattezza statistica:
$$X(t+\Delta t) = X(t) e^{-\Delta t/\tau} + \mu (1 - e^{-\Delta t/\tau}) + \sigma \sqrt{\tau (1 - e^{-2\Delta t/\tau})} \cdot \zeta_k$$
dove $\zeta_k$ è una variabile aleatoria Gaussiana con media nulla e varianza unitaria.

L'errore storico e didattico risiede nell'applicazione ingenua del Metodo di Eulero standard, che approssima il termine diffusivo $\sigma \sqrt{2\tau} \xi(t)$ con un termine che scala linearmente con $\Delta t$. Applicando invece lo sviluppo in serie di Taylor ($e^{-x} \approx 1-x$) alla soluzione analitica per $\Delta t \ll \tau$, si ottiene la forma del **Metodo di Eulero Stocastico Corretto**:
$$X(t+\Delta t) \approx X(t) + \frac{\Delta t}{\tau} (\mu - X(t)) + \sigma \sqrt{2 \Delta t} \cdot \zeta_k$$
L'osservazione critica, che ha storicamente ingannato molti, è che il termine diffusivo **non** scala con $\Delta t$, ma con **$\sqrt{\Delta t}$**. L'utilizzo di $\Delta t$ al posto di $\sqrt{\Delta t}$ altera drasticamente le proprietà statistiche macroscopiche (come la varianza stazionaria) del processo simulato, in un modo che dipende dalla scelta del passo temporale. Questo fattore $\sqrt{\Delta t}$ è legato alla natura non differenziabile del rumore bianco e assicura che il processo simulato sia un'approssimazione corretta.

### Il Processo di Stein (Shot Noise)

Il **Processo di Stein** è un modello alternativo e più biologicamente fedele che descrive la corrente sinaptica come una sequenza di impulsi discreti (*Shot Noise*). Esso è definito come la soluzione dell'equazione differenziale stocastica:
$$\tau \frac{d Y}{d t} = -Y(t) + \tau \sum_{k} j \cdot \delta(t-T_k)$$
dove $Y(t)$ è il processo, $j$ è l'ampiezza del salto (*jump*), e i tempi di occorrenza $T_k$ sono stocastici, tipicamente generati da un **processo di Poisson omogeneo** con tasso $\lambda$. Un processo di Poisson omogeneo è caratterizzato da:
1.  **Intervalli tra eventi** $\Delta T_k$ che sono variabili aleatorie distribuite con una **distribuzione esponenziale**.
2.  La probabilità di avere un evento in un intervallo infinitesimo $\Delta t$ è data da $P(\text{evento in } \Delta t) \approx \lambda \Delta t$.

Integrando l'equazione di Stein, si nota che l'integrale del termine impulsivo $\sum_{k} \delta(t-T_k)$ è il **Processo di Conteggio Poissoniano** $N(t)$, che conta il numero totale di eventi fino al tempo $t$. [Inference] Questo processo $N(t)$ è una variabile discreta a tempo continuo, che è più "dolce" rispetto al treno di $\Delta$ di Dirac, ma comunque presenta discontinuità di prima specie.

## L'Approssimazione di Diffusione e l'Equivalenza Finale

Il problema si riduce a stabilire le condizioni per cui il **Processo di Conteggio Poissoniano** (Integrale di Stein) possa essere approssimato dal **Processo di Wiener** (Integrale di OU).

#### La Transizione Discreta-Continua (Poisson $\to$ Gaussiana)

Il teorema del limite centrale, applicato ai processi stocastici, stabilisce che una **distribuzione Binomiale** (che descrive il numero di eventi in $M$ *bin* discreti) converge alla **distribuzione di Poisson** quando il passo temporale $\Delta t \to 0$. A sua volta, la distribuzione di Poisson $N \sim \text{Poisson}(\lambda t)$ tende alla **distribuzione Gaussiana** quando il suo parametro $\lambda t$ è **molto grande** (ovvero, la frequenza $\lambda$ è molto elevata).

In questo limite, si conoscono la media e la varianza della variabile Gaussiana limite:
$$\mathbb{E}[N] = \lambda t$$
$$\text{Var}[N] = \lambda t$$

#### La Ricetta per l'Equivalenza Statistica

Perché i due processi (Stein e OU) siano statisticamente equivalenti, la loro media e varianza integrate per un lungo tempo $t$ devono corrispondere.

1.  **Media degli Input Integrati:**
    $$\mathbb{E}[\text{Input}_{\text{Stein}}] = j \lambda t \quad \equiv \quad \mathbb{E}[\text{Input}_{\text{OU}}] = \mu \frac{t}{\tau}$$
    Da cui si ricava il parametro di deriva (media) per l'OU in funzione dei parametri di Stein:
    $$\mu = j \lambda \tau$$

2.  **Varianza degli Input Integrati:**
    $$\text{Var}[\text{Input}_{\text{Stein}}] = j^2 \lambda t \quad \equiv \quad \text{Var}[\text{Input}_{\text{OU}}] = \sigma^2 \frac{2 t}{\tau}$$
    Da cui si ricava il parametro di varianza per l'OU:
    $$\sigma^2 = \frac{1}{2} j^2 \lambda \tau$$

Queste formule rappresentano la **ricetta esatta** per emulare il processo di Stein (con salti $j$ e tasso $\lambda$) con il processo di Ornstein-Uhlenbeck (con media $\mu$ e varianza $\sigma^2$).

L'equivalenza è valida solo nelle condizioni in cui il processo di Poisson tende a essere Gaussiano: il tasso di eventi $\lambda$ deve essere **molto elevato** e l'ampiezza dei salti $j$ deve essere **molto piccola** (infinitesimale), in modo tale che i prodotti $j \lambda \tau$ e $j^2 \lambda \tau$ rimangano finiti. Se l'ampiezza dei salti $j$ è molto piccola, l'integrazione degli impulsi (Shot Noise) risulta in una curva che è praticamente continua, giustificando la definizione di **processo diffusivo** continuo anche per la dinamica neuronale che è intrinsecamente impulsiva.
