<p align="center">
    <img src="https://github.com/da-ekchajzer/cloud-scanner/raw/main/cloudscanner_color.svg" height="100">
</p>


[CloudScanner 📡](https://github.com/Boavizta/cloud-scanner) is a tool that leverages [BoaviztAPI 🛠️](https://github.com/Boavizta/boaviztapi) to assess environmental impacts of your AWS account.

## Commands

Use the CLI with [🐳 Docker](https://boavizta.github.io/cloud-scanner/tutorials/quickstart-docker.html) or [🦀 Cargo](https://boavizta.github.io/cloud-scanner/tutorials/quickstart-rust-cli.html)

### Inventory: list instances

Command `inventory` will list all EC2 instances from an AWS account within a specific region.

Example:

```bash
cargo run -- --aws-region eu-west-3 inventory | jq
```

Output:

```json
[
  {
    "provider": "AWS",
    "id": "i-098cc6c2fea8580d9",
    "location": {
      "aws_region": "eu-west-3",
      "iso_country_code": "FRA"
    },
    "resource_details": {
      "Instance": {
        "instance_type": "t2.micro",
        "usage": {
          "average_cpu_load": 32.5062517365935,
          "usage_duration_seconds": 300
        }
      }
    },
    "tags": [
      {
        "key": "Name",
        "value": "lab-1"
      }
    ]
  }
]
```


### Estimate: evaluate of impacts

Command `estimate` will evaluate the impacts of using EC2 instances for specific period of time (e.g. `--hours-use-time 1` for 1 hour).

**[⚠️ EXPERIMENTAL]** Optionally you can include the impacts of using block storage with `--include-block-storage` flag.

Example:

```bash
cargo run -- --aws-region eu-west-3 estimate --hours-use-time 24 | jq
```

<details><summary>Output:</summary>

```json
{
  "impacts": [
    {
      "cloud_resource": {
        "provider": "AWS",
        "id": "i-098cc6c2fea8580d9",
        "location": {
          "aws_region": "eu-west-3",
          "iso_country_code": "FRA"
        },
        "resource_details": {
          "Instance": {
            "instance_type": "t2.micro",
            "usage": {
              "average_cpu_load": 0.3355932203389826,
              "usage_duration_seconds": 300
            }
          }
        },
        "tags": [
          {
            "key": "Name",
            "value": "lab-1"
          }
        ]
      },
      "resource_impacts": {
        "adp_manufacture_kgsbeq": 0.0000027,
        "adp_use_kgsbeq": 3.6E-9,
        "pe_manufacture_megajoules": 0.21,
        "pe_use_megajoules": 0.84,
        "gwp_manufacture_kgco2eq": 0.016,
        "gwp_use_kgco2eq": 0.0073,
        "raw_data": {
          "impacts": {
            "adp": {
              "description": "Use of minerals and fossil ressources",
              "embedded": {
                "max": 0.000003497,
                "min": 0.000002137,
                "value": 0.0000027,
                "warnings": [
                  "End of life is not included in the calculation"
                ]
              },
              "unit": "kgSbeq",
              "use": {
                "max": 4.368E-9,
                "min": 3.276E-9,
                "value": 3.6E-9
              }
            },
            "gwp": {
              "description": "Total climate change",
              "embedded": {
                "max": 0.02434,
                "min": 0.009994,
                "value": 0.016,
                "warnings": [
                  "End of life is not included in the calculation"
                ]
              },
              "unit": "kgCO2eq",
              "use": {
                "max": 0.008812,
                "min": 0.006609,
                "value": 0.0073
              }
            },
            "pe": {
              "description": "Consumption of primary energy",
              "embedded": {
                "max": 0.3204,
                "min": 0.1341,
                "value": 0.21,
                "warnings": [
                  "End of life is not included in the calculation"
                ]
              },
              "unit": "MJ",
              "use": {
                "max": 1.015,
                "min": 0.7613,
                "value": 0.84
              }
            }
          }
        }
      },
      "impacts_duration_hours": 24.0
    }
  ],
  "executionStatistics": {
    "inventory_duration": {
      "secs": 0,
      "nanos": 850687309
    },
    "impact_duration": {
      "secs": 0,
      "nanos": 390805550
    },
    "total_duration": {
      "secs": 1,
      "nanos": 241493468
    }
  }
}
```
</details>

### Serve: expose metrics

Command `serve` will start and HTTP server to expose metrics.

▶ Metrics: [http://localhost:8000/metrics?aws_region=eu-west-3](http://localhost:8000/metrics?aws_region=eu-west-3)

▶ Swagger: [http://localhost:8000/swagger-ui](http://localhost:8000/swagger-ui)



## Dashboard

### Using Docker Compose

```bash
export AWS_PROFILE=my-profile-name
docker compose up -d
```

![Grafana dashboard](https://github.com/Boavizta/cloud-scanner/raw/main/docs/src/images/cloud-scanner-dashboard-clear.png)
