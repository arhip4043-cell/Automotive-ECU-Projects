# 🏎️ Automotive Cybersecurity: ECU Reverse Engineering & OT Threat Research

![Domain](https://img.shields.io/badge/Domain-Automotive_Security%20%26%20OT-darkred.svg)
![Protocols](https://img.shields.io/badge/Protocols-CAN_Bus%20%7C%20UDS%20%7C%20OBD--II-blue.svg)
![Status](https://img.shields.io/badge/Status-Active_Research-success.svg)

## 🎯 Abstract & OPSEC Notice
Questo repository pubblico funge da archivio per i **Whitepaper, i Research Paper e gli Executive Summary** derivanti dai miei studi nel campo dell'Automotive Cybersecurity e dei sistemi embedded (Operational Technology). 

> ⚠️ **OPSEC & Responsible Disclosure:** Per ragioni di sicurezza e rispetto delle policy di divulgazione responsabile, il codice sorgente offensivo (Proof of Concept), gli script di injection e i dump grezzi dei firmware delle centraline (ECU) sono mantenuti in un **repository privato** separato. 
In questa sede vengono pubblicati esclusivamente i report di analisi, le mitigazioni difensive e le metodologie di ricerca.

---

## 🔬 Il Modello di Minaccia (Automotive Threat Model)
I veicoli moderni non sono più semplici mezzi meccanici, ma vere e proprie reti su ruote (Rolling Networks). Con la convergenza tra IT e OT, le vulnerabilità delle reti automobilistiche rappresentano un rischio critico per la sicurezza fisica e la privacy. 

Le mie ricerche si concentrano sull'analisi dei seguenti vettori di attacco (Threat Vectors):
*   **In-Vehicle Network Spoofing:** Manipolazione e iniezione di pacchetti sul **CAN Bus** per forzare comandi fisici non autorizzati (es. manipolazione frenata, sterzo, quadro strumenti).
*   **Diagnostic Protocol Exploitation:** Abuso dei servizi **UDS (Unified Diagnostic Services)** e OBD-II per bypassare i controlli di sicurezza (Security Access) e alterare i parametri vitali del veicolo.
*   **Firmware Reverse Engineering:** Estrazione e analisi statica/dinamica del firmware delle **ECU (Electronic Control Units)** per l'individuazione di hardcoded credentials o logiche di evasione.
*   **Ransomware & Fleet Hijacking:** Studio teorico degli impatti di malware capaci di bloccare flotte di veicoli commerciali compromettendo la telematica di bordo.

---

## 📚 Pubblicazioni & Research Papers

Di seguito la lista degli studi, guide operative e script pubblicati derivanti dalla ricerca in laboratorio:

### 📄 [Tech Note 01] Automotive Digital Forensics: Diagnostica OBD-II via Wi-Fi e Automazione Python
* **Descrizione:** Guida pratica e analisi forense dell'estrazione dati dai veicoli. Il documento dimostra come bypassare i software commerciali interfacciandosi direttamente con uno scanner ELM327 tramite raw TCP/IP sockets (`netcat`). 
* **Focus Forense:** L'analisi si concentra sulla lettura dei *Diagnostic Trouble Codes (DTC)* e, in particolare, sull'estrazione dei **Freeze Frames (Mode 02)**. Il Freeze Frame viene trattato come un "Flight Data Recorder" (Scatola Nera) per acquisire la telemetria esatta (es. RPM, Temperatura, Velocità) al momento di un guasto o di un crash, fondamentale per la ricostruzione degli incidenti (Incident Response).
* **Automazione:** Include uno script Python (tramite libreria `obd`) per automatizzare il querying seriale e l'acquisizione massiva delle prove forensi dalla centralina.
* **Status:** `[Pubblicato]`
* **Link:** 👉 *[Leggi la Guida Completa qui](https://github.com/arhip4043-cell/Automotive-ECU-Projects/edit/main/Diagnostica%OBD-II%e%Forensics%Base.md#:~:text=Guida,-Pratica:%20Diagnostica%20OBD)*

### 📄 [Paper 02] CAN Bus Spoofing e Packet Injection
* **Descrizione:** Analisi del traffico CAN tramite sniffer (`can-utils`). Il paper in via di sviluppo dimostra la mancanza di autenticazione intrinseca nel protocollo CAN standard e propone metodologie di rilevamento di anomalie (Anomaly Detection) basate sull'analisi della frequenza dei pacchetti.
* **Status:** `[Drafting nel repository privato]`
* **Link:** *Coming Soon*

---

## 🛠️ Metodologia e Tech Stack Lab
Le ricerche documentate in questo repository sono condotte in un ambiente di laboratorio isolato (Testbench) per evitare rischi su veicoli in movimento.

*   **Hardware:** Adattatori OBD-II (es. ELM327, macchine Linux con socketCAN), Logic Analyzers, banchi prova ECU.
*   **Software/Tools:** Python, `can-utils` (candump, cansniffer), Wireshark (per analisi traffico CAN), Ghidra (per l'analisi statica dei binari estratti).
*   **Standard Analizzati:** ISO 11898 (CAN), ISO 14229 (UDS).

---
*Progetto indipendente orientato all'espansione delle competenze Blue Team dal settore IT (Endpoint/Network) al settore OT (Operational Technology & IoT Security).*
