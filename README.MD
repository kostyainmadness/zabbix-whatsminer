# zabbix-whatsminer

An external script and template for getting Whatsminer M3 metrics into Zabbix.

Script is using Cgminer API. https://github.com/tsileo/pycgminer

I'm using Whatsminer M3 with low-power firmware, so all triggers setup for lower hashrate (~9 Th)

# Supported metrics
* Fan In & Out
* GH/S (RT 5s)
* GH/S (Average)
* Tempreture
* Elapsed time
* Uptime
* ICMP ping

# Supported triggers
* Temp > 85 --> High temp on {HOST.NAME}
* GH/s (Average) < 8.5 TH --> Low average hashrate ({ITEM.LASTVALUE}) on {HOST.NAME}
* ICMP ping --> {HOST.NAME} is unavailable by ICMP
* Uptime <= 0 --> {HOST.NAME} has just been restarted
* Elapsed <= 180 --> {HOST.NAME} Cgminer has just been restarted

# Metrics from cgminer
What you can take from summary (if you want!):
'''
"SUMMARY": [
        {
            "Accepted": 42328,
            "Best Share": 258988397,
            "Device Hardware%": 0.0007,
            "Device Rejected%": 0.038,
            "Difficulty Accepted": 346972160.0,
            "Difficulty Rejected": 131072.0,
            "Difficulty Stale": 0.0,
            "Discarded": 1163316,
            "Elapsed": 175700,
            "Fan Speed In": 2700,
            "Fan Speed Out": 2730,
            "Fan set_get_err_turnoff_threshold": 15,
            "Found Blocks": 0,
            "Get Failures": 0,
            "Getworks": 6281,
            "Hardware Errors": 2379,
            "Last getwork": 1549868976,
            "Local Work": 740990848,
            "MHS 15m": 8344447.22,
            "MHS 1m": 8566004.91,
            "MHS 5m": 8390624.45,
            "MHS 5s": 8703172.47,
            "MHS av": 8431652.53,
            "Network Blocks": 306,
            "Pool Rejected%": 0.0378,
            "Pool Stale%": 0.0,
            "Rejected": 16,
            "Remote Failures": 0,
            "Stale": 0,
            "Temperature": 79.5,
            "Total MH": 1481442755346.0,
            "Uptime": 180198,
            "Utility": 14.45,
            "Voltage": 20,
            "Work Utility": 117789.06,
            "fan[0] set_get_err_turnoff_count": 0,
            "fan[1] set_get_err_turnoff_count": 0,
            "fan_stop_count": 0,
            "fan_stop_turnoff_threshold": 5,
            "fan_stop_turnoff_times": 0,
            "freq_avg": 471,
            "freq_max": 485,
            "freq_min": 455
        }
'''		
# Requirements
* Python 2 or 3
* Python [Requests](https://pypi.python.org/pypi/requests)
* Zabbix 4.0

# Install
* Download whatsminer-zbx-chk and whatsminer_zbx_template.xml
* Put whatsminer-zbx-chk into ExternalScript location (You will find it in zabbix_server configuration)
* Make whatsminer-zbx-chk executable (`chmod 755 whatsminer-zbx-chk`)
* Import whatsminer-zbx-template into zabbix
* Add Whatsminer as Zabbix Host.
* Link template to your ASICs

# Debug
You cant test whatsminer-zbx-chk from CLI

./whatsminer-zbx-chk %HOST% %metrics%

Something like this:

./whatsminer-zbx-chk M3_hostname 'MHS av'

