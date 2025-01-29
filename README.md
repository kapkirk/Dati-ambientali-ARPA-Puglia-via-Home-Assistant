# ARPA Puglia Environmental Data Integration

Automazione per l'acquisizione di dati ambientali da ARPA Puglia via API, elaborazione con Node-RED e invio a Home Assistant via MQTT.

## Struttura
- `node-red-flows/`: Flussi Node-RED
- `homeassistant-config/`: Configurazioni per Home Assistant
- `docs/`: Guide di setup

## Requisiti
- Node-RED con nodi `node-red-contrib-mqtt` e `node-red-contrib-http-request`
- Broker MQTT (es. Mosquitto)
- Accesso alle API di ARPA Puglia

---

## Configurazione

### Node-RED Flow (Esempio)
```json
[
  {
    "id": "1234567890",
    "type": "tab",
    "label": "ARPA Puglia Data",
    "nodes": [
      {
        "id": "http-request",
        "type": "http request",
        "z": "1234567890",
        "name": "Fetch ARPA Data",
        "method": "GET",
        "ret": "txt",
        "url": "https://api.arpapuglia.it/dati_ambiente",  // Sostituire con l'URL reale
        "tls": "",
        "x": 300,
        "y": 100
      },
      {
        "id": "process-data",
        "type": "function",
        "z": "1234567890",
        "name": "Converti per MQTT",
        "func": "// Elabora i dati dall'API\nconst payload = {\n  temperature: msg.payload.temperatura,\n  pm10: msg.payload.pm10,\n  timestamp: new Date().toISOString()\n};\nmsg.payload = payload;\nreturn msg;",
        "outputs": 1,
        "x": 500,
        "y": 100
      },
      {
        "id": "mqtt-out",
        "type": "mqtt out",
        "z": "1234567890",
        "name": "Invia a HA",
        "topic": "arpa/puglia/data",
        "qos": "0",
        "retain": "false",
        "broker": "mqtt-broker",  // Configurare il broker in Node-RED
        "x": 700,
        "y": 100
      },
      {
        "id": "scheduler",
        "type": "inject",
        "z": "1234567890",
        "name": "Ogni giorno alle 8:00",
        "props": [],
        "repeat": "86400",
        "crontab": "00 08 * * *",
        "once": true,
        "x": 100,
        "y": 100
      }
    ]
  }
]
