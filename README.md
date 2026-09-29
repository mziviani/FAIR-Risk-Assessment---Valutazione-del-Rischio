# 🛡️ FAIR Risk Assessment & Cyber Loss Engine (v6.0)

[![Framework](https://img.shields.io/badge/Framework-FAIR%20%2F%20ISO%2027005%3A2022-blue.svg)](https://www.openfair.org/)
[![Data Source](https://img.shields.io/badge/Threat%20Intel-ENISA%202026%20%7C%20ISACA%202026%20%7C%20IBM%202026%20%7C%20DBIR%202026-orange.svg)]()
[![Currency](https://img.shields.io/badge/Currency-EUR%20%281%20USD%20%3D%200.92%20EUR%29-green.svg)]()
[![License](https://img.shields.io/badge/License-Open%20Source-brightgreen.svg)]()

Modello quantitativo per la valutazione del rischio cyber basato sul framework **FAIR (Factor Analysis of Information Risk)** e conforme allo standard **ISO/IEC 27005:2022**.

Il sistema permette di simulare l'**Annual Loss Expectancy (ALE)** espressa in **Euro** per diverse tipologie di incidenti, integrando la **Control Strength (CS) pesata**, la modellazione stocastica di Poisson per il **tempo di ritorno e l'affidabilità operativa**, e i dati empirici dei report di Threat Intelligence 2026 (**IBM Cost of a Data Breach 2026**, **IBM X-Force 2026**, **Verizon DBIR 2026**, **Microsoft DDR 2025**, **ENISA 2025**).

---

## 📑 Indice dei Contenuti

- [1. Caratteristiche Principali](#1-caratteristiche-principali)
- [2. Parametri di Input e Personalizzazione dello Scenario](#2-parametri-di-input-e-personalizzazione-dello-scenario)
- [3. Catalogo dei 12 Vettori d'Attacco](#3-catalogo-dei-12-vettori-dattacco).
- [4. Catalogo e Descrizione delle Mitigazioni](#4-catalogo-e-descrizione-delle-mitigazioni)
  - [A. Gruppo 1: Mitigazioni Perimetriche (Abbattimento Contatti ΔCF% e Azione ΔPoA%)](#a-gruppo-1-mitigazioni-perimetriche-abbattimento-contatti-cf-e-azione-poa)
  - [B. Gruppo 2: Mitigazioni Intrinseche di Sistema (Control Strength CS% Pesato)](#b-gruppo-2-mitigazioni-intrinseche-di-sistema-control-strength-cs-pesato)
- [5. Motore di Calcolo e Logica Quantitativa](#5-motore-di-calcolo-e-logica-quantitativa)
  - [A. Control Strength (CS) Pesata e Selettore ("X")](#a-control-strength-cs-pesata-e-selettore-x)
  - [B. Vulnerabilità Residua (V%) Moltiplicativa](#b-vulnerabilità-residua-v-moltiplicativa)
  - [C. Loss Event Frequency (LEF) e Modellazione Stocastica di Poisson](#c-loss-event-frequency-lef-e-modellazione-stocastica-di-poisson)
  - [D. Scomposizione della Loss Magnitude (LM) e dell'ALE (63,8% vs. 36,2%)](#d-scomposizione-della-loss-magnitude-lm-e-dellale-638-vs-362)
- [6. Fonti di Dati e Referenze Bibliografiche](#6-fonti-di-dati-e-referenze-bibliografiche)


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

## 3. Catalogo dei 12 Vettori d'Attacco

Di seguito è riportato il catalogo dei 12 vettori di minaccia modellati nel motore FAIR, con i relativi identificatori fisici, descrizioni sintetiche ed evidenze di Threat Intelligence 2026:

| ID / MITRE | Vettore d'Attacco | Descrizione Operativa | Evidenze & Benchmark 2026 |
| :--- | :--- | :--- | :--- |
| **T1190** | **1. Exploitation of Public-Facing Applications** | Sfruttamento di vulnerabilità note (N-day) o zero-day in web app, API o servizi perimetrici pubblicati su Internet per ottenere un accesso iniziale non autorizzato. | **ENISA 2026:** Vettore #1 d'accesso in UE (**60,4%**).<br>**IBM X-Force 2026:** 40% degli accessi iniziali (+44% YoY).<br>**ISACA 2026:** 39% delle compromissioni. |
| **T1566 / T1598** | **2. Phishing & Social Engineering (Email)** | Invio di messaggi email ingannevoli contenenti link malevoli, allegati weaponizzati o richieste di credenziali e frodi Business Email Compromise (BEC). | **ISACA 2026:** **45%** dei casi di compromissione.<br>**ENISA 2026:** 77,8% delle tecniche di social engineering.<br>**DBIR 2026:** 16% tasso mediano di click. |
| **T1078 / T1110** | **3. Credential Abuse / Stolen Credentials** | Uso non autorizzato di credenziali valide trafugate (via infostealer, data breach terzi o attacchi brute force) per accedere ai sistemi d'intranet e cloud. | **IBM X-Force 2026:** 32% degli abusi via account validi.<br>**ISACA 2026:** 24% degli accessi remoti.<br>**MSFT DDR 2025:** 85% degli username presenti in breach. |
| **T1486** | **4. System Intrusion & Ransomware** | Infiltrazione complessa e movimento laterale finalizzati alla cifratura dei sistemi, esfiltrazione di dati riservati e doppia/tripla estorsione finanziaria. | **ENISA 2026:** **47,3%** delle minacce finanziarie in UE.<br>**IBM X-Force 2026:** 109 gruppi ransomware attivi.<br>**DBIR 2026:** Presente nel 61% dei breach gravi. |
| **T1195 / npm** | **5. Supply Chain & Open-Source Compromise** | Compromissione di fornitori terzi, librerie software open-source (repository npm, PyPI, GitHub) o aggiornamenti software per penetrare nell'organizzazione. | **ISACA 2026:** 11% dei casi via supply chain/API.<br>**CrowdStrike 2026:** 87% dei pacchetti malevoli concentrati in npm.<br>**ENISA 2026:** Elevato impatto sistemico su terze parti. |
| **Human Factor** | **6. Coinvolgimento del Fattore Umano** | Errori operativi, disattenzioni, violazioni di policy, misconfigurazioni accidentali o vulnerabilità alla manipolazione da parte del personale interno. | **Verizon DBIR 2026:** **62%** dei breach coinvolge il fattore umano.<br>**ENISA 2026:** Errore e social eng al 20,7%.<br>**ISACA 2026:** 45% social engineering. |
| **VERIS App** | **7. Basic Web Application Attacks** | Attacchi diretti alle applicazioni web aziendali (es. SQL Injection, Cross-Site Scripting XSS, Broken Access Control) per sottrarre dati o manipolare il servizio. | **IBM X-Force 2026:** **56%** delle vulnerabilità tracciate non richiede autenticazione.<br>**ISACA 2026:** 39% vuln exploitation. |
| **VERIS Err** | **8. Miscellaneous Errors (Misconfiguration)** | Errate configurazioni di sicurezza (es. bucket cloud esposti, permessi IAM eccessivi, regole di firewall errate) che aprono la strada agli attaccanti. | **ENISA 2026:** **20,7%** degli accessi non autorizzati da esposizione accidentale.<br>**ISACA 2026:** 39% unpatched/misconfigured systems. |
| **T1078.003** | **9. Privilege Misuse & Insider Threat** | Abuso intenzionale o malevolo dei privilegi d'accesso assegnati a dipendenti, consulenti o utenti interni per sottrarre dati o danneggiare asset. | **ISACA 2026:** **21%** delle aziende rileva insider threat.<br>**ENISA 2026:** 10,1% degli incidenti idenficati.<br>**DBIR 2026:** 60% motivato da comodo personale. |
| **T1204.001** | **10. ClickFix & Social Browser Attacks** | Tecniche avanzate di social engineering basate su falsi messaggi d'errore del browser che inducono gli utenti a eseguire comandi malevoli (es. PowerShell). | **ENISA 2026:** Inserito tra i vettori emergenti a più rapida crescita.<br>**MSFT DDR 2025:** Diffusione massiva di Fake Browser Updates/ClickFix. |
| **T1566.004** | **11. Vishing / Voice Social Engineering & SSO** | Attacchi telefonici con ingegneria sociale o deepfake vocali rivolti all'helpdesk o utenti per bypassare l'MFA e compromettere account Single Sign-On (SSO). | **CrowdStrike 2026:** Attacchi Vishing **+134% YoY** (compromissione SSO in <5 min).<br>**ENISA 2026:** 1,9% del social engineering UE. |
| **T1528 / OAuth** | **12. Device Code & OAuth Consent Phishing** | Abuso dei flussi di autorizzazione OAuth 2.0 e del Device Code Flow (es. su Microsoft Entra ID) per induzione al consenso malevolo senza rubare la password. | **CrowdStrike 2026:** Tentativi di Device Code Phishing **aumentati di 15 volte** nella prima metà del 2026 su ambienti Entra ID. |

---

## 4. Catalogo e Descrizione delle Mitigazioni

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

## 5. Motore di Calcolo e Logica Quantitativa

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

## 6. Fonti di Dati e Referenze Bibliografiche

Il modello integra dati ed evidenze dai seguenti report e standard internazionali:

1. **IBM Cost of a Data Breach Report 2026:** Benchmarking dei costi medi globali ($4,99M USD), suddivisione per macro-aree geografiche e scomposizione nei 4 centri di costo (*Detection 32,9%*, *Lost Business 30,9%*, *Ex-Post Response 27,3%*, *Notification 9,0%*).
2. **IBM X-Force Threat Intelligence Index 2026:** Frequenza dei vettori d'accesso iniziale (*Exploitation of Public Applications 40%*, *Valid Accounts / Stolen Credentials 32%*, *Phishing 9%*), incidenza delle vulnerabilità non autenticate (56%) e frammentazione dell'ecosistema Ransomware (109 gruppi attivi).
3. **Verizon Data Breach Investigations Report (DBIR) 2026:** Modello VERIS, analisi su 31.000+ incidenti globali, rilevanza del fattore umano (62%) e pattern di intrusioni di sistema.
4. **Microsoft Digital Defense Report 2025:** Analisi su 100+ trilioni di segnali giornalieri, attacchi credenziali, MFA Phishing-Resistant, diffusione delle minacce social browser ClickFix.
5. **ENISA Threat Landscape 2025 Booklet:** Mappatura dei target critici della Pubblica Amministrazione e delle Entità Essenziali nell'Unione Europea.
6. **CERT-EU Threat Landscape Report 2025:** Analisi delle minacce geopolitiche e cyberespionaggio verso le Istituzioni dell'Unione Europea.
7. **ISO/IEC 27005:2022:** Standard internazionale per la gestione dei rischi di sicurezza delle informazioni (Sezione 6.4.2 per la definizione di Risk Appetite e soglie di tolleranza per asset Life Critical Tier 0).
8. **CrowdStrike Threat Hunting Report 2026 (Executive Summary):** Analisi delle minacce cross-domain, abuso di fiducia e compromissione dei flussi di autenticazione cloud (OAuth 2.0 device code flow, Entra ID)
. Evidenzia l'aumento delle intrusioni via voice phishing (vishing) per la compromissione degli account Single Sign-On (SSO)
 e l'abuso dei registri della software supply chain (npm, PyPI)
9. **MITRE ATT&CK v19.2 (Enterprise, Mobile, ICS):** Tassonomia e identificatori delle tecniche di attacco (T1190, T1566, T1078, T1486, T1195).

---

💡 **Feedback & Contributi:**  
I suggerimenti, le segnalazioni di miglioramento e le proposte di integrazione sono graditi ed incoraggiati!

---


