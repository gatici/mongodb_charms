# Overview

This interface layer handles communication between Mongodb and its clients.

## Usage

### Provides

To implement this relation to offer a mongodb:

In your charm's metadata.yaml:
```yaml
provides:
    mongo:
        interface: mongodb
```

reactive/mymongo.py:
```python
@when('mongo.joined')
def send_config(mongo):
    mongo.send_connection(
        get_mongo_port(),
        unit_get('private-address'),
    )
```

### Requires

If you would like to use a mongodb from your charm:

metadata.yaml:
```yaml
requires:
    mongo:
        interface: mongodb
```

reactive/mycharm.py:
```python
@when('mongo.ready')
def mongo_ready():
    mongo = endpoint_from_flag('mongo.ready')
    if mongo:
        for unit in mongo.mongodbs():
            add_mongo(unit['host'], unit['port'])
```
