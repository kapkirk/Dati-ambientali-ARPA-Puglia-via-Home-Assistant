# ARPA Puglia Environmental Data Integration

Una utility [Home Assistant](https://home-assistant.io/) che ti aiuta a visualizzare i dati dei principali indicatori ambietali pubblicati da ARPA Puglia.

Scopo principale di queste righe è quello di automatizzare l'acquisizione giornaliera di dati ambientali pubblicati tramite API dalla Agenzia Regoonale Protezione e Prevenzione Ambientale per la Puglia, la loro elaborazione tramite Node-RED e la successiva integrazione in Home Assistant tramite pubblicazione MQTT per la visualizzazione in tempo reale.

---

## Caratteristiche principali

- **API Integration**: Collegamento diretto alle API di ARPA Puglia per il prelievo di dei seguenti parametri ambientali:
     1.  **PM10** (Polveri inalabili, Insieme di sostanze solide e liquide con diametro inferiore a 10 micron. Derivano  da emissioni di autoveicoli, processi industriali, fenomeni naturali)
     1.  **PM2.5** (Polveri respirabili, Insieme di sostanze solide e liquide con diametro inferiore a 2.5 micron. Derivano  da processi industriali, processi di combustione, emissionidi autoveicoli, fenomeni naturali)
     3.  **NO2** (Biossido di azoto, Gas tossico che si forma nelle combustioni ad alta temperatura. Sue principali sorgenti sono i motori a scoppio, gli impianti termici, le centrali termoelettriche)
     4.  **SO2** (Biossido di zolfo, Gas irritante, si forma soprattutto in seguito all'utilizzo di combustibili (carbone, petrolio, gasolio) contenenti impurezze di zolfo)
     5.  **CO** (Monossido di Carbonio, Sostanza gassosa, si forma per combustione incompleta di materiale organico, ad esempio nei motori degli autoveicoli e nei processi industriali)
     6.  **C6H6** (Benzene, Liquido volatile e dall'odore dolciastro. Deriva dalla combustione incompleta del carbone e del petrolio, dai gas esausti dei veicoli a motore, dal fumo di tabacco)
     7.  **IPA** (Idrocarburi Policiclici Aromatici, Classe di composti organici semi-volatili, con 2 o più da anelli benzenici condensati tra loro, generati dalla combustione incompleta di materiale organico. Il dato fornito si riferisce agli IPA adsorbiti su particelle carboniose con un diametro aerodinamico tra 0.01 e 1 micron)
     8.  **H2S** (Idrogeno solforato, Gas incolore dal caratteristico odore di uova marce, caratterizzato da una soglia olfattiva bassa.  E' generato nella produzione di carbon coke, nella lavorazione del petrolio,  di fertilizzanti, dei rifiutie di altri procedimenti industriali)
     9.  **O3** (Ozono, Sostanza non emessa direttamente in atmosfera, si forma per reazione tra altri inquinanti, principalmente NO2 e idrocarburi, in presenza di radiazione solare)
     10.  **BLACK CARB** (Black Carbon, Prodotto della combustione incompleta di combustibili fossili e biomassa; può essere emesso da sorgenti naturali ed antropiche sotto forma di fuliggine)
     11. **Qualità dell'aria**


- **Workflow Node-RED**: Flusso configurato per interrogare giornalmente le API, processare i dati (formattazione, validazione) e inviarli a un broker MQTT.
- **Comunicazione MQTT**: Trasmissione sicura dei dati a Home Assistant tramite protocollo MQTT, con topic dedicati per ciascun sensore.
- **Custom Sensor in Home Assistant**: Configurazione di sensori personalizzati in HA per ricevere i dati via MQTT e visualizzarli in dashboard.
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

## Installazione

### 1 - Prerequisiti

- Node-RED installato e configurato con nodi:
  - `node-red-contrib-mqtt`
  - `node-red-contrib-http-request`
- Broker MQTT (es. Mosquitto) in esecuzione e integrato con Home Assistant.
- Custom `flex-table-card` scaricabile da HACS installata e configurata (Se volete utilizzare la tabella di visualizzaizone come ho fatto io).


### 2 - Configurazione Home Assistant

1. Scaricare tutti i files;
1. Copiare il contenuto di 'sensori HA.txt' in  `configuration.yaml`. Se la voce `mqtt:` è già presente, accodate i sensori a quelli già presenti;

```yaml

#*******************************************************
#                                                      *
#                    Sensori qualità ambiente          *
#                                                      *
#*******************************************************

mqtt:
  sensor:
    - name: "NO2"
      state_topic: "sensors/no2"
      unit_of_measurement: "µg/m³"
      value_template: "{{ value_json.valore }}"
      json_attributes_topic: "sensors/no2"
      json_attributes_template: >
        {
          "data": "{{ value_json.data }}",
          "inquinante": "{{ value_json.inquinante }}",
          "limite": {{ value_json.limite }},
          "unita": "{{ value_json.unita }}",
          "indice_qualita": "{{ value_json.indice_qualita }}",
          "classe_qualita": "{{ value_json.classe_qualita }}",
          "superamenti": {{ value_json.superamenti }}
        }

    - name: "PM10"
      state_topic: "sensors/pm10"
      unit_of_measurement: "µg/m³"
      value_template: "{{ value_json.valore }}"
      json_attributes_topic: "sensors/pm10"
      json_attributes_template: >
        {
          "data": "{{ value_json.data }}",
          "inquinante": "{{ value_json.inquinante }}",
          "limite": {{ value_json.limite }},
          "unita": "{{ value_json.unita }}",
          "indice_qualita": "{{ value_json.indice_qualita }}",
          "classe_qualita": "{{ value_json.classe_qualita }}",
          "superamenti": {{ value_json.superamenti }}
        } 

    - name: "PM25"
      state_topic: "sensors/pm25"
      unit_of_measurement: "µg/m³"
      value_template: "{{ value_json.valore }}"
      json_attributes_topic: "sensors/pm25"
      json_attributes_template: >
        {
          "data": "{{ value_json.data }}",
          "inquinante": "{{ value_json.inquinante }}",
          "limite": {{ value_json.limite }},
          "unita": "{{ value_json.unita }}",
          "indice_qualita": "{{ value_json.indice_qualita }}",
          "classe_qualita": "{{ value_json.classe_qualita }}",
          "superamenti": {{ value_json.superamenti }}
        }         
        
    - name: "SO2"
      state_topic: "sensors/so2"
      unit_of_measurement: "µg/m³"
      value_template: "{{ value_json.valore }}"
      json_attributes_topic: "sensors/so2"
      json_attributes_template: >
        {
          "data": "{{ value_json.data }}",
          "inquinante": "{{ value_json.inquinante }}",
          "limite": {{ value_json.limite }},
          "unita": "{{ value_json.unita }}",
          "indice_qualita": "{{ value_json.indice_qualita }}",
          "classe_qualita": "{{ value_json.classe_qualita }}",
          "superamenti": {{ value_json.superamenti }}
        }         
        
    - name: "C6H6"
      state_topic: "sensors/c6h6"
      unit_of_measurement: "µg/m³"
      value_template: "{{ value_json.valore }}"
      json_attributes_topic: "sensors/c6h6"
      json_attributes_template: >
        {
          "data": "{{ value_json.data }}",
          "inquinante": "{{ value_json.inquinante }}",
          "limite": {{ value_json.limite }},
          "unita": "{{ value_json.unita }}",
          "indice_qualita": "{{ value_json.indice_qualita }}",
          "classe_qualita": "{{ value_json.classe_qualita }}",
          "superamenti": {{ value_json.superamenti }}
        }         
        
    - name: "CO"
      state_topic: "sensors/co"
      unit_of_measurement: "µg/m³"
      value_template: "{{ value_json.valore }}"
      json_attributes_topic: "sensors/co"
      json_attributes_template: >
        {
          "data": "{{ value_json.data }}",
          "inquinante": "{{ value_json.inquinante }}",
          "limite": {{ value_json.limite }},
          "unita": "{{ value_json.unita }}",
          "indice_qualita": "{{ value_json.indice_qualita }}",
          "classe_qualita": "{{ value_json.classe_qualita }}",
          "superamenti": {{ value_json.superamenti }}
        } 
        
    - name: "IPA"
      state_topic: "sensors/ipa"
      unit_of_measurement: "µg/m³"
      value_template: "{{ value_json.valore }}"
      json_attributes_topic: "sensors/ipa"
      json_attributes_template: >
        {
          "data": "{{ value_json.data }}",
          "inquinante": "{{ value_json.inquinante }}",
          "limite": {{ value_json.limite }},
          "unita": "{{ value_json.unita }}",
          "indice_qualita": "{{ value_json.indice_qualita }}",
          "classe_qualita": "{{ value_json.classe_qualita }}",
          "superamenti": {{ value_json.superamenti }}
        } 
```

1. Nella Dashbord della lovelace, dove preferite, aprite una nuova scheda ed incollate il codice del file `HA lovelace.txt` ed il risultato sarà questo:

[https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/Lovelace%20Visualizzazione%20HA.jpg]

ecco il codice:

```yaml
type: custom:flex-table-card
title: Valori Ambientali San Pietro Vernotico
entities:
  include:
    - sensor.pm10
    - sensor.pm25
    - sensor.no2
    - sensor.so2
    - sensor.co
    - sensor.c6h6
    - sensor.ipa
  sort_by: name
columns:
  - name: Inquinante
    data: inquinante
  - name: Valore
    data: state
  - name: Unita
    data: unita
  - name: Limite
    data: limite
  - name: Superamenti
    data: superamenti
  - name: Indice qualità
    data: indice_qualita
  - name: Classe qualità
    data: classe_qualita
```

### 3 - Configurazione di Node-RED

1. Copiate ed incollate il contenuto del file `Flusso Node-RED singola centralina.json` oppure `Flusso Node-RED più centralina.json` in un foglio di NodeRED:

`Flusso Node-RED singola centralina.json`:
```json
[{"id":"0fc96737abf2fa25","type":"http request","z":"ffcf04d739843cc8","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=26&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":130,"y":240,"wires":[["45d325f5c135d59b"]]},{"id":"be3859fca160266f","type":"inject","z":"ffcf04d739843cc8","name":"08.00","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"00 08 * * *","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":150,"y":80,"wires":[["0fc96737abf2fa25","7a2723b5493def9c","8d78bf9f78453e8c"]]},{"id":"45d325f5c135d59b","type":"csv","z":"ffcf04d739843cc8","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_null_values":"","x":270,"y":240,"wires":[["1dc499b20f7cf18e"]]},{"id":"1dc499b20f7cf18e","type":"function","z":"ffcf04d739843cc8","name":"Estrae i dati delle misurazioni","func":"// Ottieni l'array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell'array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all'array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":460,"y":240,"wires":[["f8fbe8a712bda9dd"]]},{"id":"f8fbe8a712bda9dd","type":"function","z":"ffcf04d739843cc8","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l'array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell'array\nif (misurazioni.length >= 2) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":2,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":750,"y":240,"wires":[["68f810c0b9e6c963"],["e0d2781f9f37aec6"]],"info":"### ## # "},{"id":"782ec5001a0c31e9","type":"mqtt out","z":"ffcf04d739843cc8","name":"NO2","topic":"sensors/no2","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1330,"y":280,"wires":[]},{"id":"377b5abe289f8a25","type":"mqtt out","z":"ffcf04d739843cc8","name":"PM10","topic":"sensors/pm10","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1330,"y":200,"wires":[]},{"id":"68f810c0b9e6c963","type":"function","z":"ffcf04d739843cc8","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"null\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"null\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"null\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"null\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"null\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1090,"y":200,"wires":[["377b5abe289f8a25"]]},{"id":"e0d2781f9f37aec6","type":"function","z":"ffcf04d739843cc8","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"null\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"null\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"null\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"null\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"null\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1090,"y":280,"wires":[["782ec5001a0c31e9"]]},{"id":"374295b309ce444f","type":"mqtt-broker","name":"HA MQTT","broker":"192.168.31.160","port":"1883","clientid":"","autoConnect":true,"usetls":false,"protocolVersion":"4","keepalive":"60","cleansession":true,"autoUnsubscribe":true,"birthTopic":"","birthQos":"0","birthRetain":"false","birthPayload":"","birthMsg":{},"closeTopic":"","closeQos":"0","closeRetain":"false","closePayload":"","closeMsg":{},"willTopic":"","willQos":"0","willRetain":"false","willPayload":"","willMsg":{},"userProps":"","sessionExpiry":""}]
```

oppure `Flusso Node-RED più centralina.json`:

```json
[{"id":"2a235b7b433bbb28","type":"http request","z":"7e71bf58d0d8df1b","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=26&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":250,"y":260,"wires":[["9eab0eaff1ec6956"]]},{"id":"daf9aeb5ce35cda6","type":"inject","z":"7e71bf58d0d8df1b","name":"08.00","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"00 08 * * *","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":270,"y":100,"wires":[["2a235b7b433bbb28","34ab8ce6de39ed9f","17b49fe2099cae18"]]},{"id":"9eab0eaff1ec6956","type":"csv","z":"7e71bf58d0d8df1b","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_null_values":"","x":390,"y":260,"wires":[["bc1b63ee1e8f0f29"]]},{"id":"bc1b63ee1e8f0f29","type":"function","z":"7e71bf58d0d8df1b","name":"Estrae i dati delle misurazioni","func":"// Ottieni l'array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell'array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all'array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":580,"y":260,"wires":[["c179c0eb7be60c8b"]]},{"id":"c179c0eb7be60c8b","type":"function","z":"7e71bf58d0d8df1b","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l'array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell'array\nif (misurazioni.length >= 2) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":2,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":870,"y":260,"wires":[["ae96a5ccee3735fd"],["d61ca08685d2e68f"]],"info":"### ## # "},{"id":"61e79e4d18ac53ff","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"NO2","topic":"sensors/no2","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":300,"wires":[]},{"id":"e3b8e52970320c3d","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"PM10","topic":"sensors/pm10","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":220,"wires":[]},{"id":"ae96a5ccee3735fd","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"null\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"null\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"null\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"null\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"null\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":220,"wires":[["e3b8e52970320c3d"]]},{"id":"640e1cd659bc8bcc","type":"http request","z":"7e71bf58d0d8df1b","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=93&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":250,"y":500,"wires":[["7b24b8a365cbd0b7"]]},{"id":"7b24b8a365cbd0b7","type":"csv","z":"7e71bf58d0d8df1b","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_null_values":"","x":390,"y":500,"wires":[["f7125ef0498ddb37"]]},{"id":"f7125ef0498ddb37","type":"function","z":"7e71bf58d0d8df1b","name":"Estrae i dati delle misurazioni","func":"// Ottieni l'array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell'array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all'array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":580,"y":500,"wires":[["c7de10a4328dec6d"]]},{"id":"c7de10a4328dec6d","type":"function","z":"7e71bf58d0d8df1b","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l'array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell'array\nif (misurazioni.length >= 4) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n    let msg3 = { payload: misurazioni[2] }; // Terzo oggetto\n    let msg4 = { payload: misurazioni[3] }; // Quarto oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2, msg3, msg4];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":4,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":870,"y":500,"wires":[[],["58c90af411350d55"],[],["14571d8042e7b752"]],"info":"### ## # "},{"id":"a70c8588afb667ec","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"SO2","topic":"sensors/so2","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":540,"wires":[]},{"id":"08c20a666bf9b9ef","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"PM25","topic":"sensors/pm25","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":460,"wires":[]},{"id":"48b112b1b5f0cc47","type":"http request","z":"7e71bf58d0d8df1b","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=27&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":250,"y":640,"wires":[["5d7c315282a8de56"]]},{"id":"5d7c315282a8de56","type":"csv","z":"7e71bf58d0d8df1b","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_null_values":"","x":390,"y":640,"wires":[["8c601449f5f7080a"]]},{"id":"8c601449f5f7080a","type":"function","z":"7e71bf58d0d8df1b","name":"Estrae i dati delle misurazioni","func":"// Ottieni l'array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell'array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all'array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":580,"y":640,"wires":[["ab1d71e891de1336"]]},{"id":"ab1d71e891de1336","type":"function","z":"7e71bf58d0d8df1b","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l'array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell'array\nif (misurazioni.length >= 7) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n    let msg3 = { payload: misurazioni[2] }; // Terzo oggetto\n    let msg4 = { payload: misurazioni[3] }; // Quarto oggetto\n    let msg5 = { payload: misurazioni[4] }; // Quinto oggetto\n    let msg6 = { payload: misurazioni[5] }; // Sesto oggetto\n    let msg7 = { payload: misurazioni[6] }; // settimo oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2, msg3, msg4, msg5, msg6, msg7];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":7,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":870,"y":640,"wires":[[],[],[],["e8030c314ae0b123"],["71fa75be75d2a4fa"],[],["526e230a4fe3aa4d"]],"info":"### ## # "},{"id":"899b55a471331944","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"CO","topic":"sensors/co","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":660,"wires":[]},{"id":"de42a07e56cd4752","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"C6H6","topic":"sensors/c6h6","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":600,"wires":[]},{"id":"4429c59fa49d1159","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"IPA","topic":"sensors/ipa","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":720,"wires":[]},{"id":"34ab8ce6de39ed9f","type":"delay","z":"7e71bf58d0d8df1b","name":"","pauseType":"delay","timeout":"5","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"allowrate":false,"outputs":1,"x":240,"y":440,"wires":[["640e1cd659bc8bcc"]]},{"id":"17b49fe2099cae18","type":"delay","z":"7e71bf58d0d8df1b","name":"","pauseType":"delay","timeout":"10","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"allowrate":false,"outputs":1,"x":240,"y":580,"wires":[["48b112b1b5f0cc47"]]},{"id":"d61ca08685d2e68f","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"null\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"null\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"null\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"null\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"null\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":300,"wires":[["61e79e4d18ac53ff"]]},{"id":"58c90af411350d55","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"null\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"null\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"null\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"null\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"null\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":460,"wires":[["08c20a666bf9b9ef"]]},{"id":"14571d8042e7b752","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"null\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"null\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"null\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"null\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"null\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":540,"wires":[["a70c8588afb667ec"]]},{"id":"e8030c314ae0b123","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"null\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"null\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"null\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"null\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"null\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":600,"wires":[["de42a07e56cd4752"]]},{"id":"71fa75be75d2a4fa","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"null\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"null\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"null\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"null\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"null\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":660,"wires":[["899b55a471331944"]]},{"id":"526e230a4fe3aa4d","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"null\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"null\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"null\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"null\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"null\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":720,"wires":[["4429c59fa49d1159"]]},{"id":"b6920f1155d953e0","type":"comment","z":"7e71bf58d0d8df1b","name":"Dati centralina San Pietro Vernotico","info":"","x":800,"y":140,"wires":[]},{"id":"165037e5d42fa2c9","type":"comment","z":"7e71bf58d0d8df1b","name":"Dati centraline Torchiarolo","info":"","x":810,"y":400,"wires":[]},{"id":"374295b309ce444f","type":"mqtt-broker","name":"HA MQTT","broker":"192.168.31.160","port":"1883","clientid":"","autoConnect":true,"usetls":false,"protocolVersion":"4","keepalive":"60","cleansession":true,"autoUnsubscribe":true,"birthTopic":"","birthQos":"0","birthRetain":"false","birthPayload":"","birthMsg":{},"closeTopic":"","closeQos":"0","closeRetain":"false","closePayload":"","closeMsg":{},"willTopic":"","willQos":"0","willRetain":"false","willPayload":"","willMsg":{},"userProps":"","sessionExpiry":""}]
```
---

## Benefici

Una soluzione replicabile per chiunque voglia monitorare dati ambientali ufficiali di ARPA Puglia in Home Assistant, con flessibilità per:
- Estendere i parametri acquisiti.
- Modificare la schedulazione degli aggiornamenti.
- Personalizzare dashboard e alert.

---


