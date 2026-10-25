# Tessere della mappa pluviometrica

Le immagini degli strati della mappa di
[precipitazioni.avventuremicologiche.it](https://precipitazioni.avventuremicologiche.it).

Stanno qui e non nel repo del sito per una ragione sola, misurata il 23/9/2026:
**le tessere sono dato derivato e si rigenerano intere.** Nel repo del sito ogni
rigenerazione lasciava dentro `.git`, per sempre, un'altra copia di tutte e
31.482. In tre generazioni erano diventate **77.127 oggetti, 233 MB**, cioè
quasi metà della storia del repo, mentre i dati veri di due anni ne occupano 49.

## ⚠️ LA REGOLA DI QUESTO REPO: un commit solo

Quando le tessere si rigenerano **non si aggiunge un commit: si sovrascrive
quello che c'è**, con un push forzato. Così questo repo non cresce mai e
ridipingere tutto torna a costare zero.

    git add -A
    git commit --amend -m "tessere <data>"
    git push --force

Se un giorno servisse tornare indietro, le tessere **si rifanno**: sono
derivate, non originali. Gli originali sono le carte forestali delle regioni e
il modello del terreno, dichiarati qui sotto.

## Perché cambiano

Non perché cambino i dati: le carte forestali sono ferme. Cambiano **quando
cambiamo la legenda noi**. Aggiungere un colore vuol dire ridipingere tutte le
tessere: è successo il 19/9 (nono colore) e il 21/9 (decimo).

## Come le prende il sito

Da jsDelivr, che serve i repo pubblici di GitHub senza configurazione:

    https://cdn.jsdelivr.net/gh/AvventureMicologiche/Mappa-Precipitazioni-Tessere@<commit>/boschi/{z}/{x}/{y}.png

⚠️ **Nell'indirizzo va il COMMIT, mai `@main`.** Con `@main` jsDelivr tiene la
copia al bordo dodici ore: il 23/9 il sito ha servito per mezza giornata
l'indice vecchio, e il rilievo compariva solo zoomando. Un commit invece non
cambia mai e jsDelivr lo serve `immutable`. Quindi, a ogni rigenerazione:

1. qui: `git commit --amend` e `git push --force` (vedi sopra);
2. `git rev-parse HEAD`;
3. nell'`index.html` del sito: il codice nuovo in `TESSERE_COMMIT`.

Niente purge da fare: le copie vecchie restano legate al commit vecchio e
nessuno le chiede più.

## Cosa c'è dentro

- `boschi/` — la carta dei boschi, livelli 5-13, 31.482 tessere, tavolozza a
  dieci colori. `boschi/indice.json` dice quali tessere esistono (dove non c'è
  bosco la tessera non c'è) e da quale carta viene ogni regione.

## Fonti e licenze

Le fonti regionali, con la loro licenza e il collegamento, sono elencate una per
una dentro `boschi/indice.json`, e la mappa le dichiara nei crediti secondo le
regioni che stai guardando. Sono carte forestali regionali e provinciali, quasi
tutte CC BY 4.0, più la «Carta della Natura» di ISPRA per sette regioni.
