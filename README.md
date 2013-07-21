# SDP Parser [![Build Status](https://secure.travis-ci.org/clux/sdp-parser.png)](http://travis-ci.org/clux/sdp-parser)
A simple parser of SDP. Defines internal regexes based on [RFC4566](http://tools.ietf.org/html/rfc4566) (SDP) and [RFC5245](http://tools.ietf.org/html/rfc5245) (ICE), and will force values that are integers to integers and leave everything else as strings. Simple to extend or build upon.


## Usage
Require it and pass it an unprocessed SDP string.

```js
var parse = require('sdp-parser');

var sdp = "v=0\n\
o=- 20518 0 IN IP4 203.0.113.1\n\
s=\n\
t=0 0\n\
c=IN IP4 203.0.113.1\n\
a=ice-ufrag:F7gI\n\
a=ice-pwd:x9cml/YzichV2+XlhiMu8g\n\
a=fingerprint:sha-1 42:89:c5:c6:55:9d:6e:c8:e8:83:55:2a:39:f9:b6:eb:e9:a3:a9:e7\n\
m=audio 54400 RTP/SAVPF 0 96\n\
a=rtpmap:0 PCMU/8000\n\
a=rtpmap:96 opus/48000\n\
a=ptime:20\n\
a=sendrecv\n\
a=candidate:0 1 UDP 2113667327 203.0.113.1 54400 typ host\n\
a=candidate:1 2 UDP 2113667326 203.0.113.1 54401 typ host\n\
"

var res = parse(sdp);

res;
{ origin: 
   { username: '-',
     sessionId: 20518,
     sessionVersion: 0,
     netType: 'IN',
     ipVer: 4,
     address: '203.0.113.1' },
  connection: { version: 4, ip: '203.0.113.1' },
  iceUfrag: 'F7gI',
  icePwd: 'x9cml/YzichV2+XlhiMu8g',
  fingerprint: 
   { type: 'sha-1',
     hash: '42:89:c5:c6:55:9d:6e:c8:e8:83:55:2a:39:f9:b6:eb:e9:a3:a9:e7' },
  media: 
   [ { rtp: [Object],
       fmtp: [],
       type: 'audio',
       port: 54400,
       protocol: 'RTP/SAVPF',
       payloads: [Object],
       ptime: 20,
       sendrecv: 'sendrecv',
       candidates: [Object] } ] }

sdp.media[0];
{ rtp: 
   [ { payload: 0,
       codec: 'PCMU',
       rate: 8000 },
     { payload: 96,
       codec: 'opus',
       rate: 48000 } ],
  fmtp: [],
  type: 'audio',
  port: 54400,
  protocol: 'RTP/SAVPF',
  payloads: [ 0, 96 ],
  ptime: 20,
  sendrecv: 'sendrecv',
  candidates: 
   [ { foundation: 0,
       component: 1,
       transport: 'UDP',
       priority: 2113667327,
       ip: '203.0.113.1',
       port: 54400,
       type: 'host' },
     { foundation: 1,
       component: 2,
       transport: 'UDP',
       priority: 2113667326,
       ip: '203.0.113.1',
       port: 54401,
       type: 'host' } ] }

```

In this example, only slightly dodgy string coercion case here is for `candidates[i].foundation`, which can be a string, but in this case can be equally parsed as an integer.

## Installation

```bash
$ npm install sdp-parser
```

## License
MIT-Licensed. See LICENSE file for details.
