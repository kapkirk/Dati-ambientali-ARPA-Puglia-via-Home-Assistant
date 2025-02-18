| [italiano](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#acquisizione-dati-inquinanti-ambientali-arpa-puglia-in-home-assistant) | [inglese](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#arpa-puglia-environmental-pollutant-data-acquisition-in-home-assistant) |

![](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/HA%20logo2.png)  ` ` ![](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/MQTT.png)            `  ` ![](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/Node-RED%20logo.jpg)
---   
# Acquisizione dati inquinanti ambientali ARPA Puglia in Home Assistant

Una utility [Home Assistant](https://home-assistant.io/) che ti aiuta a visualizzare i dati dei principali indicatori ambientali pubblicati da ARPA Puglia utilizzando [Node-RED](https://nodered.org/).

<br>

## Tavola dei Contenuti

1. **[Descrizione del progetto](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#descrizione-del-progetto)**

2. **[Caratteristiche principali](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#caratteristiche-principali)**

3. **[Descrizione del funzionamento](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#descrizione-del-funzionamento)**

4. **[Installazione](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#installazione)**
   1. **[Prerequisiti](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#1---prerequisiti)**
   1. **[Configurazione dei dati da acquisire](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#2---configurazione-dei-dati-da-acquisire)**
   1. **[Configurazione Home Assistant](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#3---configurazione-home-assistant)**
   2. **[Configurazione di Node-RED](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#4---configurazione-di-node-red)**

5. **[Personalizzazione dei flussi di NodeRed](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#personalizzazione-ed-analisi-dei-flussi-nodered)**

6. **[Commento del codice](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#commentiamolo)**  
 

 
<br>

## Descrizione del progetto

Scopo principale di queste righe è quello di automatizzare l`acquisizione giornaliera di dati ambientali pubblicati tramite API dalla Agenzia Regionale Protezione e Prevenzione Ambientale per la Puglia, la loro elaborazione tramite Node-RED e la successiva integrazione in Home Assistant tramite pubblicazione MQTT per la visualizzazione in tempo reale.

Tutte le info sono disponibili sul sito di [ARPA Puglia](https://www.arpa.puglia.it/pagina2795_aria.html) secondo al [mappa](https://dati.arpa.puglia.it/qaria?footer=false%EF%BB%BF)

---

## Caratteristiche principali

- **API Integration**: Collegamento diretto alle API di ARPA Puglia per il prelievo di dei seguenti parametri ambientali:
     1.  **PM10** (Polveri inalabili. Insieme di sostanze solide e liquide con diametro inferiore a 10 micron. Derivano  da emissioni di autoveicoli, processi industriali, fenomeni naturali)
     1.  **PM2.5** (Polveri respirabili. Insieme di sostanze solide e liquide con diametro inferiore a 2.5 micron. Derivano  da processi industriali, processi di combustione, emissioni di autoveicoli, fenomeni naturali)
     3.  **NO2** (Biossido di azoto. Gas tossico che si forma nelle combustioni ad alta temperatura. Sue principali sorgenti sono i motori a scoppio, gli impianti termici, le centrali termoelettriche)
     4.  **SO2** (Biossido di zolfo. Gas irritante, si forma soprattutto in seguito all`utilizzo di combustibili (carbone, petrolio, gasolio) contenenti impurezze di zolfo)
     5.  **CO** (Monossido di Carbonio. Sostanza gassosa, si forma per combustione incompleta di materiale organico, ad esempio nei motori degli autoveicoli e nei processi industriali)
     6.  **C6H6** (Benzene. Liquido volatile e dall`odore dolciastro. Deriva dalla combustione incompleta del carbone e del petrolio, dai gas esausti dei veicoli a motore, dal fumo di tabacco)
     7.  **IPA** (Idrocarburi Policiclici Aromatici. Classe di composti organici semi-volatili, con 2 o più da anelli benzenici condensati tra loro, generati dalla combustione incompleta di materiale organico. Il dato fornito si riferisce agli IPA adsorbiti su particelle carboniose con un diametro aerodinamico tra 0.01 e 1 micron)
     8.  **H2S** (Idrogeno solforato. Gas incolore dal caratteristico odore di uova marce, caratterizzato da una soglia olfattiva bassa.  E` generato nella produzione di carbon coke, nella lavorazione del petrolio,  di fertilizzanti, dei rifiuti e di altri procedimenti industriali)
     9.  **O3** (Ozono. Sostanza non emessa direttamente in atmosfera, si forma per reazione tra altri inquinanti, principalmente NO2 e idrocarburi, in presenza di radiazione solare)
     10.  **BLACK CARB** (Black Carbon. Prodotto della combustione incompleta di combustibili fossili e biomassa; può essere emesso da sorgenti naturali ed antropiche sotto forma di fuliggine)
     11. **Qualità dell`aria**

**Disclaimer**: Non tutte le centraline espongono tutti i dati, pertanto, ho deciso di pubblicare due flussi di node-red da poter poi integrare con i dati eventualmente recuperati direttamente dal sito ARPA di accesso alle loro API: 

[API_Regione_Puglia](https://dati.arpa.puglia.it/openapi/index.html)

- **Workflow Node-RED**: Flusso configurato per interrogare giornalmente le API, processare i dati (formattazione, validazione) e inviarli a un broker MQTT.
- **Comunicazione MQTT**: Trasmissione sicura dei dati a Home Assistant tramite protocollo MQTT, con topic dedicati per ciascun sensore.
- **Custom Sensor in Home Assistant**: Configurazione di sensori personalizzati in HA per ricevere i dati via MQTT e visualizzarli in dashboard.
- **Automazione Giornaliera**: Schedulazione degli aggiornamenti per garantire dati sempre aggiornati senza intervento manuale.

---

## Descrizione del Funzionamento

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
- Custom `flex-table-card` scaricabile da HACS installata e configurata (Se volete utilizzare la tabella di visualizzazione come ho fatto io).


### 2 - Configurazione dei dati da acquisire

1.Come già anticipato i dati sono acquisiti tramite le API pubbliche messe a disposizione dall`ARPA. Le centraline disponibili (dato aggiornato al 29.1.2025) sono le seguenti:

|id_station|             denominazione                                |                       comune    | indirizzo               |rete   |interesse_rete|provincia    |   paese   |paese_esteso  |  Latitude    |Longitude     |    sorgente  |
| -------- | -------------------------------------------------------- | ------------------------------- | ----------------------- | ----- | ------------ | ----------- | --------- | ------------ | ------------ | ------------ | ------------ |
|8|Monte S. Angelo - Ciuffreda|Monte Sant Angelo|Suolo Ciuffreda|RRQA|PUBBLICO|Foggia|IT|Italy|15.945254|41.666109|ARPAP
|10|Manfredonia - Mandorli|Manfredonia|Via dei Mandorli|RRQA|PUBBLICO|Foggia|IT|Italy|15.909638|41.629333|ARPAP
|14|Molfetta - Verdi|Molfetta|P.zza Verdi|RRQA|PUBBLICO|Bari|IT|Italy|16.605253|41.201101|ARPAP
|17|Bari - Caldarola|Bari|Via Caldarola|RRQA|PUBBLICO|Bari|IT|Italy|16.888064|41.113542|ARPAP
|18|Statte - Sorgenti|Statte|Via Delle Sorgenti|RRQA|PUBBLICO|Taranto|IT|Italy|17.203325|40.562499|ARPAP
|19|Taranto - Archimede|Taranto|Via Archimede|RRQA|PUBBLICO|Taranto|IT|Italy|17.233048|40.494441|ARPAP
|20|Taranto - Machiavelli|Taranto|Via Machiavelli|RRQA|PUBBLICO|Taranto|IT|Italy|17.225823|40.488608|ARPAP
|21|Taranto - San Vito|Taranto|presso Colonia Marina|RRQA|PUBBLICO|Taranto|IT|Italy|17.225272|40.423328|ARPAP
|22|Taranto - Alto Adige|Taranto|Via Alto Adige presso scuola XX Circolo|RRQA|PUBBLICO|Taranto|IT|Italy|17.263602|40.460553|ARPAP
|23|Mesagne - Via Udine|Mesagne|Via Udine|RRQA|PUBBLICO|Brindisi|IT|Italy|17.807998|40.565995|ARPAP
|24|Brindisi - Via Taranto|Brindisi|Via Taranto|RRQA|PUBBLICO|Brindisi|IT|Italy|17.947777|40.634166|ARPAP
|25|San Pancrazio|San Pancrazio Salentino|---|RRQA|PUBBLICO|Brindisi|IT|Italy|17.845999|40.422993|ARPAP
|26|San Pietro Vernotico|San Pietro Vernotico|---|RRQA|PUBBLICO|Brindisi|IT|Italy|18.005994|40.485998|ARPAP
|27|Torchiarolo - Don Minzoni|Torchiarolo|P.za Don Minzoni|RRQA|PUBBLICO|Brindisi|IT|Italy|18.053991|40.487999|ARPAP
|28|Guagnano - Villa Baldassarri|Guagnano|Villa Baldassarri|RRQA|PUBBLICO|Lecce|IT|Italy|17.964477|40.418525|ARPAP
|29|Lecce - Cerrate|Lecce|S. Maria Cerrate|RRQA|PUBBLICO|Lecce|IT|Italy|18.116386|40.459696|ARPAP
|31|Arnesano - Riesci|Arnesano|Zona Riesci|RRQA|PUBBLICO|Lecce|IT|Italy|18.095074|40.346270|ARPAP
|33|Brindisi - Via dei Mille|Brindisi|Via dei Mille|RRQA|PUBBLICO|Brindisi|IT|Italy|17.938152|40.638748|ARPAP
|35|Brindisi - Casale|Brindisi|Via San Giusto|RRQA|PUBBLICO|Brindisi|IT|Italy|17.942569|40.650083|ARPAP
|36|Brindisi - SISRI|Brindisi|Via Curie|RRQA|PUBBLICO|Brindisi|IT|Italy|17.975041|40.624738|ARPAP
|37|Taranto - CISI|Taranto|q.re Paolo VI - presso CISI Puglia|RRQA|PUBBLICO|Taranto|IT|Italy|17.253416|40.520934|ARPAP
|38|Taranto - Talsano|Taranto|Talsano - presso scuola U.Foscolo|RRQA|PUBBLICO|Taranto|IT|Italy|17.283879|40.411943|ARPAP
|40|Statte - Wind|Statte|ss.7 presso il ponte radio wind|RRQA|PUBBLICO|Taranto|IT|Italy|17.173611|40.526111|ARPAP
|41|Grottaglie|Grottaglie|via XXV luglio|RRQA|PUBBLICO|Taranto|IT|Italy|17.423879|40.537776|ARPAP
|42|Martina Franca|Martina Franca|Via della Stazione|RRQA|PUBBLICO|Taranto|IT|Italy|17.331939|40.700826|ARPAP
|47|Lecce - Garigliano|Lecce|---|RRQA|PUBBLICO|Lecce|IT|Italy|18.174327|40.364457|ARPAP
|49|Maglie|Maglie|Via Don. L. Sturzo 4|---|PUBBLICO|Lecce|IT|Italy|18.294098|40.123636|ARPAP
|50|Galatina - I.T.C.  La Porta |Galatina|Viale degli studenti|RRQA|PUBBLICO|Lecce|IT|Italy|18.174725|40.166947|ARPAP
|51|Campi Salentina|Campi Salentina|Via Napoli|RRQA|PUBBLICO|Lecce|IT|Italy|18.026509|40.397507|ARPAP
|54|Bari - Cavour|Bari|Corso Cavour|RRQA|PUBBLICO|Bari|IT|Italy|16.872552|41.122269|ARPAP
|55|Bari - Kennedy|Bari|Piazza R. Kennedy|RRQA|PUBBLICO|Bari|IT|Italy|16.858905|41.099593|ARPAP
|62|Lecce - Libertini|Lecce|Piazza Libertini|RRQA|PUBBLICO|Lecce|IT|Italy|18.176667|40.351945|ARPAP
|63|Andria - Vaccina|Andria|Via Vaccina|RRQA|PUBBLICO|BAT|IT|Italy|16.303939|41.233278|ARPAP
|64|Altamura - Via Santeramo|Altamura|Via Golgota|RRQA|PUBBLICO|Bari|IT|Italy|16.561015|40.828848|ARPAP
|65|Casamassima - LaPenna|Casamassima|Via Lapenna|RRQA|PUBBLICO|Bari|IT|Italy|16.920731|40.953154|ARPAP
|66|Monopoli - Aldo Moro|Monopoli|Viale Aldo Moro|RRQA|PUBBLICO|Bari|IT|Italy|17.290285|40.951167|ARPAP
|67|Brindisi - Terminal Passeggeri|Brindisi|presso Terninal Passeggeri sulla banchina portuale di Costa Morena|RRQA|PUBBLICO|Brindisi|IT|Italy|17.961687|40.647432|ARPAP
|71|Barletta - Casardi|Barletta|via Casardi|RRQA|PUBBLICO|BAT|IT|Italy|16.286111|41.316666|ARPAP
|72|Francavilla Fontana|Francavilla Fontana|Via Fabio Filzi|RRQA|PUBBLICO|Brindisi|IT|Italy|17.588331|40.529163|ARPAP
|75|Massafra|Massafra|Via Frappietri|RRQA|PUBBLICO|Taranto|IT|Italy|17.116690|40.593761|ARPAP
|76|Foggia - Rosati|Foggia|Via Giuseppe Rosati 139|RRQA|PUBBLICO|Foggia|IT|Italy|15.548611|41.455555|ARPAP
|77|Brindisi - Perrino|Brindisi|Via Crati|RRQA|PUBBLICO|Brindisi|IT|Italy|17.954778|40.631361|ARPAP
|78|Brindisi - Cappuccini|Brindisi|Via Cappuccini|IL|PUBBLICO|Brindisi|IT|Italy|17.921778|40.630917|ARPAP
|81|Bari - Carbonara|Bari|via Loguercio|RRQA|PUBBLICO|Bari|IT|Italy|16.866944|41.076666|ARPAP
|85|San Severo - Azienda Russo|San Severo|Localita` Palmori|RRQA|PUBBLICO|Foggia|IT|Italy|15.440833|41.546666|ARPAP
|90|Bari - CUS|Bari|Lungomare Starita presso CUS BARI|RRQA|PUBBLICO|Bari|IT|Italy|16.845277|41.134722|ARPAP
|92|Monopoli - Liceo Artistico Russo|Monopoli|Via Cosimo Pisonio|RRQA|PUBBLICO|Bari|IT|Italy|17.284267|40.961578|ARPAP
|93|Torchiarolo - Fanin     |Torchiarolo|Via FANIN SN - 72020 TORCHIAROLO      |RRQA|PUBBLICO|Brindisi|IT|Italy|18.047226|40.489447|ARPAP
|94|Torchiarolo-Lendinuso|Torchiarolo|C.da MONTEVACCARO SN - 72020 TORCHIAROLO      |IL|PUBBLICO|Brindisi|IT|Italy|18.078880|40.517502|ARPAP
|95|Surbo - Croce |Surbo|Via B. Croce  S.N. - 73010 SURBO    |RRQA|PUBBLICO|Lecce|IT|Italy|18.120835|40.411940|ARPAP
|96|Ceglie Messapica|Ceglie Messapica|via Martina  (Scuola Elementare Papa Giovanni XXIII )|RRQA|PUBBLICO|Brindisi|IT|Italy|17.512500|40.649166|ARPAP
|97|Modugno - EN02|Modugno|Viale delle Magnolie 6|RRQA|PUBBLICO|Bari|IT|Italy|16.766111|41.107777|ARPAP
|98|Modugno - EN03|Modugno|Via Maranda|RRQA|PUBBLICO|Bari|IT|Italy|16.781672|41.087222|ARPAP
|99|Modugno - EN04|Modugno|Via Ancona|RRQA|PUBBLICO|Bari|IT|Italy|16.787777|41.114444|ARPAP
|100|Cokeria|Taranto|---|ADI|PRIVATO|ILVA|IT|Italy|17.217630|40.502050|ARPAP
|101|Tamburi - Via Orsini|Taranto|---|ADI|PRIVATO|ILVA|IT|Italy|17.225830|40.494510|ARPAP
|102|Riv1|Taranto|---|ADI|PRIVATO|ILVA|IT|Italy|17.216900|40.518570|ARPAP
|103|Direzione|Taranto|---|ADI|PRIVATO|ILVA|IT|Italy|17.200550|40.501980|ARPAP
|104|Meteo Parchi|Taranto|---|ADI|PRIVATO|ILVA|IT|Italy|17.222180|40.496730|ARPAP
|105|Portineria C|Taranto|---|ADI|PRIVATO|ILVA|IT|Italy|17.186560|40.518900|ARPAP
|106|Bitonto - EN01|Bitonto|Agro Di Bitonto|IL|PRIVATO|Bari|IT|Italy|16.745277|41.079166|ARPAP
|107|Palo del Colle - EN05|Palo del Colle|Via Ungaretti|IL|PRIVATO|Bari|IT|Italy|16.700830|41.061388|ARPAP
|108|Cisternino|Cisternino|via Benedetto Croce|RRQA|PUBBLICO|Brindisi|IT|Italy|17.415833|40.742777|ARPAP
|109|Barletta - Mezzo Mobile Via Trani|Barletta|---|IL|PRIVATO|BAT|IT|Italy|16.293055|41.318333|ARPAP
|110|Candela - Scuola|Candela|---|IL|PRIVATO|Foggia|IT|Italy|15.519444|41.133055|ARPAP
|111|Candela - Ex Comes|Candela|---|IL|PRIVATO|Foggia|IT|Italy|15.523055|41.173055|ARPAP
|112|San Severo  - Municipio|San Severo|---|RRQA|PUBBLICO|Foggia|IT|Italy|15.379722|41.696944|ARPAP
|113|Galatina-Colacem|Galatina|Contrada Piani|IL|PUBBLICO|Lecce|IT|Italy|18.192856|40.163964|ARPAP
|114|C1 - Lato sud ovest impianti| Area imprese Brindisi|Zona industriale - Via Leonardo da Vinci|VERSALIS|PRIVATO|Brindisi|IT|Italy|17.990417|40.628028|ARPAP
|115|C2 - Area bacino idrico|Brindisi Zona industriale | Via Giuseppe Verdi|VERSALIS|PRIVATO|Brindisi|IT|Italy|17.988500|40.638889|ARPAP
|116|C3 - Area impianto P1CR Cracking|Brindisi Zona industriale| Via Aldo Moro|VERSALIS|PRIVATO|Brindisi|IT|Italy|17.998139|40.638139|ARPAP
|117|C4 - Area impianto Butadiene|Brindisi Zona industriale |Via Enrico Fermi|VERSALIS|PRIVATO|Brindisi|IT|Italy|18.002444|40.634444|ARPAP
|118|C5 - Area Pontile|Brindisi Zona industriale| Via Galileo Galilei|VERSALIS|PRIVATO|Brindisi|IT|Italy|17.989981|40.645450|ARPAP
|119|C6 - Area PE1/2 lato sud impianti|Brindisi Zona industriale| Via Alessandro Volta|VERSALIS|PRIVATO|Brindisi|IT|Italy|18.001614|40.623753|ARPAP

1. Particolare importanza assume l`individuazione dell``id_station` perchè sarà, di fatto, l`unico dato da modificare nella stringa di lettura dei dati.
1. La stringa di lettura è la seguente:

   https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=26&debugMode=false

   può essere utilizzare anche nel normale browser. La sua consultazione genererà il download di un file che potrete leggere ed apprezzare soprattutto perchè per la centralina che avrete individuato, vi consentirà di sapere quali sono i dati pubblicati da ARPA.

1. Sostituire l``id_station` con quello della stazione desiderata:

      `https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&`**id_station=26**`&debugMode=false`

1. Provate la stringa e se la risposta è giusta (sarà di facile lettura) conservartela per inserirla nel giusto nodo NodeRED. 


    
### 3 - Configurazione Home Assistant

1. Scaricare tutti i files;
1. Copiare il contenuto di `sensori HA.txt` in  `configuration.yaml`. Se la voce `mqtt:` è già presente, accodate i sensori a quelli già presenti;
1. Potete aggiungere altri sensori rekativi agli inquinanti se la centraline da voi scelte ne espongono di diversi, la configurazione è identica per tutti, basta cambiare il nome;
1. La seguente configurazione esporrà i sensori in HA come vedete di seguito:

![lovelace](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/Esposizione%20HA.jpg)



codice:
```yaml

# configuration.yaml

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

1. Nella _Dashbord_ della _lovelace_, dove preferite, aprite una nuova scheda ed incollate il codice del file `HA lovelace.txt` ed il risultato sarà questo:

![lovelace](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/Lovelace%20Visualizzazione%20HA.jpg)

codice:
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

### 4 - Configurazione di Node-RED

1. Copiate ed incollate il contenuto del file `Flusso Node-RED singola centralina.json` oppure `Flusso Node-RED più centralina.json` in un foglio di NodeRED:

`Flusso Node-RED singola centralina.json`:

![nodered1](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/Flusso%20Node-RED.jpg)


codice:
```json
[{"id":"0fc96737abf2fa25","type":"http request","z":"ffcf04d739843cc8","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=26&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":130,"y":240,"wires":[["45d325f5c135d59b"]]},{"id":"be3859fca160266f","type":"inject","z":"ffcf04d739843cc8","name":"08.00","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"00 08 * * *","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":150,"y":80,"wires":[["0fc96737abf2fa25","7a2723b5493def9c","8d78bf9f78453e8c"]]},{"id":"45d325f5c135d59b","type":"csv","z":"ffcf04d739843cc8","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_---_values":"","x":270,"y":240,"wires":[["1dc499b20f7cf18e"]]},{"id":"1dc499b20f7cf18e","type":"function","z":"ffcf04d739843cc8","name":"Estrae i dati delle misurazioni","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell`array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all`array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":460,"y":240,"wires":[["f8fbe8a712bda9dd"]]},{"id":"f8fbe8a712bda9dd","type":"function","z":"ffcf04d739843cc8","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell`array\nif (misurazioni.length >= 2) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":2,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":750,"y":240,"wires":[["68f810c0b9e6c963"],["e0d2781f9f37aec6"]],"info":"### ## # "},{"id":"782ec5001a0c31e9","type":"mqtt out","z":"ffcf04d739843cc8","name":"NO2","topic":"sensors/no2","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1330,"y":280,"wires":[]},{"id":"377b5abe289f8a25","type":"mqtt out","z":"ffcf04d739843cc8","name":"PM10","topic":"sensors/pm10","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1330,"y":200,"wires":[]},{"id":"68f810c0b9e6c963","type":"function","z":"ffcf04d739843cc8","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1090,"y":200,"wires":[["377b5abe289f8a25"]]},{"id":"e0d2781f9f37aec6","type":"function","z":"ffcf04d739843cc8","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1090,"y":280,"wires":[["782ec5001a0c31e9"]]},{"id":"374295b309ce444f","type":"mqtt-broker","name":"HA MQTT","broker":"192.168.31.160","port":"1883","clientid":"","autoConnect":true,"usetls":false,"protocolVersion":"4","keepalive":"60","cleansession":true,"autoUnsubscribe":true,"birthTopic":"","birthQos":"0","birthRetain":"false","birthPayload":"","birthMsg":{},"closeTopic":"","closeQos":"0","closeRetain":"false","closePayload":"","closeMsg":{},"willTopic":"","willQos":"0","willRetain":"false","willPayload":"","willMsg":{},"userProps":"","sessionExpiry":""}]
```

oppure `Flusso Node-RED più centralina.json`:

![nodered2](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/Flusso%20Node-RED%20pi%C3%B9%20centraline.jpg)

codice:
```json
[{"id":"2a235b7b433bbb28","type":"http request","z":"7e71bf58d0d8df1b","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=26&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":250,"y":260,"wires":[["9eab0eaff1ec6956"]]},{"id":"daf9aeb5ce35cda6","type":"inject","z":"7e71bf58d0d8df1b","name":"08.00","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"00 08 * * *","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":270,"y":100,"wires":[["2a235b7b433bbb28","34ab8ce6de39ed9f","17b49fe2099cae18"]]},{"id":"9eab0eaff1ec6956","type":"csv","z":"7e71bf58d0d8df1b","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_---_values":"","x":390,"y":260,"wires":[["bc1b63ee1e8f0f29"]]},{"id":"bc1b63ee1e8f0f29","type":"function","z":"7e71bf58d0d8df1b","name":"Estrae i dati delle misurazioni","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell`array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all`array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":580,"y":260,"wires":[["c179c0eb7be60c8b"]]},{"id":"c179c0eb7be60c8b","type":"function","z":"7e71bf58d0d8df1b","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell`array\nif (misurazioni.length >= 2) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":2,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":870,"y":260,"wires":[["ae96a5ccee3735fd"],["d61ca08685d2e68f"]],"info":"### ## # "},{"id":"61e79e4d18ac53ff","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"NO2","topic":"sensors/no2","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":300,"wires":[]},{"id":"e3b8e52970320c3d","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"PM10","topic":"sensors/pm10","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":220,"wires":[]},{"id":"ae96a5ccee3735fd","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":220,"wires":[["e3b8e52970320c3d"]]},{"id":"640e1cd659bc8bcc","type":"http request","z":"7e71bf58d0d8df1b","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=93&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":250,"y":500,"wires":[["7b24b8a365cbd0b7"]]},{"id":"7b24b8a365cbd0b7","type":"csv","z":"7e71bf58d0d8df1b","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_---_values":"","x":390,"y":500,"wires":[["f7125ef0498ddb37"]]},{"id":"f7125ef0498ddb37","type":"function","z":"7e71bf58d0d8df1b","name":"Estrae i dati delle misurazioni","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell`array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all`array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":580,"y":500,"wires":[["c7de10a4328dec6d"]]},{"id":"c7de10a4328dec6d","type":"function","z":"7e71bf58d0d8df1b","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell`array\nif (misurazioni.length >= 4) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n    let msg3 = { payload: misurazioni[2] }; // Terzo oggetto\n    let msg4 = { payload: misurazioni[3] }; // Quarto oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2, msg3, msg4];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":4,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":870,"y":500,"wires":[[],["58c90af411350d55"],[],["14571d8042e7b752"]],"info":"### ## # "},{"id":"a70c8588afb667ec","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"SO2","topic":"sensors/so2","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":540,"wires":[]},{"id":"08c20a666bf9b9ef","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"PM25","topic":"sensors/pm25","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":460,"wires":[]},{"id":"48b112b1b5f0cc47","type":"http request","z":"7e71bf58d0d8df1b","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=27&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":250,"y":640,"wires":[["5d7c315282a8de56"]]},{"id":"5d7c315282a8de56","type":"csv","z":"7e71bf58d0d8df1b","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_---_values":"","x":390,"y":640,"wires":[["8c601449f5f7080a"]]},{"id":"8c601449f5f7080a","type":"function","z":"7e71bf58d0d8df1b","name":"Estrae i dati delle misurazioni","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell`array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all`array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":580,"y":640,"wires":[["ab1d71e891de1336"]]},{"id":"ab1d71e891de1336","type":"function","z":"7e71bf58d0d8df1b","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell`array\nif (misurazioni.length >= 7) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n    let msg3 = { payload: misurazioni[2] }; // Terzo oggetto\n    let msg4 = { payload: misurazioni[3] }; // Quarto oggetto\n    let msg5 = { payload: misurazioni[4] }; // Quinto oggetto\n    let msg6 = { payload: misurazioni[5] }; // Sesto oggetto\n    let msg7 = { payload: misurazioni[6] }; // settimo oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2, msg3, msg4, msg5, msg6, msg7];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":7,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":870,"y":640,"wires":[[],[],[],["e8030c314ae0b123"],["71fa75be75d2a4fa"],[],["526e230a4fe3aa4d"]],"info":"### ## # "},{"id":"899b55a471331944","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"CO","topic":"sensors/co","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":660,"wires":[]},{"id":"de42a07e56cd4752","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"C6H6","topic":"sensors/c6h6","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":600,"wires":[]},{"id":"4429c59fa49d1159","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"IPA","topic":"sensors/ipa","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":720,"wires":[]},{"id":"34ab8ce6de39ed9f","type":"delay","z":"7e71bf58d0d8df1b","name":"","pauseType":"delay","timeout":"5","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"allowrate":false,"outputs":1,"x":240,"y":440,"wires":[["640e1cd659bc8bcc"]]},{"id":"17b49fe2099cae18","type":"delay","z":"7e71bf58d0d8df1b","name":"","pauseType":"delay","timeout":"10","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"allowrate":false,"outputs":1,"x":240,"y":580,"wires":[["48b112b1b5f0cc47"]]},{"id":"d61ca08685d2e68f","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":300,"wires":[["61e79e4d18ac53ff"]]},{"id":"58c90af411350d55","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":460,"wires":[["08c20a666bf9b9ef"]]},{"id":"14571d8042e7b752","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":540,"wires":[["a70c8588afb667ec"]]},{"id":"e8030c314ae0b123","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":600,"wires":[["de42a07e56cd4752"]]},{"id":"71fa75be75d2a4fa","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":660,"wires":[["899b55a471331944"]]},{"id":"526e230a4fe3aa4d","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":720,"wires":[["4429c59fa49d1159"]]},{"id":"b6920f1155d953e0","type":"comment","z":"7e71bf58d0d8df1b","name":"Dati centralina San Pietro Vernotico","info":"","x":800,"y":140,"wires":[]},{"id":"165037e5d42fa2c9","type":"comment","z":"7e71bf58d0d8df1b","name":"Dati centraline Torchiarolo","info":"","x":810,"y":400,"wires":[]},{"id":"374295b309ce444f","type":"mqtt-broker","name":"HA MQTT","broker":"192.168.31.160","port":"1883","clientid":"","autoConnect":true,"usetls":false,"protocolVersion":"4","keepalive":"60","cleansession":true,"autoUnsubscribe":true,"birthTopic":"","birthQos":"0","birthRetain":"false","birthPayload":"","birthMsg":{},"closeTopic":"","closeQos":"0","closeRetain":"false","closePayload":"","closeMsg":{},"willTopic":"","willQos":"0","willRetain":"false","willPayload":"","willMsg":{},"userProps":"","sessionExpiry":""}]
```
1. Aprite il nodo denominato `http request` e nel campo `URL` copiate ed incollate la stringa ottenuta prima;
1. Riavviate il tutto! Il gioco è fatto!

---

## Personalizzazione ed analisi dei flussi NodeRED

I flussi da me illustrati sono ovviamente meritevoli di sviluppo. Personalmente mi sono occupato di raccogliere i dati esposti dalle centraline vicine alle zone in cui abito ma ciò non toglie che si possano utilizzare tutti i valori a disposizione, ovvero quelli esposti nella tabella "Caratteristiche principali" sopra riportata. Il flusso NodeRED in questo caso sarà il seguente:

![nodered3](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/Flusso%20Node-RED%20centralina%20completa.jpg)

codice:
```json
[{"id":"83ae842a038866d3","type":"tab","label":"DatiARPA1","disabled":false,"info":"","env":[]},{"id":"bccfd7c9f2db270f","type":"csv","z":"83ae842a038866d3","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_---_values":"","x":370,"y":380,"wires":[["2324c1401af40bd2"]]},{"id":"2324c1401af40bd2","type":"function","z":"83ae842a038866d3","name":"Estrae i dati delle misurazioni","func":"// Ottieni l'array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell'array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all'array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":580,"y":380,"wires":[["10c547891c3baf9e"]]},{"id":"10c547891c3baf9e","type":"function","z":"83ae842a038866d3","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l'array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell'array\nif (misurazioni.length >= 9) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n    let msg3 = { payload: misurazioni[2] }; // Terzo oggetto\n    let msg4 = { payload: misurazioni[3] }; // Quarto oggetto\n    let msg5 = { payload: misurazioni[4] }; // Quinto oggetto\n    let msg6 = { payload: misurazioni[5] }; // Sesto oggetto\n    let msg7 = { payload: misurazioni[6] }; // settimo oggetto\n    let msg8 = { payload: misurazioni[7] }; // ottavo oggetto\n    let msg9 = { payload: misurazioni[8] }; // nono oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2, msg3, msg4, msg5, msg6, msg7, msg8, msg9];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":9,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":870,"y":380,"wires":[["c78bf7f00a9df94f"],["a92129cd90dbb91c"],["baacb7b203c98073"],["2019bc966017df5c"],["9f95695360a832b5"],["bd3b68228abce048"],["6a89ae6b2939c5f3"],["919b7562c0f30678"],["82a6fa3f55689262"]],"info":"### ## # "},{"id":"2019bc966017df5c","type":"function","z":"83ae842a038866d3","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1430,"y":320,"wires":[["2144cb4c68a97c98"]]},{"id":"9f95695360a832b5","type":"function","z":"83ae842a038866d3","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1430,"y":380,"wires":[["9eafd411f52e9071"]]},{"id":"82a6fa3f55689262","type":"function","z":"83ae842a038866d3","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1430,"y":620,"wires":[["f671d733e673a541"]]},{"id":"a92129cd90dbb91c","type":"function","z":"83ae842a038866d3","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1430,"y":200,"wires":[["ac820c6ebe3d0e94"]]},{"id":"c78bf7f00a9df94f","type":"function","z":"83ae842a038866d3","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1430,"y":140,"wires":[["5f4b97d3437bf760"]]},{"id":"baacb7b203c98073","type":"function","z":"83ae842a038866d3","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1430,"y":260,"wires":[["a44e6a7982d71cf0"]]},{"id":"bd3b68228abce048","type":"function","z":"83ae842a038866d3","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1430,"y":440,"wires":[["f599221c6e0a7324"]]},{"id":"6a89ae6b2939c5f3","type":"function","z":"83ae842a038866d3","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1430,"y":500,"wires":[["4961370bae9c1cf9"]]},{"id":"919b7562c0f30678","type":"function","z":"83ae842a038866d3","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1430,"y":560,"wires":[["f767e3bfd173b9fb"]]},{"id":"8e1d2d5c56127fd9","type":"http request","z":"83ae842a038866d3","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=26&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":230,"y":380,"wires":[["bccfd7c9f2db270f"]]},{"id":"ce883c9ded520944","type":"inject","z":"83ae842a038866d3","name":"12.00","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"00 12 * * *","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":90,"y":380,"wires":[["8e1d2d5c56127fd9"]]},{"id":"f671d733e673a541","type":"mqtt out","z":"83ae842a038866d3","name":"IPA","topic":"sensors/ipa","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1670,"y":620,"wires":[]},{"id":"f767e3bfd173b9fb","type":"mqtt out","z":"83ae842a038866d3","name":"BLACK CARB","topic":"sensors/black_carb","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1700,"y":560,"wires":[]},{"id":"4961370bae9c1cf9","type":"mqtt out","z":"83ae842a038866d3","name":"H2S","topic":"sensors/hs2","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1670,"y":500,"wires":[]},{"id":"f599221c6e0a7324","type":"mqtt out","z":"83ae842a038866d3","name":"SO2","topic":"sensors/so2","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1670,"y":440,"wires":[]},{"id":"9eafd411f52e9071","type":"mqtt out","z":"83ae842a038866d3","name":"CO","topic":"sensors/co","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1670,"y":380,"wires":[]},{"id":"2144cb4c68a97c98","type":"mqtt out","z":"83ae842a038866d3","name":"C6H6","topic":"sensors/c6h6","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1670,"y":320,"wires":[]},{"id":"a44e6a7982d71cf0","type":"mqtt out","z":"83ae842a038866d3","name":"NO2","topic":"sensors/no2","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1670,"y":260,"wires":[]},{"id":"ac820c6ebe3d0e94","type":"mqtt out","z":"83ae842a038866d3","name":"PM25","topic":"sensors/pm25","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1670,"y":200,"wires":[]},{"id":"5f4b97d3437bf760","type":"mqtt out","z":"83ae842a038866d3","name":"PM10","topic":"sensors/pm10","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1670,"y":140,"wires":[]},{"id":"374295b309ce444f","type":"mqtt-broker","name":"HA MQTT","broker":"192.168.31.160","port":"1883","clientid":"","autoConnect":true,"usetls":false,"protocolVersion":"4","keepalive":"60","cleansession":true,"autoUnsubscribe":true,"birthTopic":"","birthQos":"0","birthRetain":"false","birthPayload":"","birthMsg":{},"closeTopic":"","closeQos":"0","closeRetain":"false","closePayload":"","closeMsg":{},"willTopic":"","willQos":"0","willRetain":"false","willPayload":"","willMsg":{},"userProps":"","sessionExpiry":""}]
```

---

**Commentiamolo:**
--

1. Tramite una chiamata giornaliera alle ore 12.00 lancio l`aggiornamento dei sensori (le rilevazioni sono aggiornate al giorno precedente, quindi non ha senso ripeterla più volte al giorno) tramite il nodo `inject`;
2. Il successivo nodo `http request` è il nodo che invia la stringa per la chiamata dei dati, personalizzabile come detto prima;
3. Il nodo `csv` riceve i dati e li interpreta suddividendoli;
4. Il nodo `function` che segue, denominato `Estrae i dati delle misurazioni`, suddivide l`_array_ ricevuto in stringhe separate producendo più _payload_ per quante sono le righe trasmesse dalla centralina.
     Qui interviene una ulteriore personalizzazione, come faccio a sapere quali inquinanti espone una centralina? La risposta non è complessa:
     1. andare sul sito dei [dati ARPA](https://dati.arpa.puglia.it/openapi/index.html)
     2. scorrere fino alla Sezione `Misurazioni`
     3. cliccare sul successivo tasto `GET`
     4. poi cliccare sulla destra sul tasto `TRY IT OUT`
     5. inserite nel campo `format` il formato dei dati, vi consiglio `csv` per una maggiore intellegibilità
     6. inserite nel campo `id-station` il numero identificativo della centralina che vi interessa, ad esempio `104`
     7. cliccare su `debug-mode` ed impostare a `true` così da avere la risposta a video (non cambia ---a, se lasciate l`impostazione su  `false` vi scaricherà un file di testo con i dati)
     8. quindi cliccare su `Execute`
     9. dopo pochi secondi otterrete la seguente risposta:
        il link per ottenere i dati, nel caso che ci occupa, sarà:


                                 `https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&id_station=104&debugMode=true`

        Nella successiva sezione `Response body` potremo leggere i seguenti dati:
```yaml
         <pre>=== measurements/executeQuery ===
formatParam: CSV
=== measurements/executeQuery ===
<pre>data_di_misurazione,id_station,denominazione,comune,provincia,Longitude,Latitude,tipologia_di_area,tipologia_di_stazione,rete,interesse_rete,id_pollutant,inquinante_misurato,valore_inquinante_misurato,limite,unita_misura,superamenti,indice_qualita,classe_qualita
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","13","PM10",---,---,"µg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","12","PM2.5",---,---,"µg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","4","NO2",---,---,"µg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","2","C6H6",---,---,"µg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","1","CO",---,---,"mg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","6","SO2",---,---,"µg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","8","H2S",---,---,"µg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","10","BLACK CARB",---,---,"ng/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","11","IPA",---,---,"ng/m³",---,---,---
```

   
5. Il nodo `function` che segue, denominato `divide i messaggi per ogni inquinante`, è dotate di tante uscite quanti sono le righe dell`_array_ ricevuto. Quindi se una centralina espone 6 inquinanti, bisognerà modificarlo come segue:
   Nella Scheda `setup` va modificato il numero delle uscite che sarà uguale al numero degli inquinanti esposti;
   Nella Scheda `on message` va inserita una striga `let msg1 = { payload: misurazioni[0] };` per ogni inquinante esposto, così ad esempio:
```yaml
              let msg1 = { payload: misurazioni[0] }; // Primo oggetto
              let msg2 = { payload: misurazioni[1] }; // Secondo oggetto
              let msg3 = { payload: misurazioni[2] }; // Terzo oggetto
              let msg4 = { payload: misurazioni[3] }; // Quarto oggetto
              let msg5 = { payload: misurazioni[4] }; // Quinto oggetto
              let msg6 = { payload: misurazioni[5] }; // Sesto oggetto
              let msg7 = { payload: misurazioni[6] }; // settimo oggetto
              let msg8 = { payload: misurazioni[7] }; // ottavo oggetto
              let msg9 = { payload: misurazioni[8] }; // nono oggetto
```

ovviamente poi modificheremo anche il successivo messaggio di  `return` del _payload_ ottenendo quindi:

```yaml 
          return [msg1, msg2, msg3, msg4, msg5, msg6, msg7, msg8, msg9];` 
```

in questo modo avremo una uscita per ogni inquinante a cui collegheremo, nello stesso ordine mostrato dall`API, il canale MQTT tramite il nodo `mqtt out`. I nodi, per maggiore chiarezza e celerità, li ho denominati così come previsto dalla sigla breve utilizzata nella stessa API regionale.
6. Prima di arrivare alla pubblicazione del dato in `MQTT` ho dovuto inserire un nuovo nodo `function` denominato `formatta in valore "valore" in caso di errore` che ha lo scopo di sostituire i dati non pervenuti, esposti dalle API con la dicitura `---`, con il numero `0`. Questo si è reso necessario perchè altrimenti Home Assistant non è in grado di interpetrare il dato restituendo così un errore generale del sensore che non esporrà ---a.
7. L`ultima cosa è quindi la configurazione dei relativi sensori in Home Assistant, la struttura è identica per tutti, va cambiato solo il nome del sensore nel campo `name` ed il canale mqtt nel campo `json_attributes_topic:`, ---`altro:

 ```yaml

#_configuration.yaml_

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
```

8. Lavoro finito e buon divertimento a tutti!



---
# ARPA Puglia environmental pollutant data acquisition in Home Assistant


A [Home Assistant](https://home-assistant.io/) utility that helps you visualize the data of the main environmental indicators published by ARPA Puglia via [Node-RED](https://nodered.org/).

<br>

## Table of Contents

1. **[Project Description](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#project-description)**

2. **[Key Features](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#key-features)**

3. **[Operation Description](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#description-of-operation)**

4. **[Installation](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#installation)**
   1. **[Prerequisites](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#1---prerequisites)**
   1. **[Configuring the Data to Be Captured](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#2---configuring-the-data-to-be-captured)**
   1. **[Home Assistant Setup](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#3---home-assistant-setup)**
   2. **[Configuring Node-RED](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#4---configuring-node-red)**

5. **[Customizing NodeRed Flows](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#customization-and-analysis-of-nodered-flows)**

6. **[Code Comment](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant#lets-comment-on-it)**  
 

 
<br>

## Project description

The main purpose of these lines is to automate the daily acquisition of environmental data published via API by the Regional Environmental Protection and Prevention Agency for Puglia, their processing via Node-RED and the subsequent integration into Home Assistant via MQTT publication for real-time visualization.

All info is available on the website of [ARPA Puglia](https://www.arpa.puglia.it/pagina2795_aria.html) according to [map](https://dati.arpa.puglia.it/qaria?footer=false%EF%BB%BF)

---

## Key features

- **API Integration**: Direct connection to the ARPA Puglia API for the collection of the following environmental parameters:
     1. **PM10** (Inhalable dust, A set of solid and liquid substances with a diameter of less than 10 microns. They derive from vehicle emissions, industrial processes, natural phenomena)
     1. **PM2.5** (Respirable dust, a set of solid and liquid substances with a diameter of less than 2.5 microns. They derive from industrial processes, combustion processes, vehicle emissions, natural phenomena)
     3. **NO2** (Nitrogen dioxide, Toxic gas that is formed in high-temperature combustion. Its main sources are internal combustion engines, thermal plants, thermoelectric power plants)
     4. **SO2** (Sulphur dioxide, irritant gas, is formed mainly following the use of fuels (coal, oil, diesel) containing sulphur impurities)
     5. **CO** (Carbon Monoxide, Gaseous substance, is formed by incomplete combustion of organic material, for example in car engines and industrial processes)
     6. **C6H6** (Benzene, Volatile and sweet-smelling liquid. It derives from the incomplete combustion of coal and oil, from the exhaust gases of motor vehicles, from tobacco smoke)
     7. **PAHs** (Polycyclic Aromatic Hydrocarbons, Class of semi-volatile organic compounds, with 2 or more benzene rings condensed together, generated by the incomplete combustion of organic material. The data provided refers to PAHs adsorbed on carbonaceous particles with an aerodynamic diameter between 0.01 and 1 micron)
     8. **H2S** (Hydrogen sulphide, Colourless gas with the characteristic smell of rotten eggs, characterised by a low olfactory threshold.  It is generated in the production of coal coke, in the processing of oil, fertilizers, waste and other industrial processes)
     9. **O3** (Ozone, Substance not emitted directly into the atmosphere, is formed by reaction between other pollutants, mainly NO2 and hydrocarbons, in the presence of solar radiation)
     10. **BLACK CARB** (Black Carbon, Product of the incomplete combustion of fossil fuels and biomass; can be emitted from natural and anthropogenic sources in the form of soot)
     11. **Air quality**

**Disclaimer**: Not all control units expose all the data, therefore, I decided to publish two node-red flows to be brought and then integrated with any data retrieved directly from the ARPA site to access their API: 

[API_Regione_Puglia](https://dati.arpa.puglia.it/openapi/index.html)

- **Node-RED Workflow**: Flow configured to query APIs daily, process data (formatting, validation) and send it to an MQTT broker.
- **MQTT communication**: Secure data transmission to Home Assistant via MQTT protocol, with dedicated topics for each sensor.
- Custom Sensor in Home Assistant: Configuration of custom sensors in HA to receive data via MQTT and display them in dashboards.
- **Daily Automation**: Schedule updates to ensure data is always up-to-date without manual intervention.

---

## Description of Operation

1. **Data Acquisition**:  
   Node-RED makes periodic API calls to ARPA Puglia and filters the relevant data.

2. **Processing**:  
   The data is converted into compatible MQTT (es. JSON structured payloads).

3. **Sending to Home Assistant**:  
   Payloads are published to an MQTT broker (e.g., Mosquitto) and received by HA via MQTT listeners.

4. **Sensor Creation**:  
   YAML scripts in HA define sensors that map MQTT data to usable entities (e.g. `sensor.temperatura_arpapuglia`).

5. **Display**:  
   Integration into dashboards for centralized environmental monitoring.

---

## Installation

### 1 - Prerequisites

- Node-RED installed and configured with nodes:
  - `node-red-contrib-mqtt`
  - `node-red-contrib-http-request`
- MQTT broker (e.g. Mosquitto) running and integrated with Home Assistant.
- Custom `flex-table-card` downloadable from HACS installed and configured (If you want to use the table view like I did).


### 2 - Configuring the data to be captured

1. As already mentioned, the data are acquired through the public APIs made available by ARPA. The available control units (as of 29.1.2025) are as follows:

|id_station|             Name |                       municipality | address |network |interesse_rete|state |   Country |paese_esteso |  Latitude |Longitude |    Source |
| -------- | -------------------------------------------------------- | ------------------------------- | ----------------------- | ----- | ------------ | ----------- | --------- | ------------ | ------------ | ------------ | ------------ |
|8|Monte S. Angelo - Ciuffreda|Monte Sant Angelo|Suolo Ciuffreda|RRQA|PUBBLICO|Foggia|IT|Italy|15.945254|41.666109|ARPAP
|10|Manfredonia - Mandorli|Manfredonia|Via dei Mandorli|RRQA|PUBBLICO|Foggia|IT|Italy|15.909638|41.629333|ARPAP
|14|Molfetta - Verdi|Molfetta|P.zza Verdi|RRQA|PUBBLICO|Bari|IT|Italy|16.605253|41.201101|ARPAP
|17|Bari - Caldarola|Bari|Via Caldarola|RRQA|PUBBLICO|Bari|IT|Italy|16.888064|41.113542|ARPAP
|18|Statte - Sorgenti|Statte|Via Delle Sorgenti|RRQA|PUBBLICO|Taranto|IT|Italy|17.203325|40.562499|ARPAP
|19|Taranto - Archimede|Taranto|Via Archimede|RRQA|PUBBLICO|Taranto|IT|Italy|17.233048|40.494441|ARPAP
|20|Taranto - Machiavelli|Taranto|Via Machiavelli|RRQA|PUBBLICO|Taranto|IT|Italy|17.225823|40.488608|ARPAP
|21|Taranto - San Vito|Taranto|presso Colonia Marina|RRQA|PUBBLICO|Taranto|IT|Italy|17.225272|40.423328|ARPAP
|22|Taranto - Alto Adige|Taranto|Via Alto Adige| presso scuola XX Circolo|RRQA|PUBBLICO|Taranto|IT|Italy|17.263602|40.460553|ARPAP
|23|Mesagne - Via Udine|Mesagne|Via Udine|RRQA|PUBBLICO|Brindisi|IT|Italy|17.807998|40.565995|ARPAP
|24|Brindisi - Via Taranto|Brindisi|Via Taranto|RRQA|PUBBLICO|Brindisi|IT|Italy|17.947777|40.634166|ARPAP
|25|San Pancrazio|San Pancrazio Salentino|---|RRQA|PUBBLICO|Brindisi|IT|Italy|17.845999|40.422993|ARPAP
|26|San Pietro Vernotico|San Pietro Vernotico|---|RRQA|PUBBLICO|Brindisi|IT|Italy|18.005994|40.485998|ARPAP
|27|Torchiarolo - Don Minzoni|Torchiarolo|P.za Don Minzoni|RRQA|PUBBLICO|Brindisi|IT|Italy|18.053991|40.487999|ARPAP
|28|Guagnano - Villa Baldassarri|Guagnano|Villa Baldassarri|RRQA|PUBBLICO|Lecce|IT|Italy|17.964477|40.418525|ARPAP
|29|Lecce - Cerrate|Lecce|S. Maria Cerrate|RRQA|PUBBLICO|Lecce|IT|Italy|18.116386|40.459696|ARPAP
|31|Arnesano - Riesci|Arnesano|Zona Riesci|RRQA|PUBBLICO|Lecce|IT|Italy|18.095074|40.346270|ARPAP
|33|Brindisi - Via dei Mille|Brindisi|Via dei Mille|RRQA|PUBBLICO|Brindisi|IT|Italy|17.938152|40.638748|ARPAP
|35|Brindisi - Casale|Brindisi|Via San Giusto|RRQA|PUBBLICO|Brindisi|IT|Italy|17.942569|40.650083|ARPAP
|36|Brindisi - SISRI|Brindisi|Via Curie|RRQA|PUBBLICO|Brindisi|IT|Italy|17.975041|40.624738|ARPAP
|37|Taranto - CISI|Taranto|q.re Paolo VI - presso CISI Puglia|RRQA|PUBBLICO|Taranto|IT|Italy|17.253416|40.520934|ARPAP
|38|Taranto - Talsano|Taranto|Talsano - presso scuola U.Foscolo|RRQA|PUBBLICO|Taranto|IT|Italy|17.283879|40.411943|ARPAP
|40|Statte - Wind|Statte|ss.7 presso il ponte radio wind|RRQA|PUBBLICO|Taranto|IT|Italy|17.173611|40.526111|ARPAP
|41|Grottaglie|Grottaglie|via XXV luglio|RRQA|PUBBLICO|Taranto|IT|Italy|17.423879|40.537776|ARPAP
|42|Martina Franca|Martina Franca|Via della Stazione|RRQA|PUBBLICO|Taranto|IT|Italy|17.331939|40.700826|ARPAP
|47|Lecce - Garigliano|Lecce|---|RRQA|PUBBLICO|Lecce|IT|Italy|18.174327|40.364457|ARPAP
|49|Maglie|Maglie|Via Don. L. Sturzo| 4|IL|PUBBLICO|Lecce|IT|Italy|18.294098|40.123636|ARPAP
|50|Galatina - I.T.C.  La Porta |Galatina|Viale degli studenti|RRQA|PUBBLICO|Lecce|IT|Italy|18.174725|40.166947|ARPAP
|51|Campi Salentina|Campi Salentina|Via Napoli|RRQA|PUBBLICO|Lecce|IT|Italy|18.026509|40.397507|ARPAP
|54|Bari - Cavour|Bari|Corso Cavour|RRQA|PUBBLICO|Bari|IT|Italy|16.872552|41.122269|ARPAP
|55|Bari - Kennedy|Bari|Piazza R. Kennedy|RRQA|PUBBLICO|Bari|IT|Italy|16.858905|41.099593|ARPAP
|62|Lecce - Libertini|Lecce|Piazza Libertini| Lecce|RRQA|PUBBLICO|Lecce|IT|Italy|18.176667|40.351945|ARPAP
|63|Andria - Vaccina|Andria|Via Vaccina|RRQA|PUBBLICO|BAT|IT|Italy|16.303939|41.233278|ARPAP
|64|Altamura - Via Santeramo|Altamura|Via Golgota| Altamura|RRQA|PUBBLICO|Bari|IT|Italy|16.561015|40.828848|ARPAP
|65|Casamassima - LaPenna|Casamassima|Via Lapenna|RRQA|PUBBLICO|Bari|IT|Italy|16.920731|40.953154|ARPAP
|66|Monopoli - Aldo Moro|Monopoli|Viale Aldo Moro|RRQA|PUBBLICO|Bari|IT|Italy|17.290285|40.951167|ARPAP
|67|Brindisi - Terminal Passeggeri|Brindisi|presso Terninal Passeggeri sulla banchina portuale di Costa Morena|RRQA|PUBBLICO|Brindisi|IT|Italy|17.961687|40.647432|ARPAP
|71|Barletta - Casardi|Barletta|via Casardi|RRQA|PUBBLICO|BAT|IT|Italy|16.286111|41.316666|ARPAP
|72|Francavilla Fontana|Francavilla Fontana|Via Fabio Filzi|RRQA|PUBBLICO|Brindisi|IT|Italy|17.588331|40.529163|ARPAP
|75|Massafra|Massafra|Via Frappietri|RRQA|PUBBLICO|Taranto|IT|Italy|17.116690|40.593761|ARPAP
|76|Foggia - Rosati|Foggia|Via Giuseppe Rosati| 139|RRQA|PUBBLICO|Foggia|IT|Italy|15.548611|41.455555|ARPAP
|77|Brindisi - Perrino|Brindisi|Via Crati|RRQA|PUBBLICO|Brindisi|IT|Italy|17.954778|40.631361|ARPAP
|78|Brindisi - Cappuccini|Brindisi|Via Cappuccini|IL|PUBBLICO|Brindisi|IT|Italy|17.921778|40.630917|ARPAP
|81|Bari - Carbonara|Bari|via Loguercio|RRQA|PUBBLICO|Bari|IT|Italy|16.866944|41.076666|ARPAP
|85|San Severo - Azienda Russo|San Severo|Localita` Palmori|RRQA|PUBBLICO|Foggia|IT|Italy|15.440833|41.546666|ARPAP
|90|Bari - CUS|Bari|Lungomare Starita presso CUS BARI|RRQA|PUBBLICO|Bari|IT|Italy|16.845277|41.134722|ARPAP
|92|Monopoli - Liceo Artistico Russo|Monopoli|Via Cosimo Pisonio|RRQA|PUBBLICO|Bari|IT|Italy|17.284267|40.961578|ARPAP
|93|Torchiarolo - Fanin     |Torchiarolo|Via FANIN SN - 72020 TORCHIAROLO      |RRQA|PUBBLICO|Brindisi|IT|Italy|18.047226|40.489447|ARPAP
|94|Torchiarolo-Lendinuso|Torchiarolo|C.da MONTEVACCARO SN - 72020 TORCHIAROLO      |IL|PUBBLICO|Brindisi|IT|Italy|18.078880|40.517502|ARPAP
|95|Surbo - Croce |Surbo|Via B. Croce  S.N. - 73010 SURBO    |RRQA|PUBBLICO|Lecce|IT|Italy|18.120835|40.411940|ARPAP
|96|Ceglie Messapica|Ceglie Messapica|via Martina  (Scuola Elementare Papa Giovanni XXIII )|RRQA|PUBBLICO|Brindisi|IT|Italy|17.512500|40.649166|ARPAP
|97|Modugno - EN02|Modugno|Viale delle Magnolie| 6|RRQA|PUBBLICO|Bari|IT|Italy|16.766111|41.107777|ARPAP
|98|Modugno - EN03|Modugno|Via Maranda|RRQA|PUBBLICO|Bari|IT|Italy|16.781672|41.087222|ARPAP
|99|Modugno - EN04|Modugno|Via Ancona|RRQA|PUBBLICO|Bari|IT|Italy|16.787777|41.114444|ARPAP
|100|Cokeria|Taranto|---|ADI|PRIVATO|ILVA|IT|Italy|17.217630|40.502050|ARPAP
|101|Tamburi - Via Orsini|Taranto|---|ADI|PRIVATO|ILVA|IT|Italy|17.225830|40.494510|ARPAP
|102|Riv1|Taranto|---|ADI|PRIVATO|ILVA|IT|Italy|17.216900|40.518570|ARPAP
|103|Direzione|Taranto|---|ADI|PRIVATO|ILVA|IT|Italy|17.200550|40.501980|ARPAP
|104|Meteo Parchi|Taranto|---|ADI|PRIVATO|ILVA|IT|Italy|17.222180|40.496730|ARPAP
|105|Portineria C|Taranto|---|ADI|PRIVATO|ILVA|IT|Italy|17.186560|40.518900|ARPAP
|106|Bitonto - EN01|Bitonto|Agro Di Bitonto|IL|PRIVATO|Bari|IT|Italy|16.745277|41.079166|ARPAP
|107|Palo del Colle - EN05|Palo del Colle|Via Ungaretti|IL|PRIVATO|Bari|IT|Italy|16.700830|41.061388|ARPAP
|108|Cisternino|Cisternino|via Benedetto Croce|RRQA|PUBBLICO|Brindisi|IT|Italy|17.415833|40.742777|ARPAP
|109|Barletta - Mezzo Mobile Via Trani|Barletta|---|IL|PRIVATO|BAT|IT|Italy|16.293055|41.318333|ARPAP
|110|Candela - Scuola|Candela|---|IL|PRIVATO|Foggia|IT|Italy|15.519444|41.133055|ARPAP
|111|Candela - Ex Comes|Candela|---|IL|PRIVATO|Foggia|IT|Italy|15.523055|41.173055|ARPAP
|112|San Severo  - Municipio|San Severo|---|RRQA|PUBBLICO|Foggia|IT|Italy|15.379722|41.696944|ARPAP
|113|Galatina-Colacem|Galatina|Contrada Piani|IL|PUBBLICO|Lecce|IT|Italy|18.192856|40.163964|ARPAP
|114|C1 - Lato sud ovest impianti| Area imprese|Brindisi|Zona industriale| Via Leonardo da Vinci|VERSALIS|PRIVATO|Brindisi|IT|Italy|17.990417|40.628028|ARPAP
|115|C2 - Area bacino idrico|Brindisi|Zona industriale| Via Giuseppe Verdi|VERSALIS|PRIVATO|Brindisi|IT|Italy|17.988500|40.638889|ARPAP
|116|C3 - Area impianto P1CR Cracking|Brindisi|Zona industriale| Via Aldo Moro|VERSALIS|PRIVATO|Brindisi|IT|Italy|17.998139|40.638139|ARPAP
|117|C4 - Area impianto Butadiene|Brindisi|Zona industriale| Via Enrico Fermi|VERSALIS|PRIVATO|Brindisi|IT|Italy|18.002444|40.634444|ARPAP
|118|C5 - Area Pontile|Brindisi|Zona industriale| Via Galileo Galilei|VERSALIS|PRIVATO|Brindisi|IT|Italy|17.989981|40.645450|ARPAP
|119|C6 - Area PE1/2 lato sud impianti|Brindisi|Zona industriale| Via Alessandro Volta|VERSALIS|PRIVATO|Brindisi|IT|Italy|18.001614|40.623753|ARPAP


1. The identification of the `id_station` is of particular importance because it will be, in fact, the only data to be modified in the data reading string.
1. The read string is as follows:

   `https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=26&debugMode=false`

   can also be used in the normal browser. Its consultation will generate the download of a file that you can read and appreciate, especially because for the control unit that you have identified, it will allow you to know what data are published by ARPA.

1. Replace the `id_station` with that of the desired station:

      `https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&`**id_station=26**`&debugMode=false`

1. Test the string and if the answer is right (it will be easy to read) save it to insert it in the right NodeRED node. 


    
### 3 - Home Assistant Setup

1. Download all files;
1. Copy the contents of `HA.txt sensors` to `configuration.yaml`. If the `mqtt:` item is already present, append the sensors to those already present;
1. You can add other sensors if the control units you have chosen show others, the configuration is identical for all of them;
1. The following configuration will expose the sensors in HA as you see below:

![lovelace](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/Esposizione%20HA.jpg)



code:
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


1. In the _Dashboard_ of the _lovelace_, where you prefer, open a new tab and paste the code of the file `HA lovelace.txt` and the result will be this:

 ![lovelace](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/Lovelace%20Visualizzazione%20HA.jpg)

code:
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

### 4 - Configuring Node-RED

1. Copy and paste the contents of the `Single centralina.json Node-RED Flow` or `Multiple centralina.json Node-RED Flow` file into a NodeRED sheet:

`Single Node-RED Flow centralina.json`:

 ![nodered1](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/Flusso%20Node-RED.jpg)


code:
```json
[{"id":"0fc96737abf2fa25","type":"http request","z":"ffcf04d739843cc8","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=26&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":130,"y":240,"wires":[["45d325f5c135d59b"]]},{"id":"be3859fca160266f","type":"inject","z":"ffcf04d739843cc8","name":"08.00","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"00 08 * * *","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":150,"y":80,"wires":[["0fc96737abf2fa25","7a2723b5493def9c","8d78bf9f78453e8c"]]},{"id":"45d325f5c135d59b","type":"csv","z":"ffcf04d739843cc8","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_---_values":"","x":270,"y":240,"wires":[["1dc499b20f7cf18e"]]},{"id":"1dc499b20f7cf18e","type":"function","z":"ffcf04d739843cc8","name":"Estrae i dati delle misurazioni","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell`array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all`array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":460,"y":240,"wires":[["f8fbe8a712bda9dd"]]},{"id":"f8fbe8a712bda9dd","type":"function","z":"ffcf04d739843cc8","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell`array\nif (misurazioni.length >= 2) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":2,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":750,"y":240,"wires":[["68f810c0b9e6c963"],["e0d2781f9f37aec6"]],"info":"### ## # "},{"id":"782ec5001a0c31e9","type":"mqtt out","z":"ffcf04d739843cc8","name":"NO2","topic":"sensors/no2","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1330,"y":280,"wires":[]},{"id":"377b5abe289f8a25","type":"mqtt out","z":"ffcf04d739843cc8","name":"PM10","topic":"sensors/pm10","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1330,"y":200,"wires":[]},{"id":"68f810c0b9e6c963","type":"function","z":"ffcf04d739843cc8","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1090,"y":200,"wires":[["377b5abe289f8a25"]]},{"id":"e0d2781f9f37aec6","type":"function","z":"ffcf04d739843cc8","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1090,"y":280,"wires":[["782ec5001a0c31e9"]]},{"id":"374295b309ce444f","type":"mqtt-broker","name":"HA MQTT","broker":"192.168.31.160","port":"1883","clientid":"","autoConnect":true,"usetls":false,"protocolVersion":"4","keepalive":"60","cleansession":true,"autoUnsubscribe":true,"birthTopic":"","birthQos":"0","birthRetain":"false","birthPayload":"","birthMsg":{},"closeTopic":"","closeQos":"0","closeRetain":"false","closePayload":"","closeMsg":{},"willTopic":"","willQos":"0","willRetain":"false","willPayload":"","willMsg":{},"userProps":"","sessionExpiry":""}]
```

or `Flusso Node-RED più centralina.json`:

![nodered2](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/Flusso%20Node-RED%20pi%C3%B9%20centraline.jpg)

code:
```json
[{"id":"2a235b7b433bbb28","type":"http request","z":"7e71bf58d0d8df1b","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=26&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":250,"y":260,"wires":[["9eab0eaff1ec6956"]]},{"id":"daf9aeb5ce35cda6","type":"inject","z":"7e71bf58d0d8df1b","name":"08.00","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"00 08 * * *","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":270,"y":100,"wires":[["2a235b7b433bbb28","34ab8ce6de39ed9f","17b49fe2099cae18"]]},{"id":"9eab0eaff1ec6956","type":"csv","z":"7e71bf58d0d8df1b","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_---_values":"","x":390,"y":260,"wires":[["bc1b63ee1e8f0f29"]]},{"id":"bc1b63ee1e8f0f29","type":"function","z":"7e71bf58d0d8df1b","name":"Estrae i dati delle misurazioni","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell`array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all`array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":580,"y":260,"wires":[["c179c0eb7be60c8b"]]},{"id":"c179c0eb7be60c8b","type":"function","z":"7e71bf58d0d8df1b","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell`array\nif (misurazioni.length >= 2) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":2,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":870,"y":260,"wires":[["ae96a5ccee3735fd"],["d61ca08685d2e68f"]],"info":"### ## # "},{"id":"61e79e4d18ac53ff","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"NO2","topic":"sensors/no2","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":300,"wires":[]},{"id":"e3b8e52970320c3d","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"PM10","topic":"sensors/pm10","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":220,"wires":[]},{"id":"ae96a5ccee3735fd","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":220,"wires":[["e3b8e52970320c3d"]]},{"id":"640e1cd659bc8bcc","type":"http request","z":"7e71bf58d0d8df1b","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=93&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":250,"y":500,"wires":[["7b24b8a365cbd0b7"]]},{"id":"7b24b8a365cbd0b7","type":"csv","z":"7e71bf58d0d8df1b","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_---_values":"","x":390,"y":500,"wires":[["f7125ef0498ddb37"]]},{"id":"f7125ef0498ddb37","type":"function","z":"7e71bf58d0d8df1b","name":"Estrae i dati delle misurazioni","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell`array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all`array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":580,"y":500,"wires":[["c7de10a4328dec6d"]]},{"id":"c7de10a4328dec6d","type":"function","z":"7e71bf58d0d8df1b","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell`array\nif (misurazioni.length >= 4) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n    let msg3 = { payload: misurazioni[2] }; // Terzo oggetto\n    let msg4 = { payload: misurazioni[3] }; // Quarto oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2, msg3, msg4];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":4,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":870,"y":500,"wires":[[],["58c90af411350d55"],[],["14571d8042e7b752"]],"info":"### ## # "},{"id":"a70c8588afb667ec","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"SO2","topic":"sensors/so2","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":540,"wires":[]},{"id":"08c20a666bf9b9ef","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"PM25","topic":"sensors/pm25","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":460,"wires":[]},{"id":"48b112b1b5f0cc47","type":"http request","z":"7e71bf58d0d8df1b","name":"","method":"GET","ret":"txt","paytoqs":"ignore","url":"https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&network=RRQA&id_station=27&debugMode=false","tls":"","persist":false,"proxy":"","insecureHTTPParser":false,"authType":"","senderr":false,"headers":[],"x":250,"y":640,"wires":[["5d7c315282a8de56"]]},{"id":"5d7c315282a8de56","type":"csv","z":"7e71bf58d0d8df1b","name":"","spec":"rfc","sep":",","hdrin":true,"hdrout":"none","multi":"mult","ret":"\\r\\n","temp":"","skip":"0","strings":true,"include_empty_strings":"","include_---_values":"","x":390,"y":640,"wires":[["8c601449f5f7080a"]]},{"id":"8c601449f5f7080a","type":"function","z":"7e71bf58d0d8df1b","name":"Estrae i dati delle misurazioni","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Crea un array per memorizzare i risultati\nlet risultati = [];\n\n// Itera su ogni oggetto nell`array\nmisurazioni.forEach((misurazione, index) => {\n    // Estrai i campi che ti interessano\n    let risultato = {\n        data: misurazione.data_di_misurazione,\n        inquinante: misurazione.inquinante_misurato,\n        valore: misurazione.valore_inquinante_misurato,\n        limite: misurazione.limite,\n        unita: misurazione.unita_misura,\n        indice_qualita: misurazione.indice_qualita,\n        classe_qualita: misurazione.classe_qualita,\n        superamenti:misurazione.superamenti\n    };\n\n    // Aggiungi il risultato all`array\n    risultati.push(risultato);\n});\n\n// Imposta il nuovo payload con i risultati\nmsg.payload = risultati;\n\n// Restituisci il messaggio\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":580,"y":640,"wires":[["ab1d71e891de1336"]]},{"id":"ab1d71e891de1336","type":"function","z":"7e71bf58d0d8df1b","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell`array\nif (misurazioni.length >= 7) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n    let msg3 = { payload: misurazioni[2] }; // Terzo oggetto\n    let msg4 = { payload: misurazioni[3] }; // Quarto oggetto\n    let msg5 = { payload: misurazioni[4] }; // Quinto oggetto\n    let msg6 = { payload: misurazioni[5] }; // Sesto oggetto\n    let msg7 = { payload: misurazioni[6] }; // settimo oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2, msg3, msg4, msg5, msg6, msg7];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":7,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":870,"y":640,"wires":[[],[],[],["e8030c314ae0b123"],["71fa75be75d2a4fa"],[],["526e230a4fe3aa4d"]],"info":"### ## # "},{"id":"899b55a471331944","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"CO","topic":"sensors/co","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":660,"wires":[]},{"id":"de42a07e56cd4752","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"C6H6","topic":"sensors/c6h6","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":600,"wires":[]},{"id":"4429c59fa49d1159","type":"mqtt out","z":"7e71bf58d0d8df1b","name":"IPA","topic":"sensors/ipa","qos":"","retain":"","respTopic":"","contentType":"","userProps":"","correl":"","expiry":"","broker":"374295b309ce444f","x":1450,"y":720,"wires":[]},{"id":"34ab8ce6de39ed9f","type":"delay","z":"7e71bf58d0d8df1b","name":"","pauseType":"delay","timeout":"5","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"allowrate":false,"outputs":1,"x":240,"y":440,"wires":[["640e1cd659bc8bcc"]]},{"id":"17b49fe2099cae18","type":"delay","z":"7e71bf58d0d8df1b","name":"","pauseType":"delay","timeout":"10","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"allowrate":false,"outputs":1,"x":240,"y":580,"wires":[["48b112b1b5f0cc47"]]},{"id":"d61ca08685d2e68f","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":300,"wires":[["61e79e4d18ac53ff"]]},{"id":"58c90af411350d55","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":460,"wires":[["08c20a666bf9b9ef"]]},{"id":"14571d8042e7b752","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":540,"wires":[["a70c8588afb667ec"]]},{"id":"e8030c314ae0b123","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":600,"wires":[["de42a07e56cd4752"]]},{"id":"71fa75be75d2a4fa","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":660,"wires":[["899b55a471331944"]]},{"id":"526e230a4fe3aa4d","type":"function","z":"7e71bf58d0d8df1b","name":"formatta in valore \"valore\" in caso di errore","func":"\n    if (msg.payload.valore === \"---\") \n{\n    msg.payload.valore = \"0\";\n}\n\nif (msg.payload.indice_qualita === \"---\") \n{\n    msg.payload.indice_qualita = \"0\";\n}\n\nif (msg.payload.classe_qualita === \"---\") \n{\n    msg.payload.classe_qualita = \"0\";\n}\n\nif (msg.payload.superamenti === \"---\") \n{\n    msg.payload.superamenti = \"0\";\n}\n\nif (msg.payload.limite === \"---\") \n{\n    msg.payload.limite = \"0\";\n}\nreturn msg;","outputs":1,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":1210,"y":720,"wires":[["4429c59fa49d1159"]]},{"id":"b6920f1155d953e0","type":"comment","z":"7e71bf58d0d8df1b","name":"Dati centralina San Pietro Vernotico","info":"","x":800,"y":140,"wires":[]},{"id":"165037e5d42fa2c9","type":"comment","z":"7e71bf58d0d8df1b","name":"Dati centraline Torchiarolo","info":"","x":810,"y":400,"wires":[]},{"id":"374295b309ce444f","type":"mqtt-broker","name":"HA MQTT","broker":"192.168.31.160","port":"1883","clientid":"","autoConnect":true,"usetls":false,"protocolVersion":"4","keepalive":"60","cleansession":true,"autoUnsubscribe":true,"birthTopic":"","birthQos":"0","birthRetain":"false","birthPayload":"","birthMsg":{},"closeTopic":"","closeQos":"0","closeRetain":"false","closePayload":"","closeMsg":{},"willTopic":"","willQos":"0","willRetain":"false","willPayload":"","willMsg":{},"userProps":"","sessionExpiry":""}]
```


1. Open the node called `http request` and in the `URL` field copy and paste the string obtained earlier;
1. Restart everything! That`s it!

---

## Customization and analysis of NodeRED flows

The flows I have illustrated are obviously worthy of development. Personally, I have been responsible for collecting the data displayed by the control units near the areas where I live, but this does not mean that all the available values can be used, i.e. those shown in the "Main characteristics" table above. The NodeRED flow in this case will be as follows:

![nodered3](https://github.com/kapkirk/Dati-ambientali-ARPA-Puglia-via-Home-Assistant/blob/main/images/Flusso%20Node-RED%20centralina%20completa.jpg)

code:
```json
[{"id":"10c547891c3baf9e","type":"function","z":"83ae842a038866d3","name":"divide i messaggi per ogni inquinante","func":"// Ottieni l`array dal payload\nlet misurazioni = msg.payload;\n\n// Verifica che ci siano almeno due elementi nell`array\nif (misurazioni.length >= 7) {\n    // Crea due messaggi separati\n    let msg1 = { payload: misurazioni[0] }; // Primo oggetto\n    let msg2 = { payload: misurazioni[1] }; // Secondo oggetto\n    let msg3 = { payload: misurazioni[2] }; // Terzo oggetto\n    let msg4 = { payload: misurazioni[3] }; // Quarto oggetto\n    let msg5 = { payload: misurazioni[4] }; // Quinto oggetto\n    let msg6 = { payload: misurazioni[5] }; // Sesto oggetto\n    let msg7 = { payload: misurazioni[6] }; // settimo oggetto\n    let msg8 = { payload: misurazioni[7] }; // ottavo oggetto\n    let msg9 = { payload: misurazioni[8] }; // nono oggetto\n\n    // Restituisci entrambi i messaggi\n    return [msg1, msg2, msg3, msg4, msg5, msg6, msg7, msg8, msg9];\n} else {\n    // Se non ci sono abbastanza elementi, restituisci un messaggio di errore\n    msg.payload = \"Errore: Il payload non contiene abbastanza elementi.\";\n    return msg;\n}","outputs":9,"timeout":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":870,"y":380,"wires":[["c78bf7f00a9df94f"],["a92129cd90dbb91c"],["baacb7b203c98073"],["2019bc966017df5c"],["9f95695360a832b5"],["bd3b68228abce048"],["6a89ae6b2939c5f3"],["919b7562c0f30678"],["82a6fa3f55689262"]],"info":"### ## # "}]
```

---

**Let`s comment on it:**
--

1. Through a daily call at 12.00 I launch the update of the sensors (the readings are updated to the previous day, so it makes no sense to repeat it several times a day) through the `inject` node;
2. The next `http request` node is the node that sends the string for the data call, customizable as mentioned before;
3. The `csv` node receives the data and interprets them by dividing them;
4. The `function` node that follows, called `Extracts measurement data`, splits the received _array_ into separate strings producing more _payload_ for as many rows as there are transmitted by the control unit.
     This is where further customization comes in, how do I know which pollutants a control unit exposes? The answer is not complex:
     1. go to the [ARPA data](https://dati.arpa.puglia.it/openapi/index.html) website
     2. scroll down to the `Measurements` section
     3. click on the next `GET` button
     4. then click on the `TRY IT OUT` button on the right
     5. enter the format of the data in the `format` field, I recommend `csv` for greater intelligibility
     6. Enter the identification number of the control unit you are interested in in the `Id-Station` field, e.g. `104`
     7. click on `debug-mode` and set to `true` so that you get the answer on the screen (nothing changes, if you leave the setting `false` it will download a text file with the data)
     8. then click `Execute`
     9. After a few seconds you will get the following answer:
        The link to obtain the data, in the case we are dealing with, will be:

                                https://dati.arpa.puglia.it/api/v1/measurements?language=ITA&format=CSV&id_station=104&debugMode=true


      In the following `Response body` section we can read the following data:
```yaml
         <pre>=== measurements/executeQuery ===
formatParam: CSV
=== measurements/executeQuery ===
<pre>data_di_misurazione,id_station,denominazione,comune,provincia,Longitude,Latitude,tipologia_di_area,tipologia_di_stazione,rete,interesse_rete,id_pollutant,inquinante_misurato,valore_inquinante_misurato,limite,unita_misura,superamenti,indice_qualita,classe_qualita
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","13","PM10",---,---,"µg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","12","PM2.5",---,---,"µg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","4","NO2",---,---,"µg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","2","C6H6",---,---,"µg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","1","CO",---,---,"mg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","6","SO2",---,---,"µg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","8","H2S",---,---,"µg/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","10","BLACK CARB",---,---,"ng/m³",---,---,---
2025-01-28,104,"Meteo Parchi","Taranto","Taranto",17.222180,40.496730,---,---,"ADI","PRIVATO","11","IPA",---,---,"ng/m³",---,---,---
```

   
5. The `function` node that follows, called `divides messages for each pollutant`, has as many outputs as there are rows in the _array_ received. So if a control unit exposes 6 pollutants, it will be necessary to modify it as follows:
   In the `setup` tab, the number of outputs must be changed, which will be equal to the number of pollutants exposed;
   In the `on message` tab, a strip `let msg1 = { payload: measurements[0] };` must be inserted for each pollutant exposed, like this, for example:

```yaml
              let msg1 = { payload: misurazioni[0] }; // Primo oggetto
              let msg2 = { payload: misurazioni[1] }; // Secondo oggetto
              let msg3 = { payload: misurazioni[2] }; // Terzo oggetto
              let msg4 = { payload: misurazioni[3] }; // Quarto oggetto
              let msg5 = { payload: misurazioni[4] }; // Quinto oggetto
              let msg6 = { payload: misurazioni[5] }; // Sesto oggetto
              let msg7 = { payload: misurazioni[6] }; // settimo oggetto
              let msg8 = { payload: misurazioni[7] }; // ottavo oggetto
              let msg9 = { payload: misurazioni[8] }; // nono oggetto
```

Obviously then we will also modify the subsequent `return` message of the _payload_ thus obtaining:

```yaml 
          return [msg1, msg2, msg3, msg4, msg5, msg6, msg7, msg8, msg9];` 
```

in this way we will have an output for each pollutant to which we will connect, in the same order shown by the API, the MQTT channel through the `mqtt out` node. The nodes, for greater clarity and speed, I have named them as required by the short acronym used in the same regional API.
6. Before publishing the data in `MQTT` I had to insert a new `function` node called `format in value "value" in case of error` which has the purpose of replacing the data not received, exposed by the API with the wording `---`, with the number `0`. This was necessary because otherwise Home Assistant is not able to interpret the data, thus returning a general error of the sensor that will not expose anything.
7. The last thing is therefore the configuration of the relative sensors in Home Assistant, the structure is identical for everyone, only the name of the sensor must be changed in the `name` field and the mqtt channel in the `json_attributes_topic:` field, nothing else:

```yaml

#_configuration.yaml_

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
```

8. Work done and have fun everyone!
   
