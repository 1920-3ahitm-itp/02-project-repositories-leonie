# Meeting

### Datum: 06. Mai 2020

### TeilnehmerInnen: Ahammer Fabian, Brandmair Stefan, Weissengruber Nina, Wiesinger Jonas

e2e Tests sind Tests auf das gerenderte
e2e Tests sind headlessclients
Ein headlessclient ist ein Browser der keine Anzeige hat

Nuxtjs e2e Test
<br>
https://nuxtjs.org/guide/development-tools/
<br>
Nuxt js testing ist in  ```AVA```

**Aufbau:**

```
Imports -> setup (startet dev server) -> Tests -> closeup
```

Ein SmokeTest schaut kurz ob etwas auf der Seite läuft.

```json
"scripts": {
 "test": "ava"
  },
  "ava": {
    "files": [
      "test/**/*"
    ]
  },

```

```javascript
const { resolve } = require('path');
const test = require('ava');
const { Nuxt, Builder } = require('Nuxt');

// Init Nuxt.js and start listening on localhost:4000
test.before('Init Nuxt.js', async (t) => {
    const rootDir = resolve(__dirname, '..');
    let config = {};
    try { config = require(resolve(rootDir, 'nuxt.config.js')) } catch (e) { }
    config.rootDir = rootDir // project folder
    config.dev = false // production build
    config.mode = 'universal' // Isomorphic application
    const nuxt = new Nuxt(config)
    t.context.nuxt = nuxt // We keep a reference to Nuxt so we can close the server at the end of the test
    await new Builder(nuxt).build()
    nuxt.listen(3300, 'localhost')
})

// Example of testing only generated html
test.default('Route / exists and render HTML', async (t) => {
    const { nuxt } = t.context
    const context = {}
    const { html } = await nuxt.renderRoute('/', context)
    t.true(true)
})

// Close the Nuxt server
test.after('Closing server', (t) => {
    const { nuxt } = t.context
    nuxt.close()
})
```
