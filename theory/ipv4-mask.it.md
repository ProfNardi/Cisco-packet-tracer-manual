**Lingua:** [🇮🇹 Italiano](ipv4-mask.it.md) | [🇬🇧 English](ipv4-mask.md)

**Home:** [README.it.md](../README.it.md)

# Classi indirizzi IPv4

Un indirizzo IPv4 è composto da **32 bit** rappresentati in quattro ottetti:

```txt
11000000 10101000 00000001 00001011   in binario
192.168.1.11                          notazione decimale puntata (dotted-decimal)
```

| Ottetto | Binario | Decimale |
|---------|---------|----------|
| 1°      | 11000000 | 192      |
| 2°      | 10101000 | 168      |
| 3°      | 00000001 | 1        |
| 4°      | 00001011 | 11       |

---

# Maschera di rete

La maschera di rete (net mask) è un indirizzo IPv4 di 32 bit che identifica la porzione di rete e la porzione host di un indirizzo IP:

```txt
Supponiamo di voler fissare 24 bit per la parte di rete

R = Rete                H = Host
RRRRRRRR RRRRRRRR RRRRRRRR HHHHHHHH
11111111 11111111 11111111 00000000   in binario
255.255.255.0                         notazione decimale puntata (dotted-decimal)
/24                                   notazione CIDR (Classless Inter-Domain Routing)
```

## Analisi degli indirizzi 

La maschera di rete serve a:
*  **estrarre la rete di appartenenza** di un indirizzo IP tramite l'operazione (bitwise) di **Messa in AND**.
*  **estrarre l'indirizzo di broadcast**

Supponiamo ad esempio di avere una stampante collegata ad una rete con indirizzo IP 192.168.1.11 e una maschera di 255.255.255.0

```txt
11000000 10101000 00000001 00001011   (IP della stampante)
11111111 11111111 11111111 00000000   (Maschera)
───────────────────────────────────   AND (Messa in AND)
11000000 10101000 00000001 00000000   (Indirizzo di rete risultante: 192.168.1.0)
```

**Mettendo ad 1 tutta la parte di host dell'indirizzo di rete si ottiene l'indirizzo di broadcast**. 

```txt
11000000 10101000 00000001 11111111   (Indirizzo di broadcast risultante: 192.168.1.255)

```
**Risultato**: La rete di appartenenza è **192.168.1.0**

L'indirizzo di broadcast è **192.168.1.255**

## Indirizzi utilizzabili (Range Host Addresses)

Poiché l'indirizzo di rete e l'indirizzo di broadcast sono riservati e non possono essere assegnati ai dispositivi, quindi:

**Indirizzi utilizzabili = 2^(bit host) - 2**

Nel nostro esempio con /24 (8 bit per host):
- Totale indirizzi: 2^8 = 256
- Meno rete (192.168.1.**0**): -1
- Meno broadcast (192.168.1.**255**): -1
- **Indirizzi assegnabili: 254**

Gli indirizzi da 192.168.1.1 a 192.168.1.254 sono disponibili per host e dispositivi.


# Subnetting

Il subnetting **consiste nel suddividere una rete IP in più reti contigue della stessa dimensione**.

---

## Principio

Si sottraggono bit **dalla parte di host** senza toccare quelli di **rete**.

Esempio, supponiamo di avere 192.168.1.0/24, e di voler sottrarre 3 bit dalla parte di host.

```txt
R = Rete    S = Subnet     H = Host

RRRRRRRR RRRRRRRR RRRRRRRR SSSHHHHH
```
Analizzando il numero di combinazioni per bit, abbiamo:

**8 Sottoreti (2³) da 30 Host ciascuna (2⁵-2)**

Ciascuna sottorete avrà un proprio indirizzo di rete e di broadcast:

```txt
11000000 10101000 00000001 000 00000   <- (Indirizzo 1° sottorete: 192.168.1.0)
11000000 10101000 00000001 001 00000   <- (Indirizzo 2° sottorete: 192.168.1.32)
11000000 10101000 00000001 010 00000   <- (Indirizzo 3° sottorete: 192.168.1.64)
11000000 10101000 00000001 011 00000   <- (Indirizzo 4° sottorete: 192.168.1.96)
11000000 10101000 00000001 100 00000   <- (Indirizzo 5° sottorete: 192.168.1.128)
11000000 10101000 00000001 101 00000   <- (Indirizzo 6° sottorete: 192.168.1.160)
11000000 10101000 00000001 110 00000   <- (Indirizzo 7° sottorete: 192.168.1.192)
11000000 10101000 00000001 111 00000   <- (Indirizzo 8° sottorete: 192.168.1.224)

11000000 10101000 00000001 000 11111   <- (Indirizzo di broadcast 1° sottorete: 192.168.1.31)
11000000 10101000 00000001 001 11111   <- (Indirizzo di broadcast 2° sottorete: 192.168.1.63)
11000000 10101000 00000001 010 11111   <- (Indirizzo di broadcast 3° sottorete: 192.168.1.95)
11000000 10101000 00000001 011 11111   <- (Indirizzo di broadcast 4° sottorete: 192.168.1.127)
11000000 10101000 00000001 100 11111   <- (Indirizzo di broadcast 5° sottorete: 192.168.1.159)
11000000 10101000 00000001 101 11111   <- (Indirizzo di broadcast 6° sottorete: 192.168.1.191)
11000000 10101000 00000001 110 11111   <- (Indirizzo di broadcast 7° sottorete: 192.168.1.223)
11000000 10101000 00000001 111 11111   <- (Indirizzo di broadcast 8° sottorete: 192.168.1.255)
```

# Supernetting (Aggregazione)

Il supernetting **consiste nell’aggregare più reti IP contigue della stessa dimensione in un'unica rete più ampia**.

> In pratica è l’operazione inversa del subnetting.

---

## Principio

Si sottraggono bit **dalla parte rete**, lasciando invariata la parte di host.

Esempio: aggregazione di **4 reti /24 contigue**, a partire da **192.168.1.0/24**.


```txt
R = Rete  X = Supernetting H = Host

RRRRRRRR RRRRRRRR RRRRRRXX HHHHHHHH
```
Otterremo:

* Reti aggregate: 4 → 2²
* Bit sottratti alla parte rete: 2
* Nuovo prefisso: /22
* Bit host: 32 − 22 = 10
* Host utilizzabili: 2¹⁰ − 2 = 1022

```txt
11000000 10101000 000000 00 00000000   <- Rete 1: 192.168.0.0/24
11000000 10101000 000000 01 00000000   <- Rete 2: 192.168.1.0/24
11000000 10101000 000000 10 00000000   <- Rete 3: 192.168.2.0/24
11000000 10101000 000000 11 00000000   <- Rete 4: 192.168.3.0/24

11000000 10101000 000000 00 11111111   <- Broadcast rete 1: 192.168.0.255
11000000 10101000 000000 01 11111111   <- Broadcast rete 2: 192.168.1.255
11000000 10101000 000000 10 11111111   <- Broadcast rete 3: 192.168.2.255
11000000 10101000 000000 11 11111111   <- Broadcast rete 4: 192.168.3.255
```
**Supernet risultante:**

* Rete: 192.168.0.0/22
* Broadcast: 192.168.3.255
* Range host: 192.168.0.1 – 192.168.3.254
* Host assegnabili: 1022

---

Torna alla home: [README.it.md](../README.it.md)