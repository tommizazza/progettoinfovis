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

![grafo1](/img1.jpg)
