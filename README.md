# Ballerina guide to set up New Relic

This guide will help you to set up your Ballerina application to publish metrics, traces and logs to New Relic.

**Prerequisites**:
- Docker

Follow the steps given below.

## Step 1 - Setup Fluent-Bit
1. Clone this repository in your local machine.
```bash
git clone https://github.com/NipunaMadhushan/ballerina-newrelic-setup.git
```
2. Then set the `NEW_RELIC_LICENSE_KEY` in the `config/.env` file.
3. Then run the following command to start the fluent-bit agent.
```bash
docker compose -f docker-compose.yml up -d
```

## Step 2 - Set up runtime configurations
Set the following runtime configurations in your Ballerina application. A sample Ballerina application can be found in [here](https://ballerina.io/learn/overview-of-ballerina-observability/#example-observe-a-ballerina-service).

Create/update the `Config.toml` file with following conifgurations.

```toml
[ballerina.observe]
metricsEnabled = true
metricsReporter = "newrelic"
tracingEnabled = true
tracingProvider = "newrelic"
 
[ballerinax.newrelic]
apiKey = "<NEW_RELIC_LICENSE_KEY>"


[[ballerina.log.destinations]]
path = "<PATH_TO_REPOSITORY_FOLDER>/logs/ballerina/<PATH>/<LOG_FILE_NAME>.log"
```

**Note:** 
> log file path should be where the user cloned the `ballerina-newrelic-setup` repository. This is important since the fluent-bit reads the log files inside the `logs/ballerina` directory.

## Step 3 - Import New Relic extension
Add the following import in your Ballerina application.

```ballerina
import ballerinax/newrelic as _;
```

## Step 4 - Run the Ballerina application
Run the following command to start your Ballerina application.

```bash
bal run
```

## Step 5 - View Ballerina metrics
Go to the `Metrics & Events` tab in the user's `New Relic` account and open query console.

Then write an query as follows.

```
SELECT latest(requests_total_value) FROM Metric SINCE 60 MINUTES AGO UNTIL NOW FACET dimensions() LIMIT 100 TIMESERIES 30000
```
This will show you the request counts of your Ballerina service.

Similarly you can use queries to create a dashboard in the `Dashboard` tab.

## Step 6 - View Ballerina traces
Go to the `Trace` tab in the user's `New Relic` account.

Users can filter the traces as well according to their needs.

## Step 7 - View Ballerina logs
Go to the `Logs` tab in the user's `New Relic` account.

Users can filter the logs as well according to their needs.
