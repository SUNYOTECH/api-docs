---
title: API Reference

language_tabs:
  - php
  - python
  - javascript

toc_footers:
  - <a href='https://stampery.com/signup'>Sign up to Stampery</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Stampery API! When you register you'll be able to see, create and delete API keys in your user panel.

We have language bindings in Shell, PHP, Python and JavaScript! (Ruby coming soon, feel free to start one if you want!) You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

## Authenticate via API key

> To authenticate, use this code:

```php
$stampery = include('stampery.inc.php');
$stampery = new Stampery('stamperystampery');
```

```python
import stampery
s = stampery.Client('stamperystampery')
```

```javascript
var Stampery = require('stampery'),
    stampery = new Stampery('stamperystampery')
```

> Make sure to replace `stamperystampery` with your client secret.

Using your secret token, you can use it to authenticate using the libraries

## Authenticate via username and password

### HTTP Request

> JSON returned by this API endpoint

```json
{
  "api_token": {
    "name": "Token Name",
    "clientId": "Client ID",
    "secretToken": "Secret Token"
  }
}
```

`POST https://stampery.com/api/v2/login`

### JSON body

Parameter | Description
--------- | -----------
username | Your username
password | Your password
otp | In case your account has an OTP token set, put it here

## Authenticate via HTTP header

The Stampery API uses a token pair with a client ID and a client secret. You can authenticate with just the client secret (as seen in the PHP, Python and Node libraries) and username & password. The method used is by sending an HTTP Authorization header with the client ID and the client secret, in the next way:

`Authorization: client_id:client_secret`

<aside class="notice">
You must replace <code>client_id:client_secret</code> with your key and secret.
</aside>

# Stamps

## Retrieve a specific stamps 

```php
$stampery->get($hash);
```

```python
s.get(hash)
```

```javascript
stampery.get(hash, function(err, stamp) { })
```

> The above command returns JSON structured like this:

```json
{
  "v": 3,
  "data": {
    "owners": [
      {
        "email": "owner1@email.com",
        "name": "Owner 1"
      }
    ],
    "size": "1024",
    "name": "ImportantContract.pdf",
    "hash": "c65e0cc54186376e5026e16c5c59f8ef69dd745d29fc743b8033b6bcd1fc88d6"
  },
  "proof": {
    "hash": "e6f064f4f5b5041d24e8e43a5bba7e81367d435709ba473c578dc2b4b1b8ddb0",
    "merkleIndex": 0,
    "merkleSiblings": [
      "7dc47952c0bc99f61ee863018f52711d0ac4031afb1578db6cc3e9c990d1172b",
      "0fff49490d879d809cb1f57972beb9f3293f7509cd8bd4eb02986a0eca4adb10",
      "eea53b2e51b72781c95e87f3c98741ca1bc1196bd7bf700c3fb06ebce1af3382",
      "fef2371e7183475b7b181459d831a62380f4c03d4ff38b5e216ea7d3c3f44f13",
      "9eba1796584f80b7f22b648e37423f6bceeb4d06a278b8981c34ef09e1cfe57a",
      "5604dc31a496b7cf0cbcf7b5d22a6a3f5f23b70bbfa681b0c53214f8fea3d913",
      0
    ],
    "merkleRoot": "fe4354290480da434e25b3f614bc9953ed41b3fd2799db821b1f1317323ea2ba",
    "txid": "31f410d6843496300382a6b001a482e21d20f111bf781bd695d8ffe0e46cd4b8"
  }
}
```

This endpoint retrieves a stamp providing the hash.

### HTTP Request

`GET https://stampery.com/api/v2/stamps/:hash`

## Stamp data

```php
$data = array( 'str' => 'Create a proof of this using the blockchain' );

$stampery->stamp($data);
```

```python
s.stamp({'str': 'Create a proof of this using the blockchain'})
```

```javascript
var data = { str: 'Create a proof of this using the blockchain' }

stampery.stamp(data, function(err, hash) { })
```

> The above command returns a JSON object with the proof's hash:

```json
{
  "hash": "e6f064f4f5b5041d24e8e43a5bba7e81367d43709ba473c578dc2b4b1b8ddb0"
}
```

<aside class="warning">In order to stamp you need to be authenticated with a client secret key</aside>

### HTTP Request

`POST https://stampery.com/api/v2/stamps`

### JSON body

Parameter | Description
--------- | -----------
data | The data to stamp (totally arbitrary as long as it's valid JSON)

## Stamp a file

```php
$data = array();
$file = fopen('document.txt', 'r');

$stampery->stamp($data, $file);
```

```python
file = open('/Users/user/Desktop/Artboard 2.png', 'rb')

s.stamp({'meta': 'data'}, file)
```

```javascript
var data = {}
var file = fs.createReadStream('document.txt')

stampery.stamp(data, file, function(err, hash) { })
```

> The above command returns a JSON object with the proof's hash:

```json
{
  "hash": "e6f064f4f5b5041d24e8e43a5ba7e81367d435709ba473c578dc2b4b1b8ddb0"
}
```

<aside class="warning">In order to stamp you need to be authenticated with a client secret key</aside>

### HTTP Request

`POST https://stampery.com/api/v2/stamps`

This will be a multipart form, sending the file as the `file` parameter:

Parameter | Description
--------- | -----------
data | Extra metadata for the file (not needed to stamp a file)
