# Instant Widget

![AirSwap Widget](../assets/widget/instant-widget.png)

The AirSwap Instant Widget is an embeddable, HTML+JavaScript element that can be dropped into any webpage and be used to easily buy or sell Ethereum ERC20 tokens. The Widget is designed to provide instant access to liquidity for DEX aggregators, utility token-based dApps, and more.

!> Pop-up blockers can prevent the AirSwap Widget from loading properly.

### Setup

The following example will render a button that opens a Widget with a request to buy AST. You can try it out here: [JSFiddle](https://codepen.io/grahamperich/pen/xxKqBQy).

```html
<head>
  <script src="https://cdn.airswap.io/airswap-instant-widget.js"></script>
</head>
```

```js
AirSwapInstant.render(
  {
    env: 'production',
    mode: 'buy',
    token: '0x4e15361fd6b4bb609fa63c81a2be19d873717870',
    amount: '250',
    onClose: function() {
      console.info('Trade was canceled.')
    },
    onComplete: function(transactionId) {
      console.info('Trade complete.', transactionId)
    },
  },
  'body',
)
```

<button class="open-widget" id="open-instant-widget-1" onClick="(function() {
  const button = document.getElementById('open-instant-widget-1');
  button.disabled = true;
  window.AirSwapInstant.render(
    {
      env: 'production',
      mode: 'buy',
      token: '0x4e15361fd6b4bb609fa63c81a2be19d873717870',
      amount: '250',
      onClose: function() {
        console.info('Trade was canceled.')
        button.disabled = false;
      },
      onComplete: function(transactionId) {
        console.info('Trade complete.', transactionId)
      },
    },
    'body',
  )
})()">Try it out</button>

## Options

The simplest way to use the `AirSwapInstant` widget is by rendering it without any custom configuration options. This will open the widget and allow the user to buy or sell any amount of any token.

```js
AirSwapInstant.render(
  {
    onClose: function() {
      console.info('Trade was canceled.')
    },
  },
  'body',
)
```

<button class="open-widget" id="open-instant-widget-2" onClick="(function() {
  const button = document.getElementById('open-instant-widget-2');
  button.disabled = true;
  window.AirSwapInstant.render(
    {
      onClose: function() {
        console.info('Trade was canceled.')
        button.disabled = false;
      },
    },
    'body',
  )
})()">Try it out</button>

<br>

### The configuration object accepts the options described below {docsify-ignore}

#### env `string`, `optional`

Either `development` or `production`. If not specified, this option will default to `production`. Using `production` will request orders for the main Ethereum network, whereas using `development` will request orders for the Rinkeby test network.

#### mode `string`, `optional`

Either `buy` or `sell`. If specified, the user will not be able to change the mode.

#### token `string`, `optional`

The hex address of the token to swap in exchange for ETH. You can find a full list of indexed token metadata for: [Mainnet](https://token-metadata.airswap.io/tokens) or [Rinkeby](https://token-metadata.airswap.io/rinkebyTokens). If specified, the user will not be able to search for any other tokens in the widget.

!> If you pass a hex address that is not included in AirSwap token metadata, the widget will not work.

#### baseToken `string`, `optional`

Either `ETH`, `WETH`, `DAI`, or `USDC`. Defaults to `ETH` when not specified. This is the asset that will determine the price in the checkout. For example, if DAI is `baseToken` and AST is `token`, then 2 DAI for 100 AST would be displayed as `0.02 AST/DAI`

#### amount `string`, `optional`

A default amount of tokens to request orders for. If specified, the user will not be able to change the token amount in the widget.

#### makerAddress `string`, `optional`

A maker's Ethereum address. When this parameter is specified, the widget will only query the specified maker for orders instead of reaching out to all online makers.

#### onComplete `function`, `optional`

Called when the user submits the trade transaction to the blockchain. The transaction ID is passed as an argument.

```js
function onComplete(transactionId) {
  console.log('Complete!', transactionId)
}
```

#### onClose `function`, `required`

This is the only mandatory parameter. A function called when the user clicks the "X" to dismiss the widget. No arguments are passed.

```js
function onClose() {
  console.log('Canceled!')
}
```

## Options {docsify-ignore}

| Key            | Type     | Field          | Description                                                                                                                                                                                                                                                                                                                                             |
| -------------- | -------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `env`          | string   | `optional`     | Either `development` or `production`. If not specified, this option will default to `production`. Using `production` will request orders for the main Ethereum network, whereas using `development` will request orders for the Rinkeby test network. You can get test AST on Rinkeby from the [faucet](https://ast-faucet-ui.development.airswap.io/). |
| `mode`         | string   | `optional`     | Either `buy` or `sell`. If specified, the user will not be able to change the mode.                                                                                                                                                                                                                                                                     |
| `token`        | string   | `optional`     | The hex address of the token to swap in exchange for `baseToken`. You can find a full list of indexed token metadata for: [Mainnet](https://token-metadata.airswap.io/tokens) or [Rinkeby](https://token-metadata.airswap.io/rinkebyTokens). If specified, the user will not be able to search for any other tokens in the widget.                      |
| `baseToken`    | string   | `optional`     | Either `ETH`, `WETH`, `DAI`, or `USDC`. Defaults to `ETH` when not specified.                                                                                                                                                                                                                                                                           |
| `amount`       | string   | `optional`     | A default amount of tokens to request orders for. If specified, the user will not be able to change the token amount in the widget.                                                                                                                                                                                                                     |
| `makerAddress` | string   | `optional`     | A maker's Ethereum address. When this parameter is specified, the widget will only query the specified maker for orders instead of reaching out to all online makers.                                                                                                                                                                                   |
| `onComplete`   | Function | `optional`     | Called when the user submits the trade transaction to the blockchain. The transaction ID is passed as an argument.                                                                                                                                                                                                                                      |
| `onClose`      | Function | **`required`** | This is the only mandatory parameter. A function called when the user clicks the "X" to dismiss the widget. No arguments are passed.                                                                                                                                                                                                                    |

<!-- Coming soon...

#### address `string`, `optional`

A fixed maker `address` to query a specific counterparty for orders. -->
