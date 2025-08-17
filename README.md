# C2Live

[![Build Status](https://img.shields.io/badge/platform-Linux-blue.svg)](https://shields.io/)
![Maintenance](https://img.shields.io/maintenance/yes/2025.svg?style=flat-square)
[![GitHub last commit](https://img.shields.io/github/last-commit/cybersheepdog/C2Live.svg?style=flat-square)](https://github.com/cybersheepdog/C2Live/commit/main)
![GitHub](https://img.shields.io/github/license/cybersheepdog/C2Live)

## Update: 
Code has been updated to pull down both the older Nightly commits and the newer Weekly commits from [C2Tracker](https://github.com/montysecurity/C2-Tracker).  This was not pulling down the data for the most recent commits of the C2 data as it changed from Nightly to Weekly between July 2nd, 2024 and July 8, 2024.


C2Live is an open-source project aimed at providing a comprehensive and interactive platform for tracking C2 servers, tools, and botnets malicious IP addresses over time. This project focuses on categorizing and visualizing these IPs based on the framework they are associated with and the country they originate from. The goal is to help security professionals, researchers, and organizations gain insights into the evolving landscape of cyber threats. This project is based on [C2Tracker](https://github.com/montysecurity/C2-Tracker) from [@_montysecurity](https://twitter.com/_montysecurity).


Provided by [@Y_NeXRo](https://twitter.com/Y_NeXRo), [ikuroNoriiwa](https://github.com/ikuroNoriiwa), and [cybersheepdog](https://github.com/cybersheepdog)


![alt text](https://github.com/cybersheepdog/C2Live/blob/main/preview.jpg?raw=true)

The website version at [c2tracker.com](https://c2tracker.com)

## Recommended Install method
In order to keep your main python installation on your system a bit cleaner and not have any conflicting requirement version betwen differnt scripts I recommend you creat this in a virtual environment on your system.

```virtualenv C2Live && mv C2Live/ test && git clone https://github.com/cybersheepdog/C2Live.git && cd test/ && mv * ../C2Live/ && cd .. && rm -rf test/ && cd C2Live/ && source bin/activate```

Now you can continue below to install the requirements and run the program.


## To run the project:
### Install requirements.txt
`pip3 install -r requirements.txt`
### launch the docker compose
> Note: Make sure to have docker compose installed :)


`docker-compose -f elastic-grafana-docker-compose.yaml up -d`
> Note: If you have the newer version (v2) of docker compose installed


`docker compose -f elastic-grafana-docker-compose.yaml up -d`
> 
### launch the connectors.py 
`python3 connectors.py`  

It will create geoip pipeline,elastic connector to grafana and import a default dashboard.
### lunch main.py
#### Todays datas
`python3 main.py -u http://localhost:9200/  `  
It will ingest todays data so you will only have 1 day of data. 
#### Past datas
You can also ingest past datas  
`python3 main.py -u http://localhost:9200/ -n <number_of_history_commits>`  
> Note: After July 2, 2024, history commits are equivalent of 1 seek instead of 1 day.  So ingesting 10 history commits will ingest the past 10 weeks of data. Prior to that point it will be 1 day worth of data.

 
You can enjoy grafana dashboard on `http://localhost:3000/ `  
creds are admin:admin

### main.py Usage 
```
usage: C2Live Injector [-h] --elastic-url ELASTIC_URL [--elastic-index ELASTIC_INDEX] [--elastic-verify ELASTIC_VERIFY] [--data-url DATA_URL] [--local-path LOCAL_PATH] [--log-level LOG_LEVEL] [--days DAYS]

Ingest C2 data

optional arguments:
  -h, --help            show this help message and exit
  --elastic-url ELASTIC_URL, -u ELASTIC_URL
                        elasticsearch url
  --elastic-index ELASTIC_INDEX, -i ELASTIC_INDEX
                        elasticsearch index
  --elastic-verify ELASTIC_VERIFY, -ev ELASTIC_VERIFY
                        elasticsearch verify URL
  --data-url DATA_URL, -d DATA_URL
                        Data source github repository
  --local-path LOCAL_PATH, -l LOCAL_PATH
                        Local path
  --log-level LOG_LEVEL, -ll LOG_LEVEL
                        Log Level
  --days DAYS, -n DAYS  Number of history commits from source url 

```
### make a cron with main.py to ingest data weekly on Mondays.


