# n-express-monitor

[![npm version](https://badge.fury.io/js/%40financial-times%2Fn-express-monitor.svg)](https://badge.fury.io/js/%40financial-times%2Fn-express-monitor)
![npm download](https://img.shields.io/npm/dm/@financial-times/n-express-monitor.svg)
![node version](https://img.shields.io/node/v/@financial-times/n-express-monitor.svg)

[![CircleCI](https://circleci.com/gh/Financial-Times/n-express-monitor.svg?style=shield)](https://circleci.com/gh/Financial-Times/n-express-monitor)
[![Coverage Status](https://coveralls.io/repos/github/Financial-Times/n-express-monitor/badge.svg?branch=master)](https://coveralls.io/github/Financial-Times/n-express-monitor?branch=master)
[![Known Vulnerabilities](https://snyk.io/test/github/Financial-Times/n-express-monitor/badge.svg)](https://snyk.io/test/github/Financial-Times/n-express-monitor)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/Financial-Times/n-express-monitor/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/Financial-Times/n-express-monitor/?branch=master)
[![Dependencies](https://david-dm.org/Financial-Times/n-express-monitor.svg)](https://david-dm.org/Financial-Times/n-express-monitor)
[![devDependencies](https://david-dm.org/Financial-Times/n-express-monitor/dev-status.svg)](https://david-dm.org/Financial-Times/n-express-monitor?type=dev)

<br>

- [Install](#install)
- [Demo](#demo)
- [Usage](#usage)
  * [setupMonitor](#setupmonitor)
  * [monitor](#monitor)
  * [monitorService](#monitorservice)
  * [monitorModule](#monitormodule)
- [Convention](#convention)
  * [operation function](#operation-function)
  * [action function](#action-function)
- [Licence](#licence)

<br>

## Install
```shell
npm install @financial-times/n-express-monitor --save
```

## Demo
[next-monitor-express](https://github.com/Financial-Times/next-monitor-express)

## Usage

### setupMonitor
```js
import express, { metrics } from '@financial-times/n-express';
import { setupMonitor } from '@financial-times/n-express-monitor';

const app = express();

setupMonitor({ app, metrics });

// ...middlewares and routes
```

### monitor
```js
import { monitor } from '@financial-times/n-express-monitor';

const getUserProfileBySession = async (req, res) => {
	const { meta } = req;
	const { sessionId } = req.params;
	if (sessionId === 'uncovered') {
		throw Error('an uncovered function has thrown an error');
	}
	const { userId } = await SessionApi.verifySession({ sessionId }, meta);
	const userProfile = await UserProfileSvc.getUserProfileById({ userId }, meta);
	res.json(userProfile);
};

export default monitor(getUserProfileBySession);
```

### monitorService
```js
import { monitorService } from '@financial-times/n-express-monitor';

/*
	SHORTHAND DEFAULT: in case we don't need to add extra error handling,
	the default method from n-api-factory can be used to setup a client method
 */
const getUserProfileById = async ({ userId }, meta) =>
	userProfileSvc.get({
		endpoint: `/user-profile/${userId}`,
		meta,
	});

export default monitorService('user-profile-svc', {
	getUserProfileById,
});
```

### monitorModule
```js
import { monitorModule } from '@financial-times/n-express-monitor';

export default monitorModule({
	validateUserId: () => {},
 mapUserProfileToView: () => {},
});
```

## Convention

### operation function

same as express middleware/controller but without next: `(req, res) => {}`

### action function

`(param, meta) => {}`

## Licence
[MIT](/LICENSE)
