# Transitland Python Client

[![Circle CI](https://circleci.com/gh/transitland/transitland-python-client.png?style=badge)](https://circleci.com/gh/transitland/transitland-python-client)

## Installation

Installation using pip:

```
pip install transitland
```

Alternatively, [download from PyPi](https://pypi.python.org/pypi/transitland) or clone this repository, and install using setup.py:

```
python ./setup.py install
```

The dependencies [mzgeohash](https://github.com/transitland/mapzen-geohash) and [mzgtfs](https://github.com/transitland/mapzen-gtfs) will be automatically installed using the above methods.


## Opening the Transitland Feed Registry

First, make sure you have a copy of the current Transitland Feed Registry. This can be accomplished through a git clone:

```
git clone https://github.com/transitland/transitland-feed-registry.git
```

Then, create a registry reader with transitland.registry.FeedRegistry, passing the path to the registry as the argument.

```
>>> import transitland.registry
>>> registry = transitland.registry.FeedRegistry('.')
```

You can now list the known feeds:

```
>>> registry.feeds()
['f-9q5-lacmta', 'f-9q8-samtrans', 'f-9q8y-sanfranciscomunicipaltransportationagency', 'f-9q9-actransit', 'f-9q9-caltrain', 'f-9q9-vta', 'f-9vk-mtaharriscounty', 'f-c20-trimet', 'f-c23-kingcounty', 'f-c28-translink', 'f-dhw-miamidade', 'f-dnh-marta', 'f-dp3-cta', 'f-dpz8-ttc', 'f-dq-mtamaryland', 'f-dqc-wmata', 'f-dr5r-nyctsubway', 'f-dr5r-panynjpath', 'f-drt-mbta']
```

## Working with a feed

A specific feed can be read with FeedRegistry.feed(). The resulting Feed can be used to inspect the feed attributes, download the current version of the feed, etc.

```
>>> feed = registry.feed('f-9q9-caltrain')
>>> feed.operatorsInFeed()
'o-9q9-caltrain'
>>> feed.url()
'http://www.caltrain.com/Assets/GTFS/caltrain/GTFS-Caltrain-Devs.zip'
>>> feed.download('caltrain-current-gtfs.zip')
```

## Download current GTFS feeds

The current versions of GTFS feeds can be downloaded to the current directory using transitland.fetch:

```
$ python -m transitland.fetch f-9q9-caltrain
Downloading: http://www.caltrain.com/Assets/GTFS/caltrain/GTFS-Caltrain-Devs.zip -> f-9q9-caltrain.zip
```

The "--all" option can also be used to download all feeds in the registry. Only updated feeds will be downloaded:

```
$ python -m transitland.fetch --all
Downloading: http://developer.metro.net/gtfs/google_transit.zip -> f-9q5-lacmta.zip
Downloading: http://www.samtrans.com/Assets/GTFS/samtrans/SamTrans-GTFS.zip -> f-9q8-samtrans.zip
Downloading: http://archives.sfmta.com/transitdata/google_transit.zip -> f-9q8y-sanfranciscomunicipaltransportationagency.zip
...
```

## Bootstrapping a feed from a GTFS source

A Feed can be created from a GTFS url with transitland.bootstrap. Specify the URL with "--url" and the feed name with "--feedname":

```
$ python -m transitland.bootstrap --url http://developer.trimet.org/schedule/gtfs.zip --feedname trimet
Attempting to bootstrap feed from GTFS file...
Downloading: http://developer.trimet.org/schedule/gtfs.zip -> ./data/trimet-bootstrap.zip
Loading feed: trimet-bootstrap
Feed: f-c20-trimet
  Operators: 1
  Routes: 89
  Stops: 6879
Operator: TriMet
  Routes: 89
  Stops: 6879
```

A basic feed description will be written to the feeds directory that can then be annotated with details about licenses, additional identifiers, etc.

```json
{
    "feedFormat": "gtfs",
    "name": "trimet",
    "onestopId": "f-c20-trimet",
    "operatorsInFeed": [
        {
            "gtfsAgencyId": "trimet",
            "onestopId": "o-c20-trimet"
        }
    ],
    "tags": {},
    "url": "http://developer.trimet.org/schedule/gtfs.zip"
}
```

## Contributing

Please [open a Github issue](https://github.com/transitland/transitland-python-client/issues/new) with as much of the following information as yo're able to specify, or [contact us](#contact) for assistance.

## License

Unless otherwise indicated, the code used to build and maintain this registry is Copyright (C) 2014-2015 Mapzen and released under the [MIT license](http://opensource.org/licenses/MIT).

## Contact

Transitland is sponsored by [Mapzen](http://mapzen.com). Contact us with your questions, comments, or suggests: [hello@mapzen.com](mailto:hello@mapzen.com).
