## Install MongoDB and Python wrapper

```
sudo apt-get install -y mongodb-org
python3 -m pip install -U pymongo
```

## Start MongoDB daemon

```
mongod
```

> #### *or:*
>    service mongod start

## Load JSON records into DB in BSON format

```
mongoimport --db aapb --collection pbcore --file /Volumes/U/AAPB_MongoDB_data/PBCore_JSON_70K.txt
```

## Querying MongoDB in Python

```python
from pymongo import MongoClient
from pprint import pprint

client = MongoClient()

records = client.aapbsample.pbcore
```

### Search for exact creator name

```
cursor = records.find({ 'pbcoreDescriptionDocument.pbcoreCreator.creator' :  "Worden, Michelle"})

for item in cursor:
    pprint(item)
```

### Search for exact program title

```
cursor = records.find({ 'pbcoreDescriptionDocument.pbcoreTitle.#text' :  "Grace: A Tribute to Pope John Paul II"})

for item in cursor:
    pprint(item)
```

### Search for records containing "Music" as a genre

```
cursor = records.find({ 'pbcoreDescriptionDocument.pbcoreGenre.#text' :  "Music"})

for item in cursor:
    pprint(item)
```

### Search for records that include "Arkansas" in description

```
cursor = records.find({ 'pbcoreDescriptionDocument.pbcoreDescription.#text' :  {'$regex':'.*Arkansas.*'}})

for item in cursor:
    pprint(item)
```


