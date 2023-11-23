<p align="center">
    <img src="https://github.com/Boavizta/boaviztapi/raw/main/boaviztapi_color.svg" height="100">
</p>

[BoaviztAPI 🛠️](https://github.com/Boavizta/boaviztapi) is an API to evaluate environmental impacts of IT resources (servers, cloud, laptops...) inspired by LCA (multi-steps and multi-criteria).

🔗 Links:

* Repository: [Boavizta/boaviztapi](https://github.com/Boavizta/boaviztapi)
* [Docs](https://doc.api.boavizta.org/)
* [Swagger](https://api.boavizta.org/docs)
* Endpoint: [api.boavizta.org](https://api.boavizta.org) (not production ready)
* [Python SDK](https://pypi.org/project/boaviztapi-sdk/)
* Frontend: [Datavizta](https://datavizta.boavizta.org/)

## Impacts of a CPU

**POST** `/v1/component/cpu?verbose=False`

```json
{
	"name": "Intel Xeon Gold 6134"
}
```

<details><summary>Or with curl:</summary>

```bash
curl -X 'POST' \
  'https://api.boavizta.org/v1/component/cpu?verbose=false' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "name": "Intel Xeon Gold 6134"
}'
```
</details>


<details><summary>Output:</summary>
```json
{
  "impacts": {
    "gwp": {
      "embedded": {
        "value": 23.78,
        "min": 23.78,
        "max": 23.78,
        "warnings": [
          "End of life is not included in the calculation"
        ]
      },
      "use": {
        "value": 900,
        "min": 57.19,
        "max": 2814
      },
      "unit": "kgCO2eq",
      "description": "Total climate change"
    },
    "adp": {
      "embedded": {
        "value": 0.0204,
        "min": 0.0204,
        "max": 0.0204,
        "warnings": [
          "End of life is not included in the calculation"
        ]
      },
      "use": {
        "value": 0.00016,
        "min": 0.00003292,
        "max": 0.0006604
      },
      "unit": "kgSbeq",
      "description": "Use of minerals and fossil ressources"
    },
    "pe": {
      "embedded": {
        "value": 352.9,
        "min": 352.9,
        "max": 352.9,
        "warnings": [
          "End of life is not included in the calculation"
        ]
      },
      "use": {
        "value": 30000,
        "min": 32.33,
        "max": 1164000,
        "warnings": [
          "Uncertainty from technical characteristics is very important. Results should be interpreted with caution (see min and max values)"
        ]
      },
      "unit": "MJ",
      "description": "Consumption of primary energy"
    }
  }
}
```
</details>

## Impacts of a server

**POST** `/v1/server/`

```json
{
  "model": {
    "type": "rack"
  },
  "configuration": {
    "cpu": {
      "units": 2,
      "name": "Intel Xeon Gold 6134"
    },
    "ram": [
      {
        "units": 8,
        "capacity": 32
      }
    ],
    "disk": [
      {
        "units": 1,
        "type": "ssd",
        "capacity": 512
      }
    ]
  },
  "usage": {
    "avg_power": 400,
    "usage_location": "FRA"
  }
}
```

## Impacts of an EC2 instance

**POST** `/v1/cloud/instance`

```json
{
  "provider": "aws",
  "instance_type": "a1.4xlarge",
  "usage": {
    "usage_location": "FRA",
    "time_workload": [
      {
        "time_percentage": 80,
        "load_percentage": 5
      },
      {
        "time_percentage": 20,
        "load_percentage": 90
      }
    ]
  }
}
```
