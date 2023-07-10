# orientamento sole-strada

Questo progetto consiste nello sviluppo di uno strumento di calcolo che permette di individuare tratti ottimali del percorso stradale di un area geografica rispetto al movimento *apparente* del Sole.

## motivazione

Da diversi anni sono presenti installazioni di pannelli fotovoltaici lungo il percorso stradale del territorio.	Tali installazioni sono talvolta abbinate a necessità di schermatura dal rumore degli autoveicoli e, a seconda della lunghezza, permettono di generare notevoli quantità di energia elettrica.

L'installazione di tali strisce di pannelli lungo il percorso stradale, si effettua con strutture opportune ubicate all'estremità del manto stradale, solitamente al di là del guard-rail. L'orientamento dei pannelli è fisso e studiato in modo da ottimizzare la loro resa durante l'anno.

La costruzione di tali strutture avviene principalmente tramite risorse pubbliche. L'attribuzione di tali fondi avviene tramite bando.  L'accesso ai bandi avviene consegnando un studio di progetto. In questo contesto, il presente lavoro vuole creare uno strumento di calcolo che faciliti la preparazione del progetto per tali installazioni.

## oggetto del calcolo

Considerato un tratto di percorso stradale, si può calcolare la direzione perpendicolare ai sensi di marcia. Tale direzione (vettore) è interessante in quanto permette di stimare se la direzione della strada sia ottimale rispetto al movimento del Sole.

Non tutte le strade sono adatte all'installazione di strisce di pannelli. Si tratta principalmente di strade in periferia dei entri urbani, e caratterizzate da sufficiente spazio attorno al manto stradale.


# Procedimento di calcolo

In prima approssimazione, l'orientamento ottimale di un pannello rispetto al movimento apparente del Sole, può essere espresso tramite un sistema di coordinate orizzontali in cui si utilizzano gli angoli **azimut** e **altezza**:

+ l'azimut dipende dalla direzione in cui punta il pannello in senso laterale rispetto alla linea dell'orizzonte.
+ l'altezza dipende, invece, dalla distanza in senso verticale rispetto alla linea dell'orizzonte. Tale posizione dipende dal punto di massima altezza del Sole. Data una posizione geografica l'altezza massima del Sole varia in base alle stagioni dell'anno.


Il procedimento di calcolo sviluppato di seguito separa il calcolo degli angoli azimut/altezza:

1. l'azimut viene calcolato guardando la direzione normale rispetto al senso di marcia (di un segmento di tratto stradale)

2. l'altezza viene calcolata in base alla posizione geografica e rispetto alle stagioni dell'anno. In particolare si considera la altezza massima e minima del Sole 

## dati iniziali


+ area da studiare. Potrebbe essere data come città o per poligono di punti.
+ precisione nella discretizzazione dei percorsi stradali. Lunghezza in metri chiamata `dl`
+ tipo di strade accettate per l'installazione. Includere l'assenza di edifici attorno alle strade, entro un certo raggio `r0` espresso in metri.
+ lunghezza minima della striscia di pannelli. Lunghezza chiamata `l0` espressa in metri.
+ angolo massimo accettabile tra l'orientamento del pannello e la direzione di massima altezza del Sole. Angolo chiamato `t0` espresso il gradi.

## algoritmo di calcolo delle soluzioni ottimali

Calcolo dei punti ottimali rispetto all'azimut

0. estrarre l'area di interesse dal database geografico. Si chiami questa rete `map0`.

1. a partire dalla rete `map0`, filtrare per i tipi di strada richiesti e rimuovere i nodi che non soddisfano il criterio di lontananza dagli edifici. Si ottiene una nuova rete chiamata `map1`.

2. per ogni punto `p` in nella rete `map1` con precisione `dl`, si calcola la direzione perpendicolare al senso di marcia nel modo seguente. Ogni punto `p1` ha coordinate `(x1,y1)` e il su consecutivo `p2`, ha coordinate `(x2,y2)`. il segmento `p2-p1` permette di calcolare la direzione perpendicolare al senso di marcia. Si utilizzi per esempio il prodotto esterno con il vettore uscente della dalla carta geografica.

3. Ogni vettore perpendicolare al senso di marcia nel punto `p` della mappa `map1` può essere utilizzato per calcolare il seguente indice. Si stima la qualità dell'orientamento della direzione normale (al senso di marcia) guardando l'angolo di azimut compreso tra la direzione normale e la direzione in cui si ha la posizione più alta del Sole rispetto a `p`.

4.  considerato l'angolo massimo accettabile `t0` si individuano una serie di segmenti ottimali

5. si cerca tra i segmenti di percorso stradale ottimale, se ce ne sono di continui per una lunghezza maggiore di `l0`


Se l'insieme delle soluzioni al punto precedente non è vuoto, si procede con il calcolo dell'angolo altezza per i punti ottimali (todo)




## visualizzazione della soluzione

una mappa:

+ che raffigura l'area di studio inserita
+ in cui sono evidenziati i tratti di strada ottimali rispetto al movimento del Sole
+ ogni soluzione ottimale possiede un pop-up informativo che permette di verificare i parametri del calcolo


# soluzione di calcolo

Si sviluppa un figlio di calcolo interattivo in formato *jupyter* (python).

## dipendenze

**astropy** (https://pypi.org/project/astropy/), utilizzato per calcolare la posizione apparente del Sole dato un punto in coordinate geografiche

**openstreetmap**  (https://pypi.org/project/osmnx/), utilizzato come database geografico

**geopandas**  (https://geopandas.org/en/stable/), utilizzato per gestire l'informazione associata a porzioni del database geografico

**folium** (https://pypi.org/project/folium/), libreria per la visualizzazione d carte geografiche


## ulteriori funzionalità

generazione di un resoconto automatico


# riferimenti

https://towardsdatascience.com/retrieving-openstreetmap-data-in-python-1777a4be45bb

https://it.wikipedia.org/wiki/Sistema_di_coordinate_orizzontali