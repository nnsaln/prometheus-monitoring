# Contents
* Introduction
    * [Pre-requisites](https://github.com/nnsaln/prometheus-monitoring/#pre-requeisites)
    * [Installation & Configuration](https://github.com/nnsaln/prometheus-monitoring#installation--configuration)
      * [Clone repo](https://github.com/nnsaln/prometheus-monitoring#clone-repo)
      * [Check & Edit Config](https://github.com/nnsaln/prometheus-monitoring#check--edit-config)
        * [Config Prometheus](https://github.com/nnsaln/prometheus-monitoring#config-prometheus)
        * [Config Grafana](https://github.com/nnsaln/prometheus-monitoring#config-grafana)
    * [Start your Service](https://github.com/nnsaln/prometheus-monitoring#start-your-service)
      * [Deploy with docker-compose](https://github.com/nnsaln/prometheus-monitoring#deploy-with-docker-compose)
      * [Prometheus](https://github.com/nnsaln/prometheus-monitoring#prometheus)
        * [Check your Config](https://github.com/nnsaln/prometheus-monitoring#check-your-config)
        * [Check your Metrics](https://github.com/nnsaln/prometheus-monitoring#check-your-metrics)
        * [Check your Targets](https://github.com/nnsaln/prometheus-monitoring#check-your-targets)
        * [Check your Alers and Rules](https://github.com/nnsaln/prometheus-monitoring#check-you-alerts-and-rules)
      * [Grafana](https://github.com/nnsaln/prometheus-monitoring#grafana)
        * [Add Datasource](https://github.com/nnsaln/prometheus-monitoring#add-datasource)
        * [Create Dashboard](https://github.com/nnsaln/prometheus-monitoring#create-dashboard)
        * [Import Dashboard from grafana.com](https://github.com/nnsaln/prometheus-monitoring#import-dashboard-from-grafanacom)
        * [Create Alert](https://github.com/nnsaln/prometheus-monitoring#create-alert)


# Monitoring with Prometheus & Grafana
It's a quick start monitoring using docker-compose. This stack containing prometheus, node-exporter, cadvisor, and grafana. You can quickly deploy your monitoring stack using this, just edit the config, add more jobs or targets and you can see how the magic works! 

# Pre-requisites
Ensure you install docker and docker-compose.

# Installation & Configuration

## Clone Repo
`git clone https://github.com/nnsaln/prometheus-monitoring`

this repo will create this folder tree.

```
├── alertmanager
├── docker-compose.yml
├── grafana
│   └── grafana.ini
├── prometheus
│   ├── alert.rules
│   ├── prometheus.yml
│   └── targets-node-exporter.json
└── README.md
```

## Check & Edit Config

### Config Prometheus
You can check and edit the prometheus config at `prometheus/prometheus.yml`.
If you want to add more targets for `node-exporter` job, edit file `prometheus/targets-node-exporter.json`.
Example:
```
[
  {
    "targets": [
      "<your-target-ip>:9100",
      "<your-target-ip>:9100",
      "<your-target-ip>:9100"
    ]
  }
]
```
If you want to add more jobs edit `prometheus/prometheus.yml`. 
Example:
```
  - job_name: '<your-job-name>'
    scrape_interval: 60s
    metrics_path: <your-metrics-path>
    file_sd_configs:
      - files:
          - <your-target-file.json>
```         
### Config Grafana
You can edit you grafana config at `grafana/grafana.ini`.

# Start your Service

## Deploy with docker-compose

Create and start containers:
```
docker-compose up
```
List the containers:
```
docker ps
```

## Prometheus
Prometheus Web GUI
- `http://<your-prometheus-ip>:9090`
![alt text](https://user-images.githubusercontent.com/6915219/53707793-077fa100-3e63-11e9-9aaa-8e980919be84.png)
### Check your Config
Check your command line flag:
- `http://<your-prometheus-ip>:9090/flags`.
![alt text](https://user-images.githubusercontent.com/6915219/53707847-4150a780-3e63-11e9-9094-0e28280bc924.png)

Check your prometheus config:
- `http://<your-promeheus-ip>:9090/config`
![alt text](https://user-images.githubusercontent.com/6915219/53707904-87a60680-3e63-11e9-8541-df2c6f7afba0.png)

### Check your Metrics
You can check your node-exporter metric:
- `http://<your-node-exporter-target>:9100/metrics`
![alt text](https://user-images.githubusercontent.com/6915219/53707941-b45a1e00-3e63-11e9-9cbe-2f3ae58d9baf.png)
### Check your Targets
And then check if your node exporter targets are listed:
- `http://<your-promeheus-ip>:9090/targets` 

### Check you Alerts and Rules
- Check your alerts: 
  `http://<your-promeheus-ip>:9090/alerts`
  ![alt text](https://user-images.githubusercontent.com/6915219/53707973-cfc52900-3e63-11e9-8b74-c970d9c3af60.png)
- Check your rules: 
  `http://<your-promeheus-ip>:9090/rules`
  ![alt text](https://user-images.githubusercontent.com/6915219/53708015-fbe0aa00-3e63-11e9-9742-8de78e6086e2.png)

## Grafana

You can access your grafana at `http://<your-ip-grafana>:3000`
The default user & password are `admin`. After you login you can change it.

### Add Datasource

Add your prometheus datasource:
`http://<your-ip-grafana>:3000/datasources`

Change the URL address to your prometheus: 
```
http://<your-prometheus-ip>:9090
```
![alt text](https://user-images.githubusercontent.com/6915219/53706475-2713cb00-3e5d-11e9-8db6-f9ccfb0c1fee.png)


### Create Dashboard
- Create your new dashboard
![alt text](https://user-images.githubusercontent.com/6915219/53706666-fe400580-3e5d-11e9-8724-f27fa90d1630.png)

- Edit your graph
Change the **Data Source** to your prometheus datasource that you define before.
PromeQL to get total cpu cores: `count(count(node_cpu_guest_seconds_total) by (cpu, instance))`
![alt text](https://user-images.githubusercontent.com/6915219/53706828-d604d680-3e5e-11e9-91a7-e0c2fdd56cba.png)
Check how to query with PromQL [here](https://prometheus.io/docs/prometheus/latest/querying/basics/)

### Import Dashboard from grafana.com
You can import dasboard templates from [grafana-dashboard](https://grafana.com/dashboards).

- We will use dashboard [Node Exporter Full](https://grafana.com/dashboards/1860) as our node-exporter dashboard template.
![alt text](https://user-images.githubusercontent.com/6915219/53707373-50365a80-3e61-11e9-9010-eaaaf2982962.png)
- Access `http://<your-grafana-ip>:3000/dashboard/import` to import dashboard. You can paste the dashboard url or the id:
![alt test](https://user-images.githubusercontent.com/6915219/53707526-f08c7f00-3e61-11e9-9e31-1c31f130e654.png)
- Your `Node Exporter Full` Dasboard will be like this:
![alt text](https://user-images.githubusercontent.com/6915219/53707652-6c86c700-3e62-11e9-80fa-0d57e4d5426c.png)

### Create Alert
#### Create Notification Channel
Create Bot Telegram
1. Chat `@BotFather` at telegram
2. Type `/newbot` at chat
3. Chose name & username for your bot
4. After that you will get the **token**
5. Create Chat Group in Telegram App, for example: “Super Dooper Alert Group” with people who need to be alerted.
6. Chat you bot with **Start** 
7. Invite your bot to this group.
8. Get chat id via curl
`curl https://api.telegram.org/bot<TOKEN>/getUpdates`
9. Previous step should return JSON object, you need to find key “chat id” like this one:
`"chat":{"id":-123456789,"title":"Super Dooper Alert Group","type":"group","all_members_are_administrators":true}`
10. Create new notification channel at grafana 
`http://<your-grafana-ip>3000/alerting/notification/new`
11. Copy your BOT API Token and Chat ID at `Telegram API settings`
![alt text](https://user-images.githubusercontent.com/6915219/53709117-1ff2ba00-3e69-11e9-860b-f46619ad39d8.png)
12. `Send Test` to your group alert
![alt text](https://user-images.githubusercontent.com/6915219/53709718-46febb00-3e6c-11e9-987f-08acc3b0c092.png)

Create new grafana dashboard for alerting. 
- We will check the disk space usage.
![alt text](https://user-images.githubusercontent.com/6915219/53708233-241cd880-3e65-11e9-9789-fe07b91a73ba.png)
Query: `100 - ((node_filesystem_avail_bytes{device!~'rootfs'} * 100) / node_filesystem_size_bytes{device!~'rootfs'})`
- Create Alert rule at `Alert` tab:
![alt text](https://user-images.githubusercontent.com/6915219/53709809-bf657c00-3e6c-11e9-9fe2-ff1b6957964c.png)
- Add Notification channel that we create before
![alt text](https://user-images.githubusercontent.com/6915219/53709887-3a2e9700-3e6d-11e9-9b06-5f9999a8295c.png)
- Save Dashboard
