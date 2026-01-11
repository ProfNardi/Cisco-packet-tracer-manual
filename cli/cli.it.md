**Lingua:** [🇮🇹 Italiano](cli.it.md) | [🇬🇧 English](cli.md)

**Home:** [README.it.md](../README.it.md)

# Cisco IOS – Manuale di Comandi Essenziali CLI

Guida pratica per la configurazione di base di **switch** e **router Cisco** tramite interfaccia a riga di comando (CLI) in laboratorio.

---

## Modalità della CLI Cisco IOS

**Cisco IOS** utilizza diverse modalità gerarchiche di configurazione:

- **User EXEC Mode** (`>`): modalità limitata, solo comandi di visualizzazione base
- **Privileged EXEC Mode** (`#`): accesso completo ai comandi di visualizzazione e debug
- **Global Configuration Mode** (`(config)#`): modalità per la configurazione globale del dispositivo
- **Interface Configuration Mode** (`(config-if)#`): configurazione specifica delle interfacce

---

## Comandi Iniziali

### Accesso alle Modalità di Configurazione
```bash
enable                          # Passare dalla modalità utente (>) alla modalità privilegiata (#)
configure terminal              # Entrare in modalità di configurazione globale (config)#
```

### Configurazione di Base del Dispositivo
```bash
hostname Router1                # Modificare il nome del dispositivo (switch/router)
enable secret MyPassword123     # Impostare la password per l'accesso alla modalità privilegiata
```

### Configurazione Accesso Remoto (Telnet/SSH)
```bash
line vty 0 15                   # Configurare le linee virtuali per l'accesso remoto (da 0 a 15)
password MyTelnetPass           # Impostare la password per le connessioni remote
login                           # Richiedere l'autenticazione all'accesso
```

### Ottimizzazione della Console
```bash
no ip domain-lookup             # Disabilitare la risoluzione DNS (evita ritardi con comandi errati)
line console 0                  # Configurare la linea console
logging synchronous             # Sincronizzare i messaggi di log per non interrompere i comandi
exec-timeout 0 0                # Disabilitare il timeout (utile in laboratorio, sconsigliato in produzione)
```

---

## Configurazione delle Interfacce

### Router: Configurazione Interfaccia Fisica
```bash
configure terminal
interface GigabitEthernet 0/0   # Selezionare l'interfaccia da configurare
ip address 192.168.1.1 255.255.255.0    # Assegnare indirizzo IP e subnet mask
description LAN Uffici          # Aggiungere una descrizione dell'interfaccia
no shutdown                     # Attivare l'interfaccia (di default sono spente)
exit                            # Uscire dalla modalità di configurazione interfaccia
```

### Switch: Configurazione Interfaccia Virtuale (VLAN)
```bash
interface vlan 1                # Selezionare l'interfaccia virtuale della VLAN 1
ip address 192.168.1.10 255.255.255.0   # Assegnare IP per la gestione dello switch
no shutdown                     # Attivare l'interfaccia VLAN
```

---

## Comandi di Verifica e Diagnostica

```bash
show ip interface brief         # Visualizzare lo stato di tutte le interfacce (IP, status up/down)
show interfaces                 # Dettagli completi di tutte le interfacce
show version                    # Informazioni su hardware, IOS, uptime e configurazione
show cdp neighbors              # Elenco dei dispositivi Cisco direttamente collegati
show cdp neighbors detail       # Dettagli completi dei dispositivi vicini (IP, modello, porta)
show running-config             # Configurazione attualmente in esecuzione (RAM)
show startup-config             # Configurazione salvata in memoria permanente (NVRAM)
show vlan brief                 # Elenco delle VLAN configurate (solo per switch)
show ip route                   # Tabella di routing (solo per router)
show mac address-table          # Tabella MAC address dello switch
```

---

## Configurazione VLAN (Switch)

### Creare una Nuova VLAN
```bash
configure terminal
vlan 10                         # Creare VLAN con ID 10
name Amministrazione            # Assegnare un nome descrittivo alla VLAN
exit
```

### Assegnare una Porta alla VLAN (Access Mode)
```bash
interface FastEthernet 0/5      # Selezionare l'interfaccia da configurare
switchport mode access          # Configurare la porta in modalità access (per dispositivi finali)
switchport access vlan 10       # Assegnare la porta alla VLAN 10
description PC Amministrazione  # Aggiungere una descrizione
exit
```

### Configurare Porta Trunk (Collegamento tra Switch)
```bash
interface GigabitEthernet 0/1   # Selezionare l'interfaccia per il trunk
switchport mode trunk           # Configurare la porta in modalità trunk (trasporta tutte le VLAN)
switchport trunk allowed vlan all   # Permettere il passaggio di tutte le VLAN (opzionale)
description Trunk verso Switch2 # Aggiungere una descrizione
exit
```

> **Nota**: Le porte in modalità **access** sono utilizzate per collegare dispositivi finali (PC, stampanti), mentre le porte **trunk** sono usate per collegare switch tra loro, permettendo il passaggio di traffico di multiple VLAN.

---

## Salvataggio e Gestione della Configurazione

```bash
write memory                    # Salvare la configurazione dalla RAM alla NVRAM (permanente)
# Oppure
copy running-config startup-config   # Comando equivalente più esplicito
```

> **Importante**: Senza salvare la configurazione, tutte le modifiche andranno perse al riavvio del dispositivo!

### Altri Comandi Utili
```bash
erase startup-config            # Cancellare la configurazione salvata
reload                          # Riavviare il dispositivo
show startup-config             # Visualizzare la configurazione salvata
show running-config             # Visualizzare la configurazione attuale in esecuzione
```

### Uscire dalle Modalità di Configurazione
```bash
exit                            # Tornare al livello precedente
end                             # Tornare direttamente alla modalità privilegiata (#)
Ctrl+Z                          # Scorciatoia per tornare alla modalità privilegiata
```

---

Torna alla home: [README.it.md](../README.it.md)