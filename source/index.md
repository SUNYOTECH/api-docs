---
title: API Reference

language_tabs:
  - javascript
  - coffeescript
  - php
  - python
  - elixir
  - ruby
  - java
  - go

toc_footers:
  - <a href='https://api-dashboard.stampery.com/signup' target="_blank" class='signup'>Sign up</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction
> You can get the latest version of the **Stampery API** client library for your preferred language on GitHub:

> - Node.js → [https://github.com/stampery/node](https://github.com/stampery/node)
> - PHP     → [https://github.com/stampery/php](https://github.com/stampery/php)
> - Python  → [https://github.com/stampery/python](https://github.com/stampery/python)
> - Ruby    → [https://github.com/stampery/ruby](https://github.com/stampery/ruby)
> - Elixir  → [https://github.com/stampery/elixir](https://github.com/stampery/elixir)
> - Java    → [https://github.com/stampery/java](https://github.com/stampery/java)
> - Go    → [https://github.com/stampery/go](https://github.com/stampery/go)


This is the documentation for version 3 of the **Stampery API**, the industrial-scale data certification platform.

With the Stampery API you can certify a virtually **unlimited** amount of datasets or files by embedding their unique identifiers into both the **Bitcoin** and **Ethereum** public blockchains.

For every certification and blockchain, you get a receipt document called **proof** that allows you to prove the timestamped **existence**, **integrity** and **ownership** of the certified data.

Client libraries are available for **NodeJS**, **PHP**, **Python**, **Ruby**, **Elixir** and **Java**.

You can view code examples across this documentation in the dark area to the right, and switch between different programming languages with the tabs in the top right.

# Authentication
```javascript
stampery = new Stampery('367c6ec2-5791-4cf5-8094-4bae00c639b4');
```
```coffeescript
stampery = new Stampery '367c6ec2-5791-4cf5-8094-4bae00c639b4'
```
```php
<?
$stampery = include('stampery.inc.php');
$stampery = new Stampery('830fa1bf-bee7-4412-c1d3-31dddba2213d');
```
```python
from stampery import Stampery
client = Stampery('2d4cdee7-38b0-4a66-da87-c1ab05b43768', 'prod')
```
```elixir
require Stampery
Stampery.init "2d4cdee7-38b0-4a66-da87-c1ab05b43768"
```
```ruby
require 'stampery'
stampery = Client.new '2d4cdee7-38b0-4a66-da87-c1ab05b43768'
```
```java
import com.stampery.Stampery;

Stampery stampery = new Stampery("2d4cdee7-38b0-4a66-da87-c1ab05b43768");
stampery.subscribe(this);      
stampery.start();
```
```go
events := stampery.Login("2d4cdee7-38b0-4a66-da87-c1ab05b43768")
```

Authentication is performed by using app-specific **secret tokens**.

You can generate your own secret tokens from the [API dashboard](https://api-dashboard.stampery.com).

<aside class="info">As a rule of thumb, you should use a single token for every application using our API. If you are running a distributed application, you should also use a different token for every instance or otherwise you may miss some proofs.</aside>

# Stamping

## Stamping a hash
```javascript
var digest = '0989551C2CCE109F40BE2C8AD711E23A\
539445C93547DFC13D25F9E8147886B8\
D0E71A16FF4DED1CB4BC6AC2E4BBB572\
2F0996B24F79FC849531FE70BB2DE800';

stampery.on('proof', function(hash, proof) {
  console.log('Received proof for hash ' + hash, proof);
});

stampery.hash(digest);
```
```coffeescript
digest = '0989551C2CCE109F40BE2C8AD711E23A539445C93547DFC13D25F9E8147886B8D0E71A16FF4DED1CB4BC6AC2E4BBB5722F0996B24F79FC849531FE70BB2DE800'

stampery.on 'proof', (hash, proof) ->
  console.log "Received proof for hash #{hash}", proof

stampery.stamp digest
```
```php
<?
$digest = 'A69F73CCA23A9AC5C8B567DC185A756E97C982164FE25859E0D1DCC1475C80A615B2123AF1F5F94C11E3E9402C3AC558F500199D95B6D3E301758586281DCD26';
$stampery->stamp($digest);
```
```python
digest = "0989551C2CCE109F40BE2C8AD711E23A539445C93547DFC13D25F9E8147886B8D0E71A16FF4DED1CB4BC6AC2E4BBB5722F0996B24F79FC849531FE70BB2DE800"
client.stamp(digest)
```
```elixir
digest = "0989551C2CCE109F40BE2C8AD711E23A539445C93547DFC13D25F9E8147886B8D0E71A16FF4DED1CB4BC6AC2E4BBB5722F0996B24F79FC849531FE70BB2DE800"
Stampery.stamp digest
```
```ruby
digest = "0989551C2CCE109F40BE2C8AD711E23A539445C93547DFC13D25F9E8147886B8D0E71A16FF4DED1CB4BC6AC2E4BBB5722F0996B24F79FC849531FE70BB2DE800"
stampery.stamp digest
```
```java
String digest = "0989551C2CCE109F40BE2C8AD711E23A539445C93547DFC13D25F9E8147886B8D0E71A16FF4DED1CB4BC6AC2E4BBB5722F0996B24F79FC849531FE70BB2DE800";
stampery.stamp(digest);
```
```go
digest := "0989551C2CCE109F40BE2C8AD711E23A539445C93547DFC13D25F9E8147886B8D0E71A16FF4DED1CB4BC6AC2E4BBB5722F0996B24F79FC849531FE70BB2DE800"
stampery.Stamp(digest)
```

Our API is capable of stamping **SHA3-512** hashes directly.

For stamping a SHA3-512 hash, you only have to make a call to the ``stamp`` method of our API, as seen in the examples to the right.

<aside class="info"><strong>SHA3</strong>, a subset of <strong>Keccak</strong>, is the last version of the SHA family of cryptographic hash functions.<br />Our API uses a <strong>512</strong> bits output size, so the hexadecimal digest of all hashes will have a length of 128 characters.</aside>

## Stamping a file
```javascript
stampery.on('proof', function(hash, proof) {
  console.log('Received proof for hash ' + hash, proof);
});

fs.readFile('/etc/hosts', 'utf8', function(err, data) {
  stampery.hash(data, stampery.stamp);
});
```
```coffeescript
stampery.on 'proof', (hash, proof) ->
  console.log "Received proof for hash #{hash}", proof

fs.readFile '/etc/hosts', 'utf8', (err, data) ->
  stampery.hash data, stampery.stamp
```
```php
<?
$stampery.on('proof', function(hash, proof){
 echo("Received proof for hash" . $hash . "\n");
 echo("Protocol version: " . $proof[0] . "\n");
 echo("Merkle siblings:"\n");
 var_dump($proof[1]);
 echo("Merkle root: " . $proof[2] . "\n");
 echo("Blockchain: " . array('Bitcoin', 'Ethereum')[$proof[3][0]] . "\n");
 echo("Transaction ID: " . $proof[3][1] . "\n");
});

$file = file_get_contents('/path/to/file.txt');
$digest = $stampery->hash($file);
$stampery->stamp($digest);
```
```python
def on_proof(hash, proof):
    print("Received proof for")
    print(hash)
    print("Proof")
    print(proof)

file = open("/path/to/file.txt")
digest = client.hash(file.read())
client.stamp(digest)
```
```elixir
Stampery.on :proof, fn [hash, proof] ->
    IO.puts "\nReceived proof for \n#{hash} \n\nProof"
    IO.inspect proof
end

{:ok, data} = File.read "/path/to/file.txt"
digest = Stampery.hash data
Stampery.stamp digest
```
```ruby
stampery.on :proof do |hash, proof|
  puts 'Received proof for'
  puts hash
  puts 'Proof'
  puts proof.to_s
end

data = File.read "/path/to/file.txt"
digest = stampery.hash data
stampery.stamp digest
```
```java
// the following code should be inside a class that implements our Consumer interface
public void onProof(String hash, Proof proof) {
  System.out.println(hash);
  System.out.println(proof);
}

public void onReady() {
  String file = "";
  try{
  	file = new String(Files.readAllBytes(Paths.get("/path/to/file.txt")));
  }catch(IOException e){
  	e.printStackTrace();
  }

  String digest = stampery.hash(file);
  stampery.stamp(digest);
}
```
```go
for event := range events {
  switch event.Type {
  case "ready":
    data, err := ioutil.ReadFile("/path/to/file.txt")
    if err != nil {
    	log.Fatalf("Error %v\n", err)
    }
    digest := stampery.Hash(string(data))
    stampery.Stamp(digest)

  case "proof":
  	fmt.Println("\nProof")
  	p := event.Data.(stampery.Proof)
  	fmt.Println("Hash: ", p.Hash)
  	fmt.Printf("Version: %v\nSiblings: %v\nRoot: %v\n", p.Version, p.Siblings, p.Root)
  	fmt.Printf("Anchor:\n  Chain: %v\n  Tx: %v\n", p.Anchor.Chain, p.Anchor.Tx)

  case "error":
  	log.Fatalf("%v\n", event.Data)
  }
}
```

Stamping a file is just as easy as obtaining its **SHA3-512** hash and then stamping the hash as explained in the previous section.

For your convenience, all our client libraries include a function for hashing files as seen in the examples to the right.

# Proofs

## About proofs

Proofs are **JSON** documents proving that a certain file or dataset has been anchored to a certain public blockchain.

Anyone holding the original data and its corresponding proof can verify the certification by following the proving process described <a href="#checking-a-proof-proving">here</a>.

## Proof format
This is the structure of a proof:

<aside class="info">[v, s, r, [c, t]]</aside>

Where `v`, `s`, `r`, `c` and `t` are:

| Key | Description | Value type |
|-----|--------------------------------------|-------------|
| v | BTA version used. | Integer(1) |
| s | Merkle siblings. | Array |
| r | Merkle root. | String(128) |
| c | Chain code. | Integer(1) |
| t | Transaction ID (txid) | String(64) |

Valid chain codes for ``c`` are:

| Value | Chain |
|-------|-------|
| 1 | Bitcoin livenet |
| 2 | Ethererum livenet |

## Retrieving missed proofs
```javascript
stampery.on('proof', function(hash, proof) {
  console.log('Received missed proof for hash ' + hash, proof);
});

stampery.receiveMissedProofs();
```
```coffeescript
stampery.on 'proof', (hash, proof) ->
  console.log "Received missed proof for hash #{hash}", proof

stampery.receiveMissedProofs()
```
If you make a stamp and for some reason disconnect from our service right after, you will miss the proof.

Don't worry, **missed proofs are queued in our servers for 24h**, and you can retrieve them by making a simple call to the ``receiveMissedProofs`` method.

## Checking a proof (proving)
```javascript
var mix = function (a, b) {
  if (b > a){
    [a, b] = [b, a];
  }
  sha3 = new (SHA3.SHA3Hash)();
  sha3.update(data);
  return sha3.digest('hex').toUpperCase();
}

var prove = function (hash, siblings, root) {
  if (siblings.length > 0) {
    head = siblings[0];
    tail = siblings.slice(1);
    hash = mix(hash, head);
    return prove(hash, tail, root);
  } else {
    return hash == root
  }
}

console.log(prove(digest, s, r) && 'Valid!' && 'Wrong!');
```
```coffeescript
mix = (a, b) ->
  if b > a
    [a, b] = [b, a]
  sha3 = new (SHA3.SHA3Hash)
  sha3.update data
  sha3.digest('hex').toUpperCase()

prove = (hash, siblings, root) ->
  if siblings.length > 0
    head = siblings[0]
    tail = siblings.slice(1)
    hash = mix(hash, head)
    prove hash, tail, root
  else
    hash == root

console.log prove(digest, s, r) and 'Valid!' and 'Wrong!'
```
We call **proving** to the process of verifying a proof is valid and therefore demonstrating that a hash or file has been undoubtfully embedded in the blockchain and it has not been tampered since then.

Proofs are independently verifiable by everyone in possesion of the hash or file and the proof itself.

The process of proving Stampery proofs made with the current version 5 of our BTA platform is made like this:

1. Take the hash
2. Take first element in the Merkle siblings list `s[0]`
3. Concatenate both, putting the biggest of them in front
4. Hash the resulting string with `SHA3-512`
5. Take the resulting hash and the next element in the Merkle siblings list, concatenate like before and hash the resulting string
6. Repeat same process until there are no elements left in the Merkle siblings list
7. Check the resuting string matches the Merkle root of the proof, `r`
8. Take the transaction ID, `t`, and paste in your favorite Bitcoin/Ethereum chain explorer
9. Check that the Merkle root, `r`, matches the tail of the data embedded in the transaction

If the checks in steps **7** and **9** pass, then you have proved that the proof is OK and the hash or file has not been tampered.
