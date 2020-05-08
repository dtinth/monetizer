# TSDX Bootstrap

A monetization meta tag manager.

```jsx
import { monetize } from 'monetizer'
```

## Project goals

- Simple API to integrate with any frontend library or framework.
- Support single page apps.
- Support [probabilistic revenue sharing](https://webmonetization.org/docs/probabilistic-rev-sharing).

## Vanilla JS usage

```jsx
// Starts monetizing
const stopMonetizing = monetize('$dt.in.th')

// Stops monetizing
stopMonetizing()
```

## React usage

```jsx
function App() {
  useEffect(() => monetize('$dt.in.th'), [])
  return <Layout><Main /></Layout>
}
```

## Probabilistic revenue sharing

If multiple monetization calls are active, it uses probabilistic revenue sharing to divide the revenue.

```jsx
// 50% chance
monetize('$dt.in.th')

// 50% chance
monetize('$twitter.xrptipbot.com/bemusegame')
```

You can specify a weight (default weight is 1).

```jsx
// 33% chance
monetize({ content: '$dt.in.th', weight: 1 })

// 67% chance
monetize({ content: '$twitter.xrptipbot.com/bemusegame', weight: 2 })
```

## Prioritization

You can also specify priority. Higher priority wins.

```jsx
function App() {
  // Default priority is 0
  useEffect(() => monetize('$dt.in.th'), [])
  return <Layout><Main /></Layout>
}

function Article(props) {
  const paymentPointer = props.author.paymentPointer

  // Send micropayment to article’s author while viewing author’s article
  useEffect(() => monetize({ content: paymentPointer, priority: 1 }), [])
}
```

Priority ties are probabilistic-revenue-shared.

```jsx
function Article(props) {
  const paymentPointer = props.author.paymentPointer

  // 50-50 revenue sharing between article author and site developer
  useEffect(() => monetize({ content: '$dt.in.th', priority: 1 }), [])
  useEffect(() => monetize({ content: paymentPointer, priority: 1 }), [])
}
```

## Local Development

This project was bootstrapped with [TSDX](https://github.com/jaredpalmer/tsdx).

Below is a list of commands you will probably find useful.

### `yarn start`

Runs the project in development/watch mode. Your project will be rebuilt upon changes. TSDX has a special logger for you convenience. Error messages are pretty printed and formatted for compatibility VS Code's Problems tab.

<img src="https://user-images.githubusercontent.com/4060187/52168303-574d3a00-26f6-11e9-9f3b-71dbec9ebfcb.gif" width="600" />

Your library will be rebuilt if you make edits.

### `yarn build`

Bundles the package to the `dist` folder.
The package is optimized and bundled with Rollup into multiple formats (CommonJS, UMD, and ES Module).

<img src="https://user-images.githubusercontent.com/4060187/52168322-a98e5b00-26f6-11e9-8cf6-222d716b75ef.gif" width="600" />

### `yarn test`

Runs the test watcher (Jest) in an interactive mode.
By default, runs tests related to files changed since the last commit.
