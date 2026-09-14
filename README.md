# 🛡️ FAIR Risk Assessment & Cyber Loss Engine (v6.0)

[![Framework](https://img.shields.io/badge/Framework-FAIR%20%2F%20ISO%2027005%3A2022-blue.svg)](https://www.openfair.org/)
[![Data Source](https://img.shields.io/badge/Threat%20Intel-IBM%202026%20%7C%20DBIR%202026%20%7C%20MSFT%202025-orange.svg)]()

Modello quantitativo per la valutazione del rischio cyber basato sul framework **FAIR (Factor Analysis of Information Risk)** e conforme allo standard **ISO/IEC 27005:2022**.

Il sistema permette di simulare l'**Annual Loss Expectancy (ALE)** espressa in **Euro** per diverse tipologie di incidenti, integrando la **Control Strength (CS) pesata**, la modellazione stocastica di Poisson per il **tempo di ritorno e l'affidabilità operativa**, e i dati empirici dei report di Threat Intelligence 2026 (**IBM Cost of a Data Breach 2026**, **IBM X-Force 2026**, **Verizon DBIR 2026**, **Microsoft DDR 2025**, **ENISA 2025**).

---

## 📑 Indice dei Contenuti

- [1. Caratteristiche Principali](#1-caratteristiche-principali)
- [2. Parametri di Input e Personalizzazione dello Scenario](#2-parametri-di-input-e-personalizzazione-dello-scenario)
- [3. Catalogo e Descrizione delle Mitigazioni](#3-catalogo-e-descrizione-delle-mitigazioni)
  - [A. Gruppo 1: Mitigazioni Perimetriche (Abbattimento Contatti ΔCF% e Azione ΔPoA%)](#a-gruppo-1-mitigazioni-perimetriche-abbattimento-contatti-cf-e-azione-poa)
  - [B. Gruppo 2: Mitigazioni Intrinseche di Sistema (Control Strength CS% Pesato)](#b-gruppo-2-mitigazioni-intrinseche-di-sistema-control-strength-cs-pesato)
- [4. Motore di Calcolo e Logica Quantitativa](#4-motore-di-calcolo-e-logica-quantitativa)
  - [A. Control Strength (CS) Pesata e Selettore ("X")](#a-control-strength-cs-pesata-e-selettore-x)
  - [B. Vulnerabilità Residua (V%) Moltiplicativa](#b-vulnerabilità-residua-v-moltiplicativa)
  - [C. Loss Event Frequency (LEF) e Modellazione Stocastica di Poisson](#c-loss-event-frequency-lef-e-modellazione-stocastica-di-poisson)
  - [D. Scomposizione della Loss Magnitude (LM) e dell'ALE (63,8% vs. 36,2%)](#d-scomposizione-della-loss-magnitude-lm-e-dellale-638-vs-362)
- [5. Fonti di Dati e Referenze Bibliografiche](#5-fonti-di-dati-e-referenze-bibliografiche)
- [6. Struttura dei File Excel della Suite](#6-struttura-dei-file-excel-della-suite)
- [7. Licenza Aperta e Feedback](#7-licenza-aperta-e-feedback)

---

## 1. Caratteristiche Principali

* **Sdoppiamento Mitigazioni Perimetro vs. Sistema:**
  * **Gruppo 1 (Perimetro):** Misura l'abbattimento della *Contact Frequency* ($\Delta	ext{CF}\%$) e della *Probability of Action* ($\Delta	ext{PoA}\%$) agendo sui tentativi d'ingresso.
  * **Gruppo 2 (Sistema):** Valuta la forza difensiva intrinseca (*Control Strength CS%*) degli asset interni.
* **Control Strength (CS) Pesata:** I controlli di sistema intrinseci sono pesati in base al loro reale impatto difensivo preventivo e di contenimento.
* **Selettore Dinamico dei Controlli ("X"):** Attivazione puntuale delle mitigazioni presenti nell'architettura tramite flag `"X"`.
* **Distribuzione Stocastica di Poisson):** Modellazione del **Tempo di Ritorno** e dell'**Affidabilità Operativa Annua** mediante la funzione di sopravvivenza stocastica
* **Separazione Netta dei Costi (63,8% Tecnici vs. 36,2% Data Breach):**
  * **Incidente Tecnico (Senza Fuga Dati - Primary Loss 63,8%):** Include *Detection & Escalation* (32,9%) e *Lost Business & Downtime* (30,9%). Spese legali e notifiche scendono a zero.
  * **Data Breach Completo (Con Fuga Dati - Secondary Loss 36,2%):** Aggiunge le spese legali/sanzioni (*Ex-Post Response* 27,3%) e notifiche Garante (*Notification* 9,0%), portando l'impatto economico al 100%.
* **Conversione e Formattazione Valuta in Euro (€):**

---

## 2. Parametri di Input e Personalizzazione dello Scenario

Il modello consente di configurare in modo dinamico i seguenti parametri per adattare il calcolo del rischio al contesto aziendale specifico:

1. **Categoria dell'Attaccante (Threat Capability - TC%):**
   * *Script Kiddie / Opportunista (25% - 40%):* Attacchi automatizzati a bassa complessità.
   * *Cybercriminale Standard (50% - 65%):* Minacce mirate con strumenti diffusi.
   * *Gruppo Ransomware / Attaccante Esperto (70% - 85%):* Capacità strutturate e tecniche avanzate (es. T1190, T1486).
   * *APT / Nation-State Actor (90% - 98%):* Minacce persistenti avanzate con zero-day.
2. **Dimensione Aziendale (Scala Baseline CF & PoA):**
   * *Piccola Impresa (PMI):* Volume contatti contenuto, superficie esposta ridotta.
   * *Media Impresa:* Esposizione moderata su servizi cloud e web.
   * *Grande Impresa / Enterprise:* Elevata superficie d'attacco e volume contatti continuo (es. 10.000+ contatti/anno).
3. **Macro-Area Geografica (Costi IBM 2026):**
   * **EMEA** (Europe, Middle East & Africa)
   * **NA** (North America)
   * **APAC** (Asia-Pacific)
   * **LAC** (Latin America & Caribbean)
   * **GLOBAL** (Media Mondiale)
4. **Classificazione degli Asset per Tier di Criticità:**
   * **Tier 0 (Life Critical / Safety Floor):** Sistemi la cui compromissione mette a rischio la vita umana o la sicurezza nazionale (es. Sanità, OT industriali, sistemi ESD/SIS, Dispositivi Medici UE Reg. 2017/745 MDR). *Tolleranza zero per la frequenza.*
   * **Tier 1 (Mission Critical):** Asset essenziali per l'operatività aziendale.
   * **Tier 2 (Operational):** Asset di supporto operativo .
   * **Tier 3 (Non-Critical):** Asset periferici a basso impatto.

---

## 3. Catalogo e Descrizione delle Mitigazioni

### A. Gruppo 1: Mitigazioni Perimetriche (Abbattimento Contatti ΔCF% e Azione ΔPoA%)

Le mitigazioni perimetriche riducono il numero di tentativi di contatto ($\Delta	ext{CF}\%$) e la probabilità che un contatto si trasformi in un attacco effettivo ($\Delta	ext{PoA}\%$):

* **VPN / Restrizione Intranet:** Limita la visibilità degli asset esposti direttamente su Internet, consentendo l'accesso unicamente previa autenticazione su canale cifrato intranet.
* **Network Microsegmentation:** Isola la rete aziendale in subnet e zone di sicurezza confinate, bloccando la propagazione laterale delle minacce.
* **WAF & Web/Edge Filtering:** Ispeziona e filtra il traffico HTTP/HTTPS e il traffico di rete verso l'esterno/l'interno, bloccando exploit applicativi web ed exfiltration.
* **Email Security Gateway:** Intercetta ed analizza la posta elettronica in ingresso, scartando spam, allegati malevoli e link di phishing prima del recapito.
* **User Security Awareness:** Programma continuo di formazione e simulazione per addestrare il personale a riconoscere tentativi di phishing, vishing e social engineering.

### B. Gruppo 2: Mitigazioni Intrinseche di Sistema (Control Strength CS% Pesato)

I controlli intrinseci di sistema definiscono la resilienza dell'asset qualora l'attacco superi il perimetro. Ad ogni controllo è assegnato un peso strategico relativo ($w_i$):

* **Patching & Hardening System (Peso 20,0%):** Remediation tempestiva delle vulnerabilità note (CVE) e configurazione di sicurezza avanzata (hardening) di sistema operativo ed applicazioni.
* **MFA Asset - Phishing-Resistant (Peso 20,0%):** Autenticazione a più fattori forte (es. FIDO2 / WebAuthn) che impedisce l'abuso di credenziali trafugate.
* **PAM & Vault Management (Peso 15,0%):** Controllo, segregazione e rotazione automatica delle credenziali ad elevato privilegio per gli accessi critici Tier-0.
* **EDR / XDR Endpoint Defense (Peso 15,0%):** Rilevamento in tempo reale, analisi comportamentale ed isolamento automatico dei processi malevoli su endpoint e server.
* **Secure SDLC & Code Audit (Peso 10,0%):** Integrazione di test di sicurezza automatici (SAST/DAST) e secure coding lungo la pipeline di sviluppo del software.
* **SIEM & Audit Logging (Peso 10,0%):** Centralizzazione, correlazione dei log ed alerting immediato per identificare comportamenti anomali post-intrusione.
* **Backup Immutabile & Recovery (Peso 10,0%):** Copie di sicurezza protette da scrittura/cancellazione (WORM) per garantire il ripristino rapido dei dati in caso di attacco Ransomware.

---

## 4. Motore di Calcolo e Logica Quantitativa

### A. Control Strength (CS) Pesata e Selettore ("X")

La forza difensiva complessiva dei controlli di sistema ($CS$) viene calcolata valutando esclusivamente le misure contrassegnate con il flag `"X"`, rapportando il contributo pesato dei controlli attivi alla somma totale dei pesi di sistema.

### B. Vulnerabilità Residua (V%) Moltiplicativa

La Vulnerabilità residua dell'asset ($V\%$) esprime la probabilità di successo dell'attaccante data la capacità dell'attaccante ($TC\%$) e la forza difensiva residua esposta.

Se tutti i controlli sono attivi al 100% la vulnerabilità scende; se non vi è alcun controllo, la vulnerabilità coincide con la capacità dell'attaccante.

### C. Loss Event Frequency (LEF) e Modellazione Stocastica di Poisson

1. **Loss Event Frequency (LEF) Residua:** Frequenza annua stimata degli incidenti con successo.
2. **Affidabilità Operativa Annua:** Rappresenta la probabilità stocastica di operare per 365 giorni senza subire alcun incidente con successo, modellata tramite la distribuzione di Poisson per eventi rari
3. **Tempo di Ritorno degli Incidenti:** Intervallo medio atteso in anni tra un breach e il successivo.

### D. Scomposizione della Loss Magnitude (LM) e dell'ALE (63,8% vs. 36,2%)

In conformità con i benchmark del report **IBM Cost of a Data Breach 2026**:

1. **Incidente Tecnico (Senza Fuga Dati / Business Interruption):**
   * Riguarda i soli **Costi Primari (63,8%)** (*Detection & Escalation 32,9%* + *Lost Business & Downtime 30,9%*).
   * I costi legali, le notifiche e le sanzioni scendono a zero.
2. **Data Breach Completo (Con Fuga Dati / Esfiltrazione PII):**
   * Aggiunge ai costi tecnici i **Costi Secondari da Data Breach (36,2%)** (*Ex-Post Response/Legali 27,3%* + *Notification 9,0%*).
   * Si applica l'impatto economico completo pari al **100,0%**.

---

## 5. Fonti di Dati e Referenze Bibliografiche

Il modello integra dati ed evidenze dai seguenti report e standard internazionali:

1. **IBM Cost of a Data Breach Report 2026:** Benchmarking dei costi medi globali ($4,99M USD), suddivisione per macro-aree geografiche e scomposizione nei 4 centri di costo (*Detection 32,9%*, *Lost Business 30,9%*, *Ex-Post Response 27,3%*, *Notification 9,0%*).
2. **IBM X-Force Threat Intelligence Index 2026:** Frequenza dei vettori d'accesso iniziale (*Exploitation of Public Applications 40%*, *Valid Accounts / Stolen Credentials 32%*, *Phishing 9%*), incidenza delle vulnerabilità non autenticate (56%) e frammentazione dell'ecosistema Ransomware (109 gruppi attivi).
3. **Verizon Data Breach Investigations Report (DBIR) 2026:** Modello VERIS, analisi su 31.000+ incidenti globali, rilevanza del fattore umano (62%) e pattern di intrusioni di sistema.
4. **Microsoft Digital Defense Report 2025:** Analisi su 100+ trilioni di segnali giornalieri, attacchi credenziali, MFA Phishing-Resistant, diffusione delle minacce social browser ClickFix.
5. **ENISA Threat Landscape 2025 Booklet:** Mappatura dei target critici della Pubblica Amministrazione e delle Entità Essenziali nell'Unione Europea.
6. **CERT-EU Threat Landscape Report 2025:** Analisi delle minacce geopolitiche e cyberespionaggio verso le Istituzioni dell'Unione Europea.
7. **ISO/IEC 27005:2022:** Standard internazionale per la gestione dei rischi di sicurezza delle informazioni (Sezione 6.4.2 per la definizione di Risk Appetite e soglie di tolleranza per asset Life Critical Tier 0).
8. **MITRE ATT&CK v19.2 (Enterprise, Mobile, ICS):** Tassonomia e identificatori delle tecniche di attacco (T1190, T1566, T1078, T1486, T1195).

---

💡 **Feedback & Contributi:**  
I suggerimenti, le segnalazioni di miglioramento e le proposte di integrazione sono graditi ed incoraggiati!

---


