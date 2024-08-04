---
title: "Come generare un seed bitcoin con i dadi"
description: "Guida per creare un seed bitcoin usando dadi, carta e penna."
keywords: ["seed", "seed bitcoin", "seed con dadi", "dadi bitcoin", "seed bitcoin ita"]
author: "Turtlecute"
date: 2024-05-12
url: /seed
images: ["img/seed.png"]
---
# SEED

Calcolare una frase mnemonica di 24 parole usando dadi, carte e penna.

[Tedesco](https://aprycot.media/blog/diy-private-schluessel-fuer-bitcoin-erstellen/) 🇩🇪  
[Inglese](https://armantheparman.com/dicev2/) 🇺🇲

## Introduzione

Questo progetto ti aiuterà a capire meglio il funzionamento delle chiavi riguardanti Bitcoin e a generarle in autonomia. Avrai bisogno di matita, carta, qualcosa per generare un output casuale in binario (come una moneta o un dado) e un computer. Invece di studiare gli aspetti teorici riguardanti il funzionamento dei seed, nella guida di oggi metteremo le mani 'in pasta' e opereremo con un approccio pratico.

## IMPORTANTE PREAMBOLO

Prima di usare direttamente le chiavi generate, è meglio fare un po' di pratica seguendo i passi di questa guida.  
Quando crei la tua vera chiave, quella che userai per custodire i tuoi bitcoin, devi generarla su un computer air-gapped. Il dispositivo non deve avere la capacità di connettersi a Internet. Non è sufficiente disattivare temporaneamente la connessione Wi-Fi se si è seriamente interessati alla sicurezza; esistono tecniche sofisticate che consentono di estrarre le tue chiavi private anche se sei temporaneamente disconnesso da Internet.

Ok, iniziamo...

## PASSO 1 – CREA NUMERI BINARI CON ENTROPIA

É possibile generare entropia lanciando una moneta 256 volte, ma è piu randomico e veloce usare i dadi. Puoi acquistare dadi da casinò per garantire tiri equi e casuali, ma va benissimo usare qualsiasi vecchio dado. Anche se i tuoi dadi non sono perfetti e hanno qualche difetto, fintanto che ne usi diversi alla volta, otterrai una casualità sufficiente.

Questa è la procedura (una delle tante possibili):

*   Prendi circa quattro dadi (ad esempio da un vecchio gioco da tavolo).
*   Considera i numeri 1, 2 o 3 come zero mentre 4, 5 o 6 saranno un uno. In questo modo otterrai un output binario (solo zeri e uno nel risultato finale) con i dadi (ad es. tira un 3, registra uno zero; tira un 6, registra un uno).
*   Crea 23 righe di 11 cifre. La 24a riga avrà bisogno solo di tre cifre. Per ogni riga, raggruppa le cifre in gruppi di 4-4-3 (vedi immagine sotto) per facilitare la lettura e il calcolo. Mantieni le colonne verticali allineate il più possibile e lascia spazio tra ogni riga per i calcoli manuali. Tutto avrà più senso tra poco.

Esempio:

![](img/image8.webp)
![](img/image10.webp)

> Ci sono 256 cifre binarie: 23 serie complete di 11 cifre con la 24a riga che ne richiede solo tre.

  

Per ora, tieni presente che le 11 cifre binarie verranno tradotte in una parola della mnemonic phrase che rappresenterà il tuo seed. Siccome stiamo utilizzando un totale di 256 cifre casuali come entropia, e stiamo dividendo le cifre in gruppi di 11, se dividiamo 256 per 11 ci aspettiamo di ottenere il numero di parole della menmonic phrase. Ma il risultato di 256 diviso 11 non è un numero intero bensì 23 con un resto di 3.  

Abbiamo bisogno di altre 8 cifre, altri 8 bit, per creare la nostra 24a parola. Una volta che abbiamo 264 bit in totale, tutto si divide in 24 gruppi di 11 cifre, da cui ricavare una mnemonic phrase di 24 parole. Come vedremo in seguito, questi ultimi 8 bit extra avranno un importante ruolo da svolgere.

**Una nota sulla casualità:**

Possiamo creare questi 256 bit di dati casuali come preferiamo, purché siano effettivamente entropici. Se non sono randomici, qualcuno potrebbe essere in grado di riprodurre i dati. Sarebbe quindi in grado di ricreare la tua chiave privata e prendere tutti i tuoi bitcoin. Esempio: se crei 256 bit di tutti zeri (chiaramente non casuali), allora qualcuno sarà in grado di indovinare la tua chiave privata. Ecco la prova: ho generato una chiave privata da quella terribile casualità di tutti zeri e ho trovato il portafoglio esistente di qualcuno. Se non fosse già stato svuotato, avrei potuto rubare i fondi.

![](img/image9.webp)

> Sapevano chiaramente cosa stavano facendo perché si trattava di una piccola somma che non è stata lasciata lì per molto tempo. Potrebbe essere stata una dimostrazione, chissà. Ma altre persone hanno creato chiavi private non casuali facilmente indovinabili e di conseguenza hanno perso i loro bitcoin. Ma non preoccuparti, se crei una chiave privata veramente casuale, qualcuno dovrebbe ripetere esattamente i tuoi lanci di dadi binari o lanci di monete e, grazie alla matematica esponenziale, ciò non accadrà prima della fine dell'universo.

  

## PASSO 2 – CALCOLARE IL CHECKSUM

Questi ultimi 8 bit mancanti devono essere calcolati per formare quello che viene chiamato "checksum".

[Cos'è un checksum](https://en.wikipedia.org/wiki/Checksum)? Un checksum è il modo in cui i computer sanno che hai commesso un errore di battitura quando inserisci dati come il numero della tua carta di credito o del tuo conto bancario o il tuo codice fiscale. Può essere davvero utile che il computer ti avverta se hai commesso un errore di battitura nella tua chiave privata!

Per calcolare il checksum avrai bisogno di un computer Linux o Mac [(la procedura può essere fatta a mano](https://armantheparman.com/sha256/)[,](https://armantheparman.com/sha256/) [ma è molto complicata – ecco una guida)](https://armantheparman.com/sha256/). Se hai Windows 10, puoi installare l'app Ubuntu (una versione del terminale di Linux) dal Microsoft Store. Basta cercare “Ubuntu” e installarlo. Utilizzerai il terminale di Ubuntu per eseguire i comandi che seguono. L'app è una sessione temporanea; nessun file viene memorizzato all'interno della stessa. Ciò significa che avrai una sessione pulita ogni volta che lo esegui.

Agli utenti Windows consiglio di adottare questa soluzione alternativa. Ne ho provato altre ma ho sempre riscontrato dei problemi. Un'altra soluzione potrebbe essere usare il tool di Massimo Musumeci che peró altererebbe l'ordine con cui stiamo facendo le cose in questa guida: [Tool di Massimo](https://github.com/massmux/bip39checksum)

Ora che hai un terminale sul tuo computer Mac, Linux o Windows 10, digita il comando seguente. Sostituisci le mie cifre binarie con le tue cifre binarie casuali (nota che dovrebbe essere tutta una riga molto lunga, anche se il modo in cui viene visualizzato qui potrebbe sembrare diverso)

> echo 10101111001110000000111101100011 1101011110100101001000101100111101111 0100011000010100011111100100010100011 1100011101010001100111111100001010001 1000101011101000101001111111010100101 0011110110110110000001101111010011000 0011101011010010000100010000100001001 11 | shasum -a 256 -0

Spiegazione del codice: il comando echo mostrerà il risultato dell'operazione nel terminale, il comando shasum -a comunica al pc di eseguire la funzione di hash sha256 sui dati da noi inseriti mentre -0 spiega al computer che i dati che stiamo trattando sono binari.

Eseguendo questo comando, l'hash risultante viene visualizzato nella riga seguente, che inizierà con "b184":

![](img/image1.webp)

Ora possiamo iniziare a calcolare il checksum. Prendiamo le prime due cifre dell'output dell'hash, in questo caso “b” e “1”. Questi sono numeri esadecimali. In esadecimale, invece di utilizzare le cifre da 0 a 9, contiamo fino a 15 utilizzando le lettere dell'alfabeto per rappresentare i numeri maggiori di nove:

> 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, a, b, c, d, e, f

Simile a come le carte da poker contano da 1 a 13 usando le figure:

> Asso, 2, 3, 4, 5, 6, 7, 8, 9, 10, Fante, Regina, Re

Nel mio hash, la prima cifra, "b", corrisponde a 11; 1 equivale a 1.

Ora convertiamo questi due numeri nelle loro rappresentazioni binarie a quattro cifre. Puoi farlo facendo riferimento alla tabella qui sotto:

![](img/image2.webp)

> '11' in binario è '1011', '1' è '0001'. Siamo passati da b e 1, a 11 e 1, e infine a 1011 e 0001.

Questi numeri a quattro cifre sono il checksum che aggiungeremo ai nostri bit per completare la nostra 24a parola!

Aggiungili alla 24a riga per completare la serie finale di 11 cifre binarie. Ora hai 264 cifre in totale (vedi come viene completata la 24a riga nel diagramma più in basso)

## PASSO 3 – CONVERSIONE DA BINARIO A DECIMALE

Ciascuno dei gruppi di 11 cifre binarie deve essere convertito in un numero decimale.

Puoi inserirli in un calcolatore binario-decimale online, ma sarebbe estremamente rischioso per la sicurezza. Ti mostrerò come farlo manualmente.

In un sistema numerico binario, ci sono solo 0 e 1. Le altre cifre che conosci (2,3,4,5,6,7,8,9) non esistono. Quindi bisognerà trasformare questi numeri in zeri e uno.

Con undici cifre binarie, il numero più piccolo possibile è zero (00000000000), il più grande è 2047 (11111111111).

Prendiamo ciascun gruppo di undici cifre binarie sulla nostra pagina (ogni riga) e convertiamolo in un numero decimale decimale. Puoi farlo manualmente o convertirli dalla riga di comando in Linux, Mac o nell'app Ubuntu.

Per il numero 10101111001, devi digitare:

> echo "$((2#10101111001))"

Otterrai come output "1401". A questo punto calcola il decimale di tutti i tuoi 24 gruppi di 11 cifre binarie.

Fare questa conversione esclusivamente a mano è più difficile, ma possibile.

Nella parte superiore della pagina, scrivi questa sequenza esatta di numeri da destra a sinistra, verticalmente in linea con le cifre binarie sottostanti: "1024" sopra la prima colonna di cifre binarie. Quindi "512" sulla colonna successiva. Poi "256". E così via, dimezzando il numero ogni volta finché non si finisce con "1" sopra l'ultima (undicesima) colonna delle cifre binarie.

![](img/image12.webp)

Ora, osserva la tua prima riga di cifre binarie. Ovunque ci sia un “1”, aggiungi il numero decimale che si trova direttamente sopra di esso e registralo sotto la cifra binaria. Dove c'è uno “0” ignora il numero sopra. In questo modo:

![](img/image7.webp)

> In questo esempio, c'è un "1" sotto la colonna 1024, la colonna 256, il 64, il 32, il 16, l'8 e l'1.

Aggiungi i numeri decimali per ottenere il totale:

![](img/image11.webp)

Ora ripeti questo processo per tutte le 24 righe:

![](img/image5.webp) ![](img/image6.webp)

Ora avrai 24 numeri decimali che vanno da 0 a 2047.

## PASSO 4 – CERCA IL BIP 39

Il protocollo BIP 39 (Bitcoin Improvement Proposal numero 39) specifica 2048 parole diverse, elencate in ordine alfabetico. Quando questo elenco viene letto, ogni parola può essere identificata dalla sua posizione numerica nell'elenco. I numeri appena calcolati vengono utilizzati per cercare la parola corrispondente. Ad esempio, la prima riga ha prodotto il numero 1401 che equivale alla parola "quality" nell'elenco di parole BIP 39 ordinato.

Zero è il valore più piccolo possibile che puoi calcolare per una riga (dal binario 00000000000). In tal caso, selezionerai "abandon", la prima parola nell'elenco.

Il numero più grande possibile è 2047 (ottenuto dal binario 11111111111). La parola corrispondente è "zoo", l'ultima parola della lista.

C'è un problema da tenere presente: i computer contano gli elementi a partire da 0. Quindi il quinto elemento in un elenco è il numero 4 del computer.

[La specifica ufficiale delle parole BIP 39 è su GitHub](https://github.com/bitcoin/bips/blob/master/bip-0039/english.txt) ma l'elenco delle parole viene visualizzato con numeri di riga che iniziano con uno anziché zero. Quindi, mentre "abstract" è l'ottava parola ed è elencata nella riga numero 8, il suo effettivo equivalente numerico BIP 39 è 7.

![](img/image4.webp)

La mia prima riga di 11 cifre binarie dà come risultato 1401 in decimale. Quindi nell'elenco GitHub, devo trovare la parola sulla riga 1402 (1401 + 1). Otterrò la parola "quality". Procedi a cercare ogni decimale, avendo cura di aggiungere un 1 in più al risultato calcolato in modo che corrisponda alla numerazione delle righe di GitHub e trova la parola per ciascuna delle 24 righe.

![](img/image3.webp) ![](img/image13.webp)

Ben fatto! Se sei arrivato fin qui! Ora hai una mnemonic phrase Bitcoin valida di 24 parole. Ora puoi buttarlo via. A meno che tu non abbia utilizzato l'approccio completamente manuale, non puoi usarlo per i tuoi Bitcoin perché non è stato creato in un ambiente sicuro!

In realtà, prima di buttarlo, potresti provare ad inserire le parole in un portafoglio hardware o in un portafoglio software e vedere se vengono rifiutate. Se vengono rifiutate, hai commesso un errore da qualche parte, il che è molto facile da fare con l'approccio manuale. Se c'è qualche errore, il checksum non corrisponderà e tutti i portafogli segnaleranno immediatamente un errore.

## PER LE TUE CHIAVI REALI

Genera assolutamente le tue chiavi usando un procedimento il più possibile manuale e, nelle parti in cui ti appoggi ad un dispositivo elettronico, sfrutta sistemi sicuri, sistemi operativi vergini e valuta di smontare la scheda wireless dal tuo pc. Un'opzione potrebbe essere installare TailsOS su un disco esterno, smontare la scheda wireless dal tuo pc, avviare l'OS e mantenerlo disconnesso da internet fino alla fine della procedura. Una volta calcolato il seed, spegnere il pc e ri-avviare il computer con la scheda wireless ri-montata. E possibile anche usare dei raspberry come pc air-gapped.

<style> .btcpay-form { display: inline-flex; align-items: center; justify-content: center; } .btcpay-form--inline { flex-direction: row; } .btcpay-form--block { flex-direction: column; } .btcpay-form--inline .submit { margin-left: 15px; } .btcpay-form--block select { margin-bottom: 10px; } .btcpay-form .btcpay-custom-container{ text-align: center; }.btcpay-custom { display: flex; align-items: center; justify-content: center; } .btcpay-form .plus-minus { cursor:pointer; font-size:25px; line-height: 25px; background: #DFE0E1; height: 30px; width: 45px; border:none; border-radius: 60px; margin: auto 5px; display: inline-flex; justify-content: center; } .btcpay-form select { -moz-appearance: none; -webkit-appearance: none; appearance: none; color: currentColor; background: transparent; border:1px solid transparent; display: block; padding: 1px; margin-left: auto; margin-right: auto; font-size: 11px; cursor: pointer; } .btcpay-form select:hover { border-color: #ccc; } .btcpay-form option { color: #000; background: rgba(0,0,0,.1); } .btcpay-input-price { -moz-appearance: textfield; border: none; box-shadow: none; text-align: center; font-size: 25px; margin: auto; border-radius: 5px; line-height: 35px; background: #fff; }.btcpay-input-price::-webkit-outer-spin-button, .btcpay-input-price::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; } </style>
<form method="POST" action="https://btcpay.priorato.org/api/v1/invoices" class="btcpay-form btcpay-form--block">
  <input type="hidden" name="storeId" value="2B1STLH5REvhHZBRQuyJNieRTexpeuJ4Usjn4ziEfEfd" />
  <input type="hidden" name="checkoutDesc" value="Grazie per la donazione tartaruga!" />
  <input type="hidden" name="browserRedirect" value="https://turtlecute.org" />
  <input type="hidden" name="currency" value="EUR" />
  <input type="hidden" name="defaultPaymentMethod" value="BTC_LNURLPAY" />
  <input type="image" class="submit" name="submit" src="https://btcpay.priorato.org/img/paybutton/pay.svg" style="width:209px" alt="Pay with BTCPay Server, a Self-Hosted Bitcoin Payment Processor">
</form>