**Lingua:** [🇮🇹 Italiano](reti-private.it.md) | [🇬🇧 English](reti-private.md)

**Home:** [README.it.md](../README.it.md)

# Reti IPv4 private

Le reti private utilizzano intervalli di indirizzi IP riservati secondo lo standard RFC 1918. Questi indirizzi non sono instradabili su Internet e vengono tradotti in indirizzi pubblici tramite NAT, permettendo a più dispositivi interni di accedere a Internet mantenendo sicurezza e semplicità gestionale.

## Intervalli RFC 1918

```txt
Classe A: 10.0.0.0 - 10.255.255.255 (10/8)
Classe B: 172.16.0.0 - 172.31.255.255 (172.16/12)
Classe C: 192.168.0.0 - 192.168.255.255 (192.168/16)
```

### Funzionamento

- **Non instradabili**: Riservati per reti interne, non routabili pubblicamente
- **NAT (Network Address Translation)**: Traduce gli indirizzi privati interni in un singolo IP pubblico per la comunicazione esterna
- **Sicurezza**: Crea una barriera protettiva nascondendo la topologia interna

### Indirizzi privati aggiuntivi

- **APIPA (169.254.0.0/16)**: Auto-assegnazione automatica quando DHCP non è disponibile (RFC 3330)
 
---

Torna alla home: [README.it.md](../README.it.md)
