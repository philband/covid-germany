# covid-germany

Some analysis for the COVID situation in Germany

## Stack

* Influxdb for data storage
* Grafana for visualization
* Python scrip to convert data https://github.com/nhancv/nc-csv2influxdb

## Data source

https://npgeo-corona-npgeo-de.hub.arcgis.com/datasets/dd4580c810204019a7b8eb3e0b329dd6_0

## Howto

### Stack setup (in Kubernetes, using helm)

```bash
helm repo add influxdata https://influxdata.github.io/helm-charts
helm repo update
helm upgrade -i grafana stable/grafana
helm upgrade -i influxdb influxdata/influxdb --set persistence.storageClass=local-path
```

### Python 3 deps
```bash
pip install panda progressbar
```

### Data import

```bash
# Reformat original dataset to line format used in influxdb
python csv2line.py -d $DATABASE_NAME -i data/20200323_rki.csv -o data/20200323_rki.txt
# Import to influxdb
influx -import -path data/20200323_rki.txt -precision 'ns'
```

### Import grafana dashboard from vis folder