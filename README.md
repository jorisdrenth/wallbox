# wallbox

Python Module interface for Wallbox EV chargers api

## Usage

### Installation

```python
pip install wallbox
```

## Implemented methods

### authenticate()

- authenticates to the wallbox api.

### getChargersList()

- returns a list of chargers available to the account

### getChargerStatus(chargerID)

- returns a dictionary containing the charger status data

### unlockCharger(chargerId)

- unlocks charger

### lockCharger(chargerId)

- locks charger

### setMaxChargingCurrent(chargerId, chargingCurrentValue)

- sets charger Maximum Charging Current (Amps)

### pauseChargingSession(chargerId)

- pauses a charging session

### resumeChargingSession(chargerId)

- resumes a charging session

### getSessionList(chargerId, startDate, endDate)

- provides the list of charging sessions between startDate and endDate
- startDate and endDate are provided in python datetime format

## Simple example

```python
from wallbox import Wallbox
import time
import datetime

w = Wallbox("user@email", "password")

# Authenticate with the credentials above
w.authenticate()

# Print a list of chargers in the account
print(w.getChargersList())

# Get charger data for all chargers in the list, then lock and unlock chargers
for chargerId in w.getChargersList():
    chargerStatus = w.getChargerStatus(chargerId)
    print(f"Charger Status: {chargerStatus}")
    print(f"Lock Charger {chargerId}")
    endDate = datetime.datetime.now()
    startDate = endDate - datetime.timedelta(days = 30)
    sessionList = w.getSessionList(chargerId, startDate, endDate)
    print(f"Session List: {sessionList}")
    w.lockCharger(chargerId)
    time.sleep(10)
    chargerStatus = w.getChargerStatus(chargerId)
    print(f"Charger {chargerId} lock status {chargerStatus['config_data']['locked']}")
    print(f"Unlock Charger {chargerId}")
    w.unlockCharger(chargerId)
    time.sleep(10)
    chargerStatus = w.getChargerStatus(chargerId)
    print(f"Charger {chargerId} lock status {chargerStatus['config_data']['locked']}")
```
