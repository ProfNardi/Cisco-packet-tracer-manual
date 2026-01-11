**Lingua:** [🇮🇹 Italiano](scenario1.it.md) | [🇬🇧 English](scenario1.md)

**Home:** [README.it.md](../README.it.md)

## Scenario 1 – Singola rete LAN

### Topologia

- 1 Switch di livello 2 (**esempio: modello 2960**)
- 3 host (PC) collegati allo switch tramite cavi **Copper Straight-Through**

### Indirizzamento IP

| Dispositivo | Indirizzo IP  | Subnet Mask   |
|-------------|---------------|---------------|
| PC1         | 192.168.1.10  | 255.255.255.0 |
| PC2         | 192.168.1.11  | 255.255.255.0 |
| PC3         | 192.168.1.12  | 255.255.255.0 |


![Scenario 1](../images/scenario1.png)

---
### Configurazione (esempio su PC1)

> **Nota**: lo switch non richiede alcuna configurazione IP per il funzionamento di base in una rete LAN semplice.

1. **Modificare il nome del dispositivo**: personalizzare il nome del PC per identificarlo facilmente nella topologia.
   
   Percorso: `Config` → `Global` → `Settings`

![Nome PC](../images/sc1-pc1-1.png)

2. **Configurare l'indirizzo IP e la subnet mask**: assegnare l'indirizzo IP statico alla scheda di rete.
   
   Percorso: `Config` → `Interface` → `FastEthernet0`

![Indirizzo IP](../images/sc1-pc1-2.png)

3. **Ripetere la configurazione** per gli altri due host PC2 e PC3, utilizzando gli indirizzi IP della tabella sopra.

### Verifica

- Utilizzare il comando **ping** dalla riga di comando di un PC verso gli altri (esempio: `ping 192.168.1.11`)
- La comunicazione deve avvenire con successo senza perdita di pacchetti (0% packet loss)
- Verificare che tutti e tre i PC possano comunicare tra loro

---

### Riferimenti teorici

- Maschera di rete, CIDR e calcolo rete/broadcast: [theory/ipv4-mask.it.md](theory/ipv4-mask.it.md)
- Intervalli IPv4 privati (RFC 1918): [theory/reti-private.it.md](theory/reti-private.it.md)
- Guida comandi CLI Cisco IOS: [cli/cli.it.md](cli/cli.it.md)

---

Torna alla home: [README.it.md](../README.it.md)
