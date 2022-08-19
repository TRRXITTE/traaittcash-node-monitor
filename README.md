[![NPM](https://nodei.co/npm/@trrxitte/traaittcash-node-monitor.png?downloads=true&stars=true)](https://nodei.co/npm/@trrxitte/traaittcash-node-monitor/)

<h1 align="center"></h1>
<p>
  <img alt="Version" src="https://img.shields.io/badge/version-0.0.8-blue.svg?cacheSeconds=2592000" />
  <a href="https://github.com/trrxitte/traaittcash-node-monitor#readme">
    <img alt="Documentation" src="https://img.shields.io/badge/documentation-meh-brightgreen.svg" target="_blank" />
  </a>
  <a href="https://github.com/trrxitte/traaittcash-node-monitor/graphs/commit-activity">
    <img alt="Maintenance" src="https://img.shields.io/badge/Maintained%3F-yes-green.svg" target="_blank" />
  </a>
  <a href="https://github.com/trrxitte/traaittcash-node-monitor/blob/master/LICENSE">
    <img alt="License: AGPL-3.0" src="https://img.shields.io/badge/License-AGPL-yellow.svg" target="_blank" />
  </a>
  <a href="https://twitter.com/TRRXITTE">
    <img alt="Twitter:  " src="https://img.shields.io/twitter/follow/trrxitte.svg?style=social" target="_blank" />
  </a>
</p>

## Prerequisites

* Node.js >= 6.x
* MySQL/MariaDB
  * Load the [database schema](#database-schema)

## Install



### Collection Service

```sh
npm install -g traaittcash-node-monitor
export MYSQL_HOST=<server ip>
export MYSQL_PORT=<server port>
export MYSQL_USERNAME=<server username>
export MYSQL_PASSWORD=<server password>
export MYSQL_DATABASE=<database>
traaittcash-node-monitor
```

#### Additional Options

```sh
export MYSQL_SOCKET=<server socket path (default: not set)>
export MYSQL_CONNECTION_LIMIT=<# of maximum server connections (default: 10)>
export HISTORY_DAYS=<# of days to keep history (default: 6 hours)>
export UPDATE_INTERVAL=<# of seconds between updating node list (default: 1 hour)>
export POLLING_INTERVAL=<# of seconds between checking nodes (default: 120s)>
export NODE_LIST_URL=<Full URL to node list (default: turtlecoin-nodes-json)>
```

### As a Module for Pulling Stats

```sh
npm install --save traaittcash-node-monitor
```

#### Sample Code

```javascript
const StatsDatabase = require('traaittcash-node-monitor')

const db = new StatsDatabase({
  host: 'localhost',
  port: 3306,
  username: 'root',
  password: 'password',
  database: 'traaittcash',
  connectionLimit: 10
})

db.getNodeStats().then((stats) => {
  console.log(stats)
})
```

## Database Schema

```sql
CREATE TABLE IF NOT EXISTS `nodes` (
  `id` varchar(64) NOT NULL,
  `name` varchar(255) NOT NULL,
  `hostname` varchar(255) NOT NULL,
  `port` int(11) NOT NULL DEFAULT 14486,
  `ssl` int(11) NOT NULL DEFAULT 0,
  `cache` int(11) NOT NULL DEFAULT 0,
  PRIMARY KEY (`id`),
  KEY `ssl` (`ssl`),
  KEY `cache` (`cache`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE IF NOT EXISTS `node_polling` (
  `id` varchar(64) NOT NULL,
  `timestamp` bigint(1) unsigned NOT NULL,
  `status` int(11) NOT NULL DEFAULT 0,
  `feeAddress` varchar(255) DEFAULT NULL,
  `feeAmount` bigint(20) NOT NULL DEFAULT 0,
  `height` bigint(20) NOT NULL DEFAULT 0,
  `version` varchar(20) NOT NULL DEFAULT '0.0.0',
  `connectionsIn` int(11) NOT NULL DEFAULT 0,
  `connectionsOut` int(11) NOT NULL DEFAULT 0,
  `difficulty` bigint(20) NOT NULL DEFAULT 0,
  `hashrate` bigint(20) NOT NULL DEFAULT 0,
  `txPoolSize` bigint(20) NOT NULL DEFAULT 0,
  PRIMARY KEY (`id`,`timestamp`),
  KEY `status` (`status`),
  KEY `feeAmount` (`feeAmount`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 ROW_FORMAT=COMPRESSED;
```

## Author

**TRRXITTE Int., incorporate**

* Twitter: [@trrxitte](https://twitter.com/trrxitte )
* Github: [@trrxitte](https://github.com/trrxitte)

**The TurtleCoin Developers**

* Twitter: [@turtlecoin](https://twitter.com/_turtlecoin )
* Github: [@turtlecoin](https://github.com/turtlecoin)

## License
Copyright © 2022 [TRRXITTE Int., incorporate](https://github.com/trrxitte)

Copyright © 2019 [The TurtleCoin Developers](https://github.com/turtlecoin).<br />
This project is [AGPL-3.0](https://github.com/turtlecoin/cryptodira/blob/master/LICENSE) licensed.
