# Scaphandre workshop sheet

Scaphandre in an open source software capable to estimate the energy consumed by your machine and attribute this consumption to the running processes.


#### Scaphandre public repository: https://github.com/hubblo-org/scaphandre
#### Scaphandre doc: https://hubblo-org.github.io/scaphandre-documentation/

## Getting started

To quickly run scaphandre in your terminal you may use docker:

```bash
docker run --privileged -v /sys/class/powercap:/sys/class/powercap -v /proc:/proc -ti hubblo/scaphandre stdout -t 15
```

It should write in Stdout the top 5 process consumer on your machine for 15 secondes.

expected kind of output:

```bash
Scaphandre stdout exporter
Sending ⚡ metrics
Measurement step is: 2s
Host:	0 W
	package 	core		dram
Top 5 consumers:
Power		PID	Exe
No processes found yet or filter returns no value.
------------------------------------------------------------

Host:	2.563512 W
	package 	core		dram
Socket0	2.563512 W |	0.807113 W	0.527853 W	

Top 5 consumers:
Power		PID	Exe
0.445828 W	641	"nv_queue"
0.111457 W	398	"systemd-journal"
0.111457 W	639	"irq/131-nvidia"
0.111457 W	649	"dbus-daemon"
0 W	1	"systemd"
------------------------------------------------------------
```


The exporter used **stdout** is quite basic. other exporters exists, have a look at the [documentation](https://hubblo-org.github.io/scaphandre-documentation/references/exporter-json.html) (json, prometheus, etc...)


## Try the prometheus exporter

```bash
docker run --privileged -v /sys/class/powercap:/sys/class/powercap -v /proc:/proc -p 8080:8080 -ti hubblo/scaphandre prometheus
```

when the container is deployed, check the metrics endpoint:

```bash
curl -s http://localhost:8080/metrics
```

## Run a complete stack with docker-compose

You can have a look at the documentation page about how to [Run a complete stack with docker-compose](https://hubblo-org.github.io/scaphandre-documentation/tutorials/docker-compose.html#run-a-complete-stack-with-docker-compose) 


Start by going to the scaphandre folder of this repository and run docker-compose:


```bash
cd scaphandre
docker-compose up
```

Grafana will be available at http://localhost:3000, the default username is `admin` and the password is `secret`.

## Going further


- Create a dashboard to look at the consumption of your browser. The interesting metrics is named `scaph_process_power_consumption_microwatts`

- Create a dashboard to look at the sum of the power domain power consumption and the host host consumption. Is there a difference ?

hints:
- [available metrics in scaphandre](https://hubblo-org.github.io/scaphandre-documentation/references/metrics.html)
- [scaphandre documentaiton, about RAPL domain](https://hubblo-org.github.io/scaphandre-documentation/explanations/rapl-domains.html)