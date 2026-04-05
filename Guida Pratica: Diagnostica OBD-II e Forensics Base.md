# 🚗 Guida Pratica: Diagnostica OBD-II Wi-Fi e Forensics Base
**🛠️ Analisi dei codici d’errore (DTC) e dei Freeze Frames**  
 
---  

## 1️⃣ Introduzione Operativa 💻
Per eseguire queste operazioni senza app a pagamento, utilizzeremo un laptop nel mio caso un **Asus**, con **netcat (nc)** collegato al Wi-Fi dello scanner **ELM327** 📡 (IP standard: `192.168.0.10`, porta `35000`).    

### 🚀 Fase di Inizializzazione  
Occorre aprire il terminale ed eseguire la connessione al **ELM327**:  

```bash  
nc 192.168.0.10 35000
```

Una volta connesso, lo scanner può eseguire delle funzioni base in relazione alla capacità hardware dello scanner, ma in linea di principio tutti gli scanner possono eseguire le seguenti operazioni tramite il **CAN BUS** dell'auto:

- 🔄 `ATZ` – Reset dello scanner
- 📨 `ATH1` – Attiva gli Header per vedere chi invia il dato
- ⚙️ `ATSP0` – Auto-selezione protocollo

---

## 2️⃣ Lettura dei Codici d’Errore (DTC) ⚠️

I codici d’errore sono composti da una lettera e quattro numeri (es. `P0300`):

- **P (Powertrain):** 🏎️ Motore/Cambio
- **C (Chassis):** 🛞 Freni/Sospensioni
- **B (Body):** 🚘 Airbag/Serrature
- **U (Network):** 🌐 Comunicazione tra centraline

### 2.1 Lettura DTC Attivi (Mode 03) 🔴
Questi sono gli errori che potrebbero accedere la spia "Check Engine" 💡.

- **Comando:** `03`
- **Risposta tipica:** `43 01 03 00 00 00`
- **Decodifica:** Il primo byte `43` conferma la risposta alla mode 03 che io ho inviato tramite il comando 03. I byte `01 03` indicano il codice `P0103` i restanti sono byte di padding, tipicamente ma dipende dalle centraline, la lunghezza delle risposte varia tra i 6 e i 12 byte per le risposte di diagnostica base.

### 2.2 Scansione Codici Pending (Mode 07) 🟡
Errori rilevati durante l’ultimo ciclo di guida ma non ancora confermati. Sono fondamentali per la manutenzione predittiva 🔮.

- **Comando:** `07`
- **Utilità:** Se si trovi un errore qui, il guasto è imminente o intermittente ⏳.

### 2.3 Analisi Codici Permanent (Mode 0A) 🔒
Codici che non possono essere cancellati tramite software. La centralina li rimuove solo dopo aver verificato tramite i sensori che il pezzo è stato effettivamente sostituito o riparato 🔧.

- **Comando:** `0A` (o `10` in base al protocollo)

---

## 3️⃣ Analisi dei Freeze Frame (Mode 02) 📸❄️

Il Freeze Frame è una "scatola nera" ⬛ che salva i parametri del motore nel momento esatto in cui è apparso l’errore.

### 📋 Procedura
1. Digitare `0200` per vedere per quale errore è disponibile un frame.
2. Digitare `02` seguito dal PID che vuoi analizzare.
3. **Esempio:** `020C` richiede i giri motore (RPM) al momento del crash 💥.

### 🕵️ Scenario Forense
Bisogna anche avere un certo intuito nel leggere i PIDS che la centralina comunica. Se il codice è `P0300` (Mancata accensione) e il Freeze Frame mostra `0205` (Temperatura acqua 🌡️) a 115°C, capisci che l’errore non è nelle candele, ma nel surriscaldamento del motore 🔥.

---

## 4️⃣ Cancellazione Mirata (Mode 04) 🗑️

Questo comando resetta la memoria degli errori, spegne la spia e cancella i Freeze Frames ✨.

> 🛑 **ATTENZIONE:**  
> Eseguire questo comando **solo a motore spento e quadro acceso**. Non cancellare mai gli errori prima di una diagnosi completa: distruggeresti le prove necessarie alla riparazione ❌🕵️‍♂️.

- **Comando:** `04`
- **Risposta attesa:** `44` ✅ (Indica la cancellazione confermata)

---

## 5️⃣ Scripting in Python (Automazione) 🐍🤖

Ecco come si potrebbe automatizzare la lettura forense usando la libreria **odb**:

```python
import obd  
  
# Connessione WiFi 
connection=obd.OBD("192.168.0.10", port="35000")  
  
# Legge i codici attivi 
dtc_codes=connection.query(obd.commands.GET_DTC)  
for codice in dtc_codes.value:  
    print(f"Errore Trovato: {codice[0]} - {codice[1]}")  
  
# Analisi parametro in Freeze Frame (Giri Motore) con le modalità spiegate in precedenza 
ff_rpm=connection.query(obd.commands.FREEZE_RPM)  
print(f"RPM al momento dell’errore: {ff_rpm}")
```
***
