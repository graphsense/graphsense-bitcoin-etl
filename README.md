[![pytest](https://github.com/graphsense/graphsense-bitcoin-etl/actions/workflows/pytest.yml/badge.svg)](https://github.com/graphsense/graphsense-bitcoin-etl/actions/workflows/pytest.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# A GraphSense component to ingest Bitcoin Core data to Apache Cassandra (DEPRECATED)

The [graphsense-lib]( https://github.com/graphsense/graphsense-lib) supersede this repository, it provides the same functionality e.g. to import btc/ltc/... data into cassandra use

    graphsense-cli -v ingest from-node -e dev -c ltc --previous_day --batch-size 100 --create-schema

This requires a properly configured gs config file. The default location is ~/.graphsense.yaml. An example dev environment config could look as follows


    environments:
      dev:
        cassandra_nodes:
        - localhost
        keyspaces:
            ltc:
                raw_keyspace_name: ltc_raw
                transformed_keyspace_name: ltc_transformed
                schema_type: utxo
                ingest_config:
                  node_reference: http://[user]:[pw]@localhost:8532

## Prerequisites

### Apache Cassandra

Download and install [Apache Cassandra][apache-cassandra] >= 3.11
in `$CASSANDRA_HOME`.

Start Cassandra (in the foreground for development purposes):

    $CASSANDRA_HOME/bin/cassandra -f

Connect to Cassandra via CQL

    $CASSANDRA_HOME/bin/cqlsh

and test if it is running

    cqlsh> SELECT cluster_name, listen_address FROM system.local;

    cluster_name | listen_address
    --------------+----------------
    Test Cluster |      127.0.0.1

    (1 rows)

## Local setup

Create and activate a python environment for required dependencies
([bitcoin-etl][bitcoin-etl] and
[Python client driver for Apache Cassandra][python-cassandra]).

    python3 -m venv venv
    . venv/bin/activate

Install dependencies in local environment

    pip install -r requirements.txt

Starting on a freshly installed database, first create a keyspace

    scripts/create_keyspace.py -d $CASSANDRA_HOST -k $KEYSPACE -s schema.cql

Then start the data ingest.  

The process will continue from the latest block id found in the `block` table,
or start at genesis block 0.

    scripts/btc_cassandra_streaming.py -d $CASSANDRA_HOST -k $KEYSPACE -p $PROVIDER_URI 

By default, data until the **latest** block available from the Bitcoin Core
client will be ingested.  To stop ingesting at an earlier block, use the
`-e/--end_block` option (see `btc_cassandra_streaming.py -h`).

[bitcoin-etl]: https://github.com/blockchain-etl/bitcoin-etl
[python-cassandra]: https://github.com/datastax/python-driver
[apache-cassandra]: http://cassandra.apache.org/download
[coindesk]: https://www.coindesk.com/api
[coinmarketcap]: https://coinmarketcap.com
[graphsense-cli]: https://github.com/graphsense/graphsense-lib#exchange-rates
