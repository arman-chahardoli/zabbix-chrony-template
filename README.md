# Zabbix Chrony Template

Chrony monitoring template for **Zabbix 6.0 LTS** using Zabbix Agent/Agent 2.

![Zabbix Chrony Template](https://raw.githubusercontent.com/arman-chahardoli/zabbix-chrony-template/refs/heads/main/zabbix_chrony_template.png)

## Quick Setup

### 1. Install the Zabbix agent configuration

```bash
sudo mkdir -p /etc/zabbix/zabbix_agent2.d
sudo curl -fsSL \
  https://raw.githubusercontent.com/arman-chahardoli/zabbix-chrony-template/refs/heads/main/zabbix_agent2.d/chrony_service.conf \
  -o /etc/zabbix/zabbix_agent2.d/chrony_service.conf
```

### 2. Install the Chrony status scripts

```bash
sudo curl -fsSL \
  https://raw.githubusercontent.com/arman-chahardoli/zabbix-chrony-template/refs/heads/main/scripts/chrony_global_status \
  -o /usr/local/bin/chrony_global_status

sudo chmod 755 /usr/local/bin/chrony_global_status
```
  
```bash
sudo curl -fsSL \
  https://raw.githubusercontent.com/arman-chahardoli/zabbix-chrony-template/refs/heads/main/scripts/chrony_global_sources_status \
  -o /usr/local/bin/chrony_global_sources_status

sudo chmod 755 /usr/local/bin/chrony_global_sources_status
```

### 3. Restart Zabbix Agent 2

```bash
sudo systemctl restart zabbix-agent2
sudo systemctl status zabbix-agent2
```

### 4. Test the agent items

```bash
sudo zabbix_agent2 -t chrony.stratum
sudo zabbix_agent2 -t chrony.offset
sudo zabbix_agent2 -t chrony.sources_total
sudo zabbix_agent2 -t chrony_global_status_numeric
```

You should receive values similar to:

```text
chrony.stratum                          [s|2]
chrony.offset                           [d|0.000012]
chrony.sources_total                    [u|2]
chrony_global_status_numeric            [u|0]
```

### 5. Import the Zabbix template

Import:

```text
zabbix_template/chrony_service_template.yaml
```

Then link **Chrony Service** to the required Linux host.

The monitoring uses native `chronyc` commands and Zabbix `UserParameter` checks; no additional monitoring software is required.
