# Assignment 4
Transfer Learning using VGG16 and CIFAR
ATTENZIONE: Questo README é una conversione automatica del file "report.pdf" che é formattato in modo migliore e contiene immagini di esempio! Leggi quello!

## Introduzione

Il progetto è stato realizzato su google ​ **colab** ​ in ​ **python** ​, servendosi delle librerie Keras,
sklearn, pandas, mathplotlib​ ​ (è già presente il comando pip per installarla) e ​ **Pandas** ​.

## Dataset scelto

Andava scelto un dataset di immagini contenente ​ **dalle 3 alle 10 classi** ​ e ho trovato tramite
google “​ **CIFAR-10** ​”: ​https://www.cs.toronto.edu/~kriz/cifar.html
Il dataset CIFAR-10 consiste in ​ **60.000** ​ immagini ​ **32x32** ​ a colori divise in ​ **10 classi** ​ con 6000
immagini per classe. Il test set è formato da 10.000 test immagini scelte tra le 60.000.
Le classi sono perfettamente bilanciate.
Ho scelto questo dataset poichè rispetta le caratteristiche richieste e contiene immagini
32x32 che sono quindi abbastanza “piccole” per garantire un tempo di computazione non
esagerato.


Nuovo task - Selezione delle classi
Le classi originarie del dataset CIFAR10 sono le seguenti
Inizialmente l’assignment è stato svolto cercando di considerare tutte le 10 classi, ovvero
tutte le 50.000 immagini presenti nel dataset. Così facendo però la fase di traning della SVM
(approfondita dopo) era davvero​ **troppo complessa per le risorse a disposizione.
Ho quindi selezionato 4 delle 10 classi** ​ di questo dataset, creando un nuovo dataset
contenente solo le immagini relative a:
● Aeroplani (0)
● Automobili (1)
● Navi (8)
● Camion (9)
Da notare che la scelta è stata volutamente fatta all’interno della categoria “​ **mezzi di
trasporto** ​”. Infatti se avessi messo 2 classi di animali e 2 classi di mezzi di trasporto
probabilmente il modello sarebbe stato “facilitato” nel distinguere queste categorie.
**Il nuovo dataset quindi è costituito da 20.000 immagini di training** ​ (meno della metà di
quello di partenza) ​ **e 4.000 immagini di test** ​:


VGG16 - Importazione e layer di taglio
La VGG-16 è stata importata tramite le funzioni già predisposte da keras escludendo i layer
dense finali dell’immagine qui sopra.
Ho quindi ricavato 4 modelli, così composti:
● Model1: dall’input layer fino a ​ **block5_pool** ​ (prima dei dense) con ​ **512** ​ parametri
● Model2: dall’input layer fino a ​ **block4_pool** ​ con ​ **2048** ​ parametri
● Model3: dall’input layer fino a ​ **block3_pool** ​ con ​ **4096** ​ parametri
● Model4: dall’input layer fino a ​ **block2_pool** ​ con ​ **8192** ​ parametri
_All’interno del source code sono presenti ulteriori dettagli riguardanti i layer (visualizzati
tramite la funzione summary)_
Feature extraction sui dati di training e di test
E’ stata poi eseguita la funzione ​ **predict** ​ sui dati di ​ **test** ​ e di ​ **train** ​ per estrarre, da ogni
immagine,​ **le feature** ​ che i modelli precedenti hanno ricavato dalle stesse.


Successivamente​ **le matrici tridimensionali contenenti le feature estratte** ​ ​ **sono state
“appiattite”** ​ottenendo così le seguenti shape, corrispondenti al modello1, modello2,
modello3, modello4 sopradescritti.
**Original Shape
After flattering**
E’ possibile notare come ​ **le feature aumentino avvicinandosi al layer di input** ​ e diventino
quindi più complesse da elaborare nei passaggi successivi (durante la SVM)
Traning della SVM e prediction sui dati di test
Per questo progetto è stato richiesto di utilizzare un ​ **classificatore lineare** ​, ho quindi optato
per una ​ **SVM** ​.
Quindi, il passaggio successivo, è stato quello di ​ **addestrare la SVM con le feature
estratte dalla VGG-16** ​tagliata nei 4 punti​**.** ​ Il processo è stato ripetuto sui 4 modelli visti
prima per poter valutare quale taglio della VGG-16 fornisse feature “migliori” per poter
effettuare la classificazione.
Dopo il ​ **fit** ​ della SVM sui dati di ​ **training** ​ è stato realizzata direttamente la ​ **predizione** ​ sui dati
di ​ **test** ​ con una SVM, stampando a video le performance dello stesso.
La SVM è stata lasciata con i parametri di default, specificando l’utilizzo di un kernel lineare.
_Riporto i risultati del test nella pagina successiva:_


## Performance sui dati di test

- Performance del ​ modello 1: accuracy 0,
- Performance del ​ modello 2: accuracy 0,
- Performance del ​ modello 3: accuracy 0,
- Performance del​ modello 4 ​: ​ accuracy 0,


Riportando in un grafico l’andamento dell’accuracy ottengo questo risultato
Si nota facilmente come il ​ **terzo** ​ taglio della VGG-16 produca le ​ **performance migliori** ​con
la SVM applicata successivamente.
Come mi aspettavo ​ **le performance migliori non si ottengono ne con tagli vicini
all’input** ​ (dove le feature sono tante e poco specifiche) ​ **ne vicino all’output** ​ (dove le feature
sono più specifiche)
_Viene ora riportata una breve analisi visiva sui risultati ottenuti_


Analisi visiva sui dati di Test
“PRED: X” dove X si riferisce a cosa il modello ha predetto per l’immagine corrispettiva tra le
seguenti categorie:
● **0** ​ - Aeroplani
● **1** ​- Automobili
● **8** ​ - Navi
● **9** ​ - Camion


