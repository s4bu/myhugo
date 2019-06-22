---
title: "Nodejs Mutation Based Fuzzer"
tags: [ "Security Testing", "0-day", "Fuzzing", "NodeJS"]
date: 2017-12-21
cover: "/media/mutation-based-fuzzer.gif"
---

## Mutation Fuzzers (AKA Dumb Fuzzers :wink:)

Mutation Fuzzers are all about mutating the existing input values (blindly). That’s why it is known as “dumb” fuzzers, as it lacks understanding of the complete format/structure of the data. One example of data mutation can be just replacing/appending a random section of data.
Some methods used by mutation fuzzers to generate the data are:

1. Bit flipping
2. Random postfix
3. Random prefix
4. encoding disruption

We will be looking at one of the mutation based fuzzer written in NodeJS today. This does not though qualify to be completely dumb fuzzer :sunglasses:.
Surku is a mutation-based general purpose fuzzer, written in JavaScript. Surku runs on Node.js platform. Being based on Nodejs it provides very good run speed :rocket: with many async features and also understand the boundaries of data formats like XML.
It can be used for any interface like CLI, web OR thin clients. In some case it may require you to create pipe or stream to pump in your data to right place to cause fault. For example suppose there is a web client application that need to be fuzzed and it uses XML data format for communication. In this case we need a :smiling_imp: web server. Let try to create one such server in nodeJS only.

```javascript
var fs = require('fs');  
var http = require('http');
var server = http.createServer(function (req, res) {
  //Read stream is containing our mutated fuzzer data
  var rstream = fs.createReadStream('existFile');
  rstream.pipe(res);
});
server.listen(666, '127.0.0.1');  

```  
Similary for each API we can route format specific fuzzer data to test the application for any existing vulnerability.




**XML Mutation Sample**

![ Mutation Fuzzer NodeJS](/media/Mutationfuzzer.gif)


##### **References**:
1. [GitHub](https://github.com/attekett/Surku)



