# How many ways we can create ?
- Import via Dashboard ID (Global Dashboards ID's are there)
- Create via JSON (Write or Upload File)
- Create the Dashboard from Scratch (Add visualizations manually)

# Creating Dashboards
- Add datasource first
- Add queries (A,B,C,D.... etc)

# Alerts Creation
- Types of Expressions (Classic/Reduce/Math)
- 5 Configure Notification - Add Labels here that we will use whole creating notification policies

## Option 1 
- Reduce and threshold condition

## Option 2 
- Classic condition

## Option 3 
- Math expression - if you want to add (or any other math operation) multiple expressionns and based on the value set the alerting


# Evaluation Group
If you assign multiple alerts to same Evaluation Group: infra_alerts_1m, and set it to evaluate every 1 minute, Grafana will:
Evaluate them together every 1 minute
Log them under the same evaluation group in alert logs


# Pending Period
- 5m - if alert is violating for 5mins fire an alert
- 0m - As soon as alert violates fire


# How to disable/Pause Alerts on Grafana
- Pencil Icon
- Evaluatoin section
- Pause Evaluation


# Create Contact Points
- Alerting can be sent to Emails, discord, slack etc
- For sending mails we need to configire mail in Grafana

# Grafana Mail setutp
- We will need to create app pass from google account
- then in /etc/grafana/grafana.ini paste the below
```
#################################### SMTP / Emailing ##########################

[smtp]
enabled = true
host = smtp.gmail.com:587
user = your-email@gmail.com
password = your-app-password
; If the from_address is not set, the default value is 'admin@grafana.localhost'
from_address = your-email@gmail.com
from_name = Grafana Alerts
skip_verify = false
startTLS_policy = OpportunisticStartTLS

```
- then $ sudo systemctl restart grafana-server


# Connecting Alert Rule with Contact Points via Notification Policies
- New nested policy
- Add Label that we created and used in our alert 5th point


# How to link Alert rule and Panel
- Multiple Alert rules can be created on one panel but it is not necessary that it is linked to that panel.
- Linking just makes the alert rule visible on the panel by displaying heart icon and dotted lines when it is triggered and back to normal
- To link go to alert 4th section of Annotation and Link to Panel


# How to check alert history
- Go to alert rule
- Expand Alert
- Show state history

