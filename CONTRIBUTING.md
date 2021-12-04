# Partecipare alla traduzione

Lo scopo di questo documento è spiegare come partecipare alla stesura della documentazione italiana di PHP.

Se è presente un errore nella documentazione, verifica che lo stesso non sia presente nella documentazione inglese, in caso affermativo correggilo prima lì.
La traduzione italiana seguirà la modifica apportata alla documentazione inglese.

# Sommario

 1. Installazione
 2. Crea la documentazione
 3. Revision Tracking
 4. Coding Standard
 5. Traduzione, correzione di bozze e ortografia
 6. Workflow git


## 1: Installazione:

Per creare la documentazione, devi avere almeno i tre repository seguenti:
 - ``php/doc-base``: contiene gli strumenti per costruire la documentazione,
   disponibile su ``github.com``: https://github.com/php/doc-base
 - ``php/doc-en``: la versione inglese della documentazione su cui fare affidamento quando
   la versione italiana è inesistente per una pagina: https://github.com/php/doc-en
 - ``php/doc-it``: la versione italiana della documentazione: https://github.com/php/doc-it

 > Nota: per costruire la documentazione,
 > la cartella dove si trova quella inglese *deve* chiamarsi ``en``
 > e quella della documentazione italiana *deve* chiamarsi ``it``.


## 2: Costruire la documentazione

È importante sapere come creare la documentazione per garantire che le modifiche apportate non rompano la build, il che impedirà la pubblicazione dell'ultima versione su php.net.

Immaginando di trovarci nella cartella ``it`` nella seguente struttura di cartelle:

```
|
| - base
| - en
| - it
 |-...
```

Basta eseguire ``php ../base/configure.php --with-lang=it``.

Se tutto va bene l'output sarà il seguente:

```
All good. Saving .manual.xml... done.
All you have to do now is run 'phd -d /home/user/Dev/php-docs/base/.manual.xml'
If the script hangs here, you can abort with ^C.
         _ _..._ __
        \)`    (` /
         /      `\
        |  d  b   |
        =\  Y    =/--..-="````"-.
          '.=__.-'               `\
             o/                 /\ \
              |                 | \ \   / )
               \    .--""`\    <   \ '-' /
              //   |      ||    \   '---'
         jgs ((,,_/      ((,,___/

 (Run `nice php configure.php` next time!)
```

Altrimenti, avrai un errore XML Docbook che dovrà essere corretto.


## 3: Revision Tracking

Per garantire che la traduzione italiana sia aggiornata con la documentazione inglese, esiste un sistema di `rev-check`.

Ciò è manifestato dal seguente commento nella parte superiore di ogni file XML:

```xml
<!-- EN-Revision: git-hash Maintainer: XXXX Status: YYYYY -->
```

Quando si aggiorna un file per replicare le modifiche apportate alla versione inglese, è essenziale aggiornare il commit inglese indicato sopra come `git-hash`.

Lo stato del rev-check può essere attualmente visualizzato su
http://doc.php.net/revcheck.php?p=files&lang=it


## 4: Coding Standard

### File XML

Il passo da rispettare per l'indentazione è di 1.
Il carattere di indentazione è lo spazio ` ` (non sono consentite tabulazioni nei file `.xml`).
Esempio:

```xml
<note>
_<para>
__<example>
___<title>
___</title>
__</example>
_</para>
</note>
```

Inoltre, il soft-limit per il numero di caratteri per riga è 80.


### Esempio PHP

Ufficialmente il gruppo di documentazione PHP ha scelto di utilizzare la codifica standard PEAR, che si può trovare qui: http://pear.php.net/manual/en/standards.php

> In pratica, tuttavia, lo stile di codifica è un mix tra PEAR e PSR-2/12,
> cerca di seguire lo stile con cui è stata scritta la pagina, o quello della documentazione inglese.

Il codice sorgente PHP inizia dalla colonna zero, come nel seguente esempio:

```php
<?php
inizia_qui (); // bene
  inizia_qui (); // non bene
?>
```

Nota anche che si preferisce `echo` a `print` (`echo` senza parentesi).
Tutto il codice dovrebbe essere compatibile con `error_reporting (E_ALL)`.


## 5: Traduzione, correzione di bozze e ortografia

Per avere un manuale in un buon italiano, la traduzione di alcuni termini tecnici si trova nel documento ``TRADUZIONI.txt``.

È inoltre necessario rileggere la traduzione per garantire che il testo tradotto abbia senso e sia conforme al testo inglese.

Dopo aver corretto una traduzione, il seguente tag/commento
``<!-- Reviewed: no/yes -->`` deve avere il valore `yes`.
Quando si modifica un file revisionato, questo tag deve passare al valore ``no``, eccetto durante modifiche minori/modifiche puramente XML (ad es. modifica di un elemento `<methodsynopsis>`).


### Tradurre una nuova pagina

La traduzione di una nuova pagina inglese in italiano è relativamente semplice:
 
 * copia/incolla il file in questione;
 * aggiungi il commento di tracciamento della revisione con l'hash del commit della versione del file inglese che hai appena copiato, in modo da assicurarti che il file sia aggiornato dopo che la traduzione è stata completata.

Va notato che il file deve essere *completamente* (inclusi gli esempi) tradotto prima di essere aggiunto al repository git ufficiale.


## 6: Workflow git

Prova (se possibile) ad eseguire il commit directory per directory, o in ``reference/`` estensione per estensione.

Per i messaggi di log dei commit, bisogna aderire alle seguenti regole:
 - usare messaggi in inglese (nel caso in cui un non italiano debba comprendere le modifiche)
 - usare messaggi espliciti (non inserire "typo" quando si aggiunge del testo...)


### Utente medio

Per proporre una modifica devi passare attraverso una pull request sulla repository GitHub `doc-it`.

Segui i seguenti passi:

* crea un fork dal repository `doc-it` di GitHub;
* crea un nuovo ramo (feature branch);
* apporta le modifiche;
* esegui il commit;
* esegui il comando `git push` sul ramo presente sul tuo fork;
* crea una pull request.

Se vengono fatte osservazioni sulla tua pull request, seguile.
Dopo che la pull request è stata approvata, esegui uno squash-rebase della stessa in modo che le modifiche siano in un singolo commit. Questo semplifica il lavoro per la persona che ha bisogno di caricare il tuo contributo nel repository git ufficiale.

### Utente con accesso sulla repository GitHub

Non è necessario passare attraverso una pull request ed è possibile eseguire il commit e il push direttamente sul ramo ``master`` del repository doc-it su https://github.com.

Evita i "merge commits" e preferisci un ``git rebase`` seguito da un merge fast-forward.
