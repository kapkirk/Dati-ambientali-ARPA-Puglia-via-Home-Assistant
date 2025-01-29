# ARPA Puglia Environmental Data Integration

**Scopo**: Automatizzare l'acquisizione giornaliera di dati ambientali dall'API di ARPA Puglia, elaborarli tramite Node-RED e integrarli in Home Assistant via MQTT per la visualizzazione in tempo reale.

---

## Caratteristiche principali

- **API Integration**: Collegamento diretto alle API di ARPA Puglia per il prelievo di parametri ambientali (es. qualità dell'aria, temperatura, umidità, PM10/PM2.5).
- **Workflow Node-RED**: Flusso configurato per interrogare giornalmente le API, processare i dati (formattazione, validazione) e inviarli a un broker MQTT.
- **Comunicazione MQTT**: Trasmissione sicura dei dati a Home Assistant tramite protocollo MQTT, con topic dedicati per ciascun sensore.
- **Custom Sensor in Home Assistant**: Configurazione di sensori personalizzati in HA per ricevere i dati via MQTT e visualizzarli in dashboard (es. Lovelace UI).
- **Automazione Giornaliera**: Schedulazione degli aggiornamenti per garantire dati sempre aggiornati senza intervento manuale.

---

## Funzionamento

1. **Acquisizione Dati**:  
   Node-RED effettua chiamate API periodiche ad ARPA Puglia e filtra i dati rilevanti.

2. **Elaborazione**:  
   I dati vengono convertiti in payload MQTT compatibili (es. JSON strutturato).

3. **Invio a Home Assistant**:  
   I payload sono pubblicati su un broker MQTT (es. Mosquitto) e ricevuti da HA tramite listener MQTT.

4. **Creazione Sensori**:  
   Script YAML in HA definiscono sensori che mappano i dati MQTT su entità utilizzabili (es. `sensor.temperatura_arpapuglia`).

5. **Visualizzazione**:  
   Integrazione in dashboard per monitoraggio ambientale centralizzato.

---


---

## Prerequisiti

- Node-RED installato e configurato con nodi:
  - `node-red-contrib-mqtt`
  - `node-red-contrib-http-request`
- Broker MQTT (es. Mosquitto) in esecuzione e integrato con Home Assistant.
- Accesso alle API di ARPA Puglia (credenziali/autorizzazioni richieste).

---

## Benefici

Una soluzione replicabile per chiunque voglia monitorare dati ambientali ufficiali in Home Assistant, con flessibilità per:
- Estendere i parametri acquisiti.
- Modificare la schedulazione degli aggiornamenti.
- Personalizzare dashboard e alert.

---

*Include template configurabili ed esempi pratici per un deploy rapido.*
