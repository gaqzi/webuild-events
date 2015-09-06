#webuild-events

> Get a list of free developer-related events in your city from facebook, meetup.com, eventbrite, ics url and manual entries.

##install

```sh
npm i webuildorg/webuild-events
```

##usage

1. create a `.env` file to store all the environment variables:

	```sh
	NODE_ENV=development
	LOCATION=Singapore # name of city
	WEBUILD_API_SECRET=secret # any random string - used as a password when remotely refreshing the feeds
	MEETUP_API_KEY=secret # generate it from https://secure.meetup.com/meetup_api/key/ to get meetup.com events
	EVENTBRITE_TOKEN=secret # generate it from http://developer.eventbrite.com/ to get eventbrite events

	# Create an Auth0 https://auth0.com/ account and a Facebook app and link them (https://docs.auth0.com/facebook-clientid). Configure the `WEBUILD_AUTH0_CLIENT_*` environment variables.`
	WEBUILD_AUTH0_CLIENT_ID=secret
	WEBUILD_AUTH0_CLIENT_SECRET=secret
	```
- create a file `config.js` with the following contents:

	```js
	var city = 'Singapore'; // name of your city E.g. 'Sydney'
	var country = 'Singapore'; // name of your country E.g. 'Australia'
	var locationSymbol = 'SG'; // country code: https://en.wikipedia.org/wiki/ISO_3166-1#Officially_assigned_code_elements

	module.exports = {
	  location: city,
	  city: city,
	  country: country,
	  symbol: locationSymbol,

	  api_version: 'v1',

	  displayTimeformat: 'DD MMM YYYY, ddd, h:mm a', // format date: http://momentjs.com/docs/#/displaying/
	  dateFormat: 'YYYY-MM-DD HH:mm Z', // format time: http://momentjs.com/docs/#/displaying/
	  timezone: '+0800', // timezone UTC offset: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
	  timezoneInfo: 'Asia/Singapore', // timezone name: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones

	  debug: process.env.NODE_ENV === 'development',

	  ignoreWordsInDuplicateEvents: [
	    'meetup', 'group', 'event',

	    'centre', 'center', 'tower', 'road',
	    'avenue', 'ave',
	    'building', 'city',
	    'jalan', 'jln',
	    'lane', 'ln',
	    'street', 'st',
	    'plaza', 'town', 'new',
	    'level', 'floor',

	    'first',
	    'second',
	    'third',

	    'jan', 'january',
	    'feb', 'february',
	    'mar', 'march',
	    'apr', 'april',
	    'may',
	    'jun', 'june',
	    'jul', 'july',
	    'aug', 'august',
	    'sep', 'sept', 'september',
	    'oct', 'october',
	    'nov', 'november',
	    'dec', 'december',
	    '-',

	    'mon', 'monday',
	    'tue', 'tues', 'tuesday',
	    'wed', 'wednesday',
	    'thu', 'thurs', 'thursday',
	    'fri', 'friday',
	    'sat', 'saturday',
	    'sun', 'sunday',

	    'topic', 'create', 'talk', 'session', 'workshop', 'tell', 'share', 'coding', 'venue', 'about',

	    'speaker', 'member',

	    'a', 'i', 'will', 'be', 'who', 'want', 'or', 'have', 'if', 'go', 'of', 'with', 'from', 'for',

	    'the', 'others', 'another', 'all',

	    'your', 'you', 'our', 'you\'re', 'we\'re'
	  ],

	  auth0: {
	    domain: 'webuildsg.auth0.com',
	    clientId: process.env.WEBUILD_AUTH0_CLIENT_ID,
	    clientSecret: process.env.WEBUILD_AUTH0_CLIENT_SECRET
	  },

	  meetupParams: {
	    key: process.env.MEETUP_API_KEY,
	    country: locationSymbol,
	    state: locationSymbol,
	    city: city,
	    category_id: 34, // Tech category
	    page: 500,
	    fields: 'next_event',

	    blacklistGroups: [],
	    blacklistWords: [],
	  },

	  eventbriteParams: {
	    token: process.env.EVENTBRITE_TOKEN,
	    url: 'https://www.eventbriteapi.com/v3/events/search',
	    categories: [
	      '102',
	      '119'
	    ],
	    blacklistOrganiserId: []
	  }
	};

	```
- create `index.js`:

	```js
	require('dotenv').load();
	var config = require('./config');
	var events = require('webuild-events').init(config).events;

	setTimeout(function() {
	  console.log('Found ' + events.feed.events.length + ' events from Facebook:')
	  console.log('\nMeta info:')
	  console.log(events.feed.meta)
	  console.log('\nFirst event info:')
	  console.log(events.feed.events[0])
	}, 30000);
	```
- run the file with `node index.js`

#contribute

Please see `CONTRIBUTING.md` for details.

#versioning

Every production code has a version following the [Semantic Versioning guidelines](http://semver.org/). Run the `grunt bump`, `grunt bump:minor` or `grunt bump:major` command to bump the version accordingly and then push to production with `git push production master`.

#license

webuild-events is released under the [MIT License](http://opensource.org/licenses/MIT).
