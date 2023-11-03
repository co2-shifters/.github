# Welcome CO2-Shifters!

Confluence: https://confluence.axa.com/confluence/x/UasEG

## Teamzusammenstellung
Team 1: Anbindung API + Optimierungslösung
-	Elmedin
-	Noemi
-	Maurice
-	Jay

Team 2: Frontend + Testing Prognosen
-	Sven
-	Noah
-	Armin

Team 3: Infrastruktur + Konzepte + Pitch
-	Davide (Lead)
-	Christoph (Co-Lead)
-	Simone
-	Joshua
-	Johanna

## Mögliche Risiken
![image](https://github.com/co2-shifters/.github/assets/46636416/2bd57d39-aef5-4035-ac5e-79a731103129)


## Architektur
![image](https://github.com/co2-shifters/.github/assets/46636416/27964447-6625-4553-b909-570934e2a25a)

### API (1)
API Aufruf von aktuellen CO2 Prognose-Werten (2 Tage Prognose verfügbar.

```json
{
  "zone": "CH",
  "forecast": [
    {
      "carbonIntensity": 90,
      "datetime": "2023-11-01T07:00:00.000Z"
    },
    {
      "carbonIntensity": 107,
      "datetime": "2023-11-01T08:00:00.000Z"
    },
...
    {
      "carbonIntensity": 81,
      "datetime": "2023-11-03T05:00:00.000Z"
    },
    {
      "carbonIntensity": 79,
      "datetime": "2023-11-03T06:00:00.000Z"
    }
  ],
  "updatedAt": "2023-11-01T06:50:23.662Z"
}
```


API Aufruf von historisierten CO2 Werten.

```json
{
  "zone": "CH",
  "history": [
    {
      "zone": "CH",
      "carbonIntensity": 69,
      "datetime": "2023-10-31T08:00:00.000Z",
      "updatedAt": "2023-11-01T06:46:43.768Z",
      "createdAt": "2023-10-28T09:09:52.517Z",
      "emissionFactorType": "lifecycle",
      "isEstimated": true,
      "estimationMethod": "MODE_BREAKDOWN"
    },
...
    {
      "zone": "CH",
      "carbonIntensity": 51,
      "datetime": "2023-11-01T06:00:00.000Z",
      "updatedAt": "2023-11-01T06:46:43.768Z",
      "createdAt": "2023-10-29T07:06:26.168Z",
      "emissionFactorType": "lifecycle",
      "isEstimated": true,
      "estimationMethod": "MODE_BREAKDOWN"
    },
    {
      "zone": "CH",
      "carbonIntensity": 57,
      "datetime": "2023-11-01T07:00:00.000Z",
      "updatedAt": "2023-11-01T06:46:48.069Z",
      "createdAt": "2023-10-29T08:04:20.729Z",
      "emissionFactorType": "lifecycle",
      "isEstimated": true,
      "estimationMethod": "MODE_BREAKDOWN"
    }
  ]
}
```

### Schnittstelle API - Frontend (2)
Aufruf von Fronted (payload)
```json
{
  "datetime": "2023-10-31T08:00:00.000Z",
  "start_datetime": "2023-11-01T01:00:00.000Z",
  "end_datetime": "2023-11-01T10:00:00.000Z", 
  "duration": "60", # minutes
}
```


Rückgabe API an Frontend

```json
{
  "input_payload" : {
    "datetime": "2023   -10-31T08:00:00.000Z",
    "start_datetime": "2023-11-01T01:00:00.000Z",
    "end_datetime": "2023-11-01T10:00:00.000Z", 
    "duration": "60", # minutes
  }
,
  "forecast_range" : [{
    "rank": 1,
    "start_datetime": "2023-11-01T01:00:00.000Z",
    "end_datetime": "2023-11-01T02:00:00.000Z", 
    "duration": "60", # minutes
    "total_carbonIntensity": 90,
  },
    "rank": 2,
    "start_datetime": "2023-11-01T02:00:00.000Z",
    "end_datetime": "2023-11-01T03:00:00.000Z", 
    "duration": "60", # minutes
    "total_carbonIntensity": 100,
  },
]
,
    "forecast" :
    {
      "zone": "CH",
      "forecast": [
        {
          "carbonIntensity": 90,
          "datetime": "2023-11-01T07:00:00.000Z",
          "is_optimal": "1"
 
        },
        {
          "carbonIntensity": 107,
          "datetime": "2023-11-01T08:00:00.000Z",
          "is_optimal": "1"
        },
    ...
        {
          "carbonIntensity": 81,
          "datetime": "2023-11-03T05:00:00.000Z",
          "is_optimal": "0"
        },
        {
          "carbonIntensity": 79,
          "datetime": "2023-11-03T06:00:00.000Z",
          "is_optimal": "0"
        }
      ],
      "updatedAt": "2023-11-01T06:50:23.662Z"
    }
}
```

