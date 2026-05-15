Sistemi e Reti – Classe 4

# 🦈 WireShark

Introduzione, installazione su Windows e primo esercizio pratico per analizzare il traffico di rete.

[① Perché usare WireShark](#perche)
[② Come funziona](#come-funziona)
[③ Installazione (Windows)](#installazione)
[④ Configurazione iniziale](#configurazione)
[⑤ Esercizio base](#esercizio)

🔍

## 1 · Perché usare WireShark

**WireShark** è un *analizzatore di protocolli di rete* (o *packet sniffer*) open‑source,
considerato lo strumento di riferimento nel settore della sicurezza e dell'amministrazione di reti.
Consente di catturare tutto il traffico che transita su un'interfaccia di rete e di esaminarlo in dettaglio,
pacchetto per pacchetto.

### Principali utilizzi

* **Diagnosi di problemi di rete** – individuare ritardi, pacchetti persi o errori di configurazione.
* **Analisi della sicurezza** – rilevare traffico sospetto, scansioni di porte o dati in chiaro non cifrati.
* **Studio dei protocolli** – osservare dal vivo come funzionano TCP/IP, DNS, HTTP, ARP, ecc.
* **Sviluppo e debug** – verificare che un'applicazione invii e riceva i dati nel modo corretto.
* **Didattica** – comprendere concretamente il modello ISO/OSI e la suite TCP/IP.

**📚 Perché è importante per voi**
Nel corso di Sistemi e Reti studiate i protocolli in modo teorico; WireShark vi permette di
*vederli funzionare in tempo reale*, rendendo molto più semplice capire cosa accade ad ogni livello dello stack.

Stack ISO/OSI

7 · Applicazione

6 · Presentazione

5 · Sessione

4 · Trasporto (TCP/UDP)

3 · Rete (IP)

2 · Collegamento (Ethernet)

1 · Fisico

WireShark
Cattura pacchetti
dai livelli 1 – 7
Decodifica i protocolli
e mostra i campi dei
singoli header
🦈 Packet Sniffer

traffico catturato

Interfaccia di rete (NIC)

Fig. 1 – WireShark intercetta il traffico a tutti i livelli dello stack ISO/OSI

**⚠️ Attenzione legale**
Catturare traffico di rete **senza autorizzazione** è illegale in Italia (art. 617 c.p. – intercettazione illecita).
Usate WireShark **solo** su reti e dispositivi di vostra proprietà o per cui avete esplicito consenso.

⚙️

## 2 · Come funziona WireShark

WireShark mette la scheda di rete in **modalità promiscua** (o usa il driver *Npcap* su Windows),
che consente di ricevere *tutti* i frame che transitano sul segmento, non solo quelli destinati al proprio MAC.
I dati vengono poi decodificati e visualizzati in modo strutturato.

Scheda di
rete (NIC)
Modalità promiscua

Driver
Npcap
Copia pacchetti raw

Dissector
WireShark
Decodifica protocolli

Interfaccia grafica
Lista pacchetti
Dettaglio header
Byte raw (hex dump)

Fig. 2 – Pipeline di cattura: dalla NIC all'interfaccia grafica

### Le tre aree dell'interfaccia

L'interfaccia di WireShark è divisa in tre pannelli principali:

* **Packet List** (in alto) – elenco cronologico dei pacchetti catturati con info sintetiche.
* **Packet Details** (centro) – albero espandibile con tutti i campi di ogni livello protocollare.
* **Packet Bytes** (basso) – dump esadecimale dei byte grezzi del pacchetto selezionato.

💾

## 3 · Installazione su Windows

WireShark richiede il driver **Npcap** per catturare i pacchetti su Windows.
L'installer ufficiale lo include automaticamente.

**🔗 Link ufficiale**
Scaricate l'installer dalla pagina ufficiale: `https://www.wireshark.org/download.html`
Scegliete *Windows x64 Installer* (la versione più recente stabile).

### Procedura passo-passo

1. **Scaricare l'installer**
   Andate su `wireshark.org/download.html` e cliccate su
   *Windows x64 Installer*. Il file scaricato si chiamerà qualcosa come
   `Wireshark-4.x.x-x64.exe`.
2. **Avviare l'installer come Amministratore**
   Fate click destro sul file scaricato → *Esegui come amministratore*.
   I privilegi elevati sono necessari per installare il driver Npcap.
3. **Accettare la licenza**
   Leggete (o almeno scorrete) la licenza GNU GPL e cliccate *Noted* → *Next*.
4. **Scegliere i componenti**
   Lasciate la selezione predefinita (WireShark + TShark + Plugins).
   Accertatevi che la voce *Npcap* sia spuntata.
5. **Installare Npcap**
   Quando appare la finestra di Npcap, lasciate le opzioni di default e cliccate *Install*.
   Al termine, chiudete la finestra di Npcap: l'installer principale riprenderà automaticamente.
6. **Completare l'installazione**
   Cliccate *Next* fino a *Finish*. Al termine potrebbe essere richiesto un
   **riavvio del sistema**: riavviate prima di procedere.

**⚠️ Nota per i laboratori scolastici**
Se i PC del laboratorio non permettono installazioni, chiedete al docente/tecnico di installare
WireShark in anticipo, oppure usate la versione **Portable** (disponibile su portableapps.com)
che non richiede privilegi di amministratore (ma alcune funzioni di cattura live possono essere limitate).

① Download
wireshark.org

② Esegui
come Admin

③ Npcap
installa driver

④ Finish
completamento

⑤ Riavvio
→ WireShark pronto

Fig. 3 – Fasi dell'installazione di WireShark su Windows

🛠️

## 4 · Configurazione iniziale

Al primo avvio, WireShark mostra la **schermata iniziale** con la lista delle interfacce di rete
disponibili e un grafico del traffico in tempo reale per ciascuna. Prima di iniziare a catturare è utile
fare alcune impostazioni di base.

### 4.1 · Scegliere l'interfaccia giusta

Identificate quale interfaccia è attiva: di solito è quella con il grafico di traffico animato.
Le più comuni sono:

* `Ethernet` – se siete connessi via cavo.
* `Wi-Fi` – se siete connessi in wireless.
* `Loopback (lo)` – interfaccia virtuale interna al PC, utile per test locali.

**💡 Suggerimento**
Per l'esercizio di questa lezione useremo l'interfaccia di **loopback**
(`Adapter for loopback traffic capture`) che non richiede connessione di rete esterna.

### 4.2 · Impostare la risoluzione dei nomi

Andate su **Edit → Preferences → Name Resolution** e valutate se abilitare o disabilitare
la risoluzione DNS. *Disabilitarla* rende la cattura più leggera e veloce; abilitarla
mostra i nomi host al posto degli indirizzi IP (utile ma introduce traffico DNS aggiuntivo).

### 4.3 · Impostare un filtro di cattura (opzionale)

Un **capture filter** limita già in fase di cattura i pacchetti registrati.
Si scrive nella barra in alto prima di avviare la cattura. Usa la sintassi di *libpcap*.

```
# Esempi di capture filter
icmp                 # solo pacchetti ICMP (ping)
tcp port 80          # solo traffico HTTP
host 192.168.1.1     # solo traffico da/verso quell'IP
```

### 4.4 · Display filter – filtrare dopo la cattura

I **display filter** si applicano sulla lista di pacchetti già catturati senza perdere dati.
Si scrivono nella barra verde in cima alla finestra principale. Usano una sintassi propria di WireShark.

```
icmp                 # mostra solo pacchetti ICMP
ip.addr == 8.8.8.8   # mostra pacchetti con quell'IP src o dst
tcp.flags.syn == 1   # mostra pacchetti SYN (apertura connessione)
```

**✅ Differenza chiave**
*Capture filter* → scarta i pacchetti prima che vengano salvati (meno memoria, no undo).
*Display filter* → nasconde/mostra pacchetti già catturati (reversibile, senza perdita dati).

### 4.5 · Colori dei pacchetti

WireShark colora automaticamente le righe per tipo di protocollo. Alcuni colori predefiniti:

* 🟩 **Verde chiaro** – traffico TCP generico
* 🟦 **Blu chiaro** – traffico UDP / DNS
* 🟥 **Rosso/rosa** – errori TCP, RST
* 🟨 **Giallo** – ARP, routing
* ⬛ **Grigio scuro** – ICMP

I colori si possono personalizzare da **View → Coloring Rules**.

🧪

## 5 · Esercizio base – Cattura un ping in loopback

In questo esercizio userete WireShark per catturare i pacchetti **ICMP** (Echo Request/Reply)
generati da un semplice comando `ping` verso l'indirizzo di loopback `127.0.0.1`.
Tutto avviene localmente, senza bisogno di connessione Internet.

Il vostro PC
cmd → ping 127.0.0.1

Loopback
127.0.0.1

ICMP Request

ICMP Reply

🦈 WireShark
cattura i pacchetti

Fig. 4 – Il ping in loopback torna allo stesso PC; WireShark osserva il traffico sull'interfaccia loopback

### Istruzioni passo-passo

1. **Aprire WireShark**
   Cercate WireShark nel menu Start e avviatelo. Vedrete la schermata con la lista delle interfacce.
2. **Selezionare l'interfaccia Loopback**
   Nella lista delle interfacce cercate *"Adapter for loopback traffic capture"*
   (o semplicemente `Loopback`). Fate doppio click per avviare la cattura.
3. **Aprire il Prompt dei Comandi**
   Premete `Win + R`, digitate `cmd` e premete Invio.
4. **Eseguire il ping**
   Nel Prompt dei Comandi digitate:

   ```
   ping 127.0.0.1 -n 4
   ```

   Il parametro `-n 4` invia esattamente 4 pacchetti ICMP Echo Request.
5. **Fermare la cattura in WireShark**
   Tornate su WireShark e premete il pulsante rosso **■ Stop**
   (oppure usate la scorciatoia `Ctrl+E`).
6. **Applicare il display filter ICMP**
   Nella barra dei filtri in cima alla finestra digitate `icmp` e premete Invio.
   Vedrete solo i pacchetti ICMP (8 righe: 4 Request + 4 Reply).
7. **Esaminare un pacchetto**
   Fate click su un pacchetto *Echo (ping) request*. Nel pannello centrale espandete:
   * `Internet Protocol Version 4` → src/dst = `127.0.0.1`
   * `Internet Control Message Protocol` → Type: 8 (Request) o 0 (Reply)

### Verifica – Lista di controllo

Spuntate ogni punto dopo averlo completato:

* WireShark è installato e si apre correttamente
* L'interfaccia Loopback è visibile nella lista
* La cattura è stata avviata sull'interfaccia Loopback
* Il comando `ping 127.0.0.1 -n 4` è stato eseguito nel cmd
* La cattura è stata fermata dopo il ping
* Il filtro `icmp` mostra 8 pacchetti (4 Request + 4 Reply)
* Ho esaminato un pacchetto ed ho trovato i campi IP e ICMP nel pannello centrale

**🎉 Ottimo lavoro!**
Hai completato tutti i passaggi. WireShark è installato, configurato e funzionante.
Sei pronto per le analisi di rete più avanzate nei prossimi esercizi!

**📝 Domande di riflessione**

1. A quale livello ISO/OSI opera il protocollo ICMP?
2. Qual è la differenza tra *Echo Request* (tipo 8) e *Echo Reply* (tipo 0)?
3. Perché src e dst dell'indirizzo IP sono entrambi `127.0.0.1`?
4. Quanti byte occupa un pacchetto ICMP ping (payload di default)?

Tutorial WireShark · Sistemi e Reti – Classe 4  |  Realizzato con HTML + SVG
