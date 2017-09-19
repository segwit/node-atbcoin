# node-atbcoin

node-atbcoin is a simple wrapper for the ATBCoin client's JSON-RPC API.

The methods are exposed as lower camelcase methods on the `atbcoin.Client`
object, or you may call the API directly using the `cmd` method.

## Install

`npm install atbcoin`

## Examples

### Create client
```js
// all config options are optional
var client = new atbcoin.Client({
  host: 'localhost',
  port: 8332,
  user: 'username',
  pass: 'password',
  timeout: 30000
});
```

### Get balance across all accounts with minimum confirmations of 6

```js
client.getBalance('*', 6, function(err, balance, resHeaders) {
  if (err) return console.log(err);
  console.log('Balance:', balance);
});
```
### Getting the balance directly using `cmd`

```js
client.cmd('getbalance', '*', 6, function(err, balance, resHeaders){
  if (err) return console.log(err);
  console.log('Balance:', balance);
});
```

### Batch multiple RPC calls into single HTTP request

```js
var batch = [];
for (var i = 0; i < 10; ++i) {
  batch.push({
    method: 'getnewaddress',
    params: ['myaccount']
  });
}
client.cmd(batch, function(err, address, resHeaders) {
  if (err) return console.log(err);
  console.log('Address:', address);
});
```

## SSL
See [Enabling SSL on original client](https://en.bitcoin.it/wiki/Enabling_SSL_on_original_client_daemon).

If you're using this to connect to bitcoind across a network it is highly
recommended to enable `ssl`, otherwise an attacker may intercept your RPC credentials
resulting in theft of your atbcoins.

When enabling `ssl` by setting the configuration option to `true`, the `sslStrict`
option (verifies the server certificate) will also be enabled by default. It is
highly recommended to specify the `sslCa` as well, even if your atbcoind has
a certificate signed by an actual CA, to ensure you are connecting
to your own atbcoind.

```js
var client = new atbcoin.Client({
  host: 'localhost',
  port: 8332,
  user: 'username',
  pass: 'password',
  ssl: true,
  sslStrict: true,
  sslCa: fs.readFileSync(__dirname + '/myca.cert')
});
```
