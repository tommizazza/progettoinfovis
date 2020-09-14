# # Progetto INFOVIS 2019/2020: Tool di visualizzazione dell'output di un recommender system

## Introduzione


E' stato realizzato un tool per la visualizzazione dell'output di un recommender system.<br/>Il dominio di applicazione è quello di un negozio che utilizza un recommender system per dedurre i prodotti più consigliati/acquistati dai suoi utenti.<br/><br/>
Il dataset descrive 2 tipologie di nodi: **nodi utenti** (id) ,e **nodi prodotto** (id,categoriaProdotto).<br/>
Tra questi nodi sono presenti 2 diverse relazioni:<br/>**-Relazione utente->prodotto**: rappresenta quanto un utente X ha acquistato un prodotto Y.<br/>**-Relazione prodotto->utente**: rappresenta quanto un prodotto X è consigliato a un utente Y dal recommender system.<br/><br/>
Esempio di nodo utente: (ha acquistato uva,melanzane e del vino pecorino)
```
{
		"type": "userNode",
		"id": "user45",
		"category": "user",
		"neighbors":
		[

			{"id":"Uva","quantity":15},
			{"id":"Melanzane","quantity":5},
			{"id":"VinoPec","quantity":7}

		]
}
```
Esempio di nodo prodotto: (raccomandato agli utenti 871,744,155)
```
{
		"type": "productNode",
		"id": "Salmone",
		"category": "Pesce",
		"neighbors":
		[

			{"id":"user871","pleasure":0.4},
			{"id":"user744","pleasure":0.5},
			{"id":"user155","pleasure":0.7}

		]
}
```
Il dataset è composto da 1000 nodi utenti e circa 200 nodi prodotti.

## Progetto

Il tool consiste in uno Spring Embedder realizzato da zero, che consente di visualizzare il dataset sia come **grafo bipartito [utenti,prodotti]**, che come grafo comune.<br/>
Per questo motivo, è stato incluso un **pannello di gestione dei parametri** delle forze dello spring embedder.<br/> Inoltre è presente anche uno **star plot** che mostra la distrubuzione degli acquisti di un utente sulle categorie dei prodotti: (verdura,frutta,carne,pesce,bevande,latticini,alcolici).<br/></br>Le relazioni **Utente --> Prodotto** vengono visualizzate con **archi verdi**, mentre le relazioni **Prodotto --> Utenti** vengono visualizzate con **archi rossi**

![grafo1](/img1.jpg) ![grafo2](/img2.jpg)

Nella **prima immagine** si ha un solo centro di gravità posto al centro dello schermo, funziona come un normale Spring Embedder.<br/>
Nella **seconda immagine** invece si hanno 2 centri di gravità, uno per i nodi blu (1/6*width,height/2) e uno per i nodi rossi (5/6*width,height/2).<br/><br/>
Quando si è in modalità bipartito, si incrementa di un fattore K la componente repulsiva dell'asse Y dei nodi; l'alta repulsione sull'asse y mista all'attrazione ai centri di gravità comporta un allungamento dei 2 gruppi di nodi in verticale.<br/><br/>

Lo spring embedder normalmente non riuscirebbe a gestire tutti i 1200 nodi e 6000 archi circa su schermo contemporaneamente. Per ovviare al problema sono stati implementati dei filtri sui nodi, i nodi utenti sono stati divisi in bucket di 50 nodi ordinati per quantità totale di acquisti, mentre i nodi prodotti sono stati suddivisi per categoria di appartenenza. L'aggiornamento delle posizione dei nodi e dei relativi archi viene svolta solo sui nodi visibili selezionati dai filtri.<br/><br/>

Per implementare questi filtri, durante la lettura del dataset necessaria per creare la struttura dati *grafo* usata dallo Spring Embedder, vengono inizializzate 2 strutture dati: ***categoriaProdotti*** (array associativo [categoria] -> [lista ID prodotti categoria]) e quantitaNodi (lista ordinata dei nodi utenti sulla base degli acquisti). Con 2 select si selezionano la categoria di prodotti da visualizzare e un bucket di nodi utenti [i,i+50] corrispondente agli indici dei nodi utenti da voler visualizzare (in questo modo non si ripercorrono sempre le liste da capo)<br/><br/>

Inoltre per rendere la visualizzione più chiara al gestore, sono stati inseriti dei filtri sugli archi, si sfruttano 2 liste ordinate di archi (una per gli archi rossi e una per gli archi verdi, entrambe ordinate sulla base del peso degli archi) e 2 array associativi ( [peso Archi] -> [startIndex,endIndex] ) che dato un peso, ritorna lo startIndex e l'endIndex all'interno della lista ordinata, in 	questo modo, per cercare gli archi di un certo peso non si deve scorrere l'intera lista di archi.
