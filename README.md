
# Beam by Cion Studio

Lightweight wrapper around the browser's fetch API. Can be configured to automatically assemble full URLs and attach authorization tokens.

## Requirements
- npm and NodeJS
	- Download and Install nodeJs
    - [https://nodejs.org/en/](https://nodejs.org/en/)


## Usage in the browser
You can configure Beam to use a default base URL. Any request starting with a
"/" will have the base URL appended to it. 

You can also configure Beam to use a JWT with every request by providing a token builder function.
The example below uses a firebase auth JWT. 

```
// configuredBeam.ts

import firebase from 'firebase/app'
import 'firebase/auth'
import Beam from 'browser-beam'

async function tokenBuilder(): Promise<string> {
	const token = await firebase.auth().currentUser?.getIdToken().catch(() => '')
	return token || ''
}

const beam = new Beam({
	tokenBuilder: tokenBuilder,
	urlPrefix: 'localhost:5000'
})

export default beam
```

Now that Beam is configured we can use it like this. This will make a POST request 
to a server hosted on localhost:5000 with the given body. The JWT will be
automatically generated from firebase auth as well. 
```
import beam from './configuredBeam'
beam.post('/api/accounts'{
    email: 'info@cionstudio.com'
    name: 'Cion Studio'
})
```

## Usage with Node

Since Node doesn't have a native fetch object, we have to provide one. You can use
node-fetch for this. 

```npm i node-fetch```

Now we create a custom Beam class that uses this dependency. 
```
import beamCreator from 'browser-beam/beamCreator'
import fetchHandler from 'node-fetch'

const Beam = beamCreator(fetchHandler)

const beam = new Beam()

beam.get('https://api.kanye.rest/').then(console.log).catch(console.error)
```


## Development

#### Recommended VS Code extensions:
* ES Lint