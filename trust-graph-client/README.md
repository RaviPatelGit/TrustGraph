# a flutter client to trade BTC or USDT peer to peer.

An alternative to p2p telegram groups where every one is admin and there is no group owner.

- Users can give "trust" badges with expiry.

- Users can see list of "trusting" and "trusted by". Similar to twitter where I can see list of "following" and "followed by". Defalut choice for expiration of trust is 1 year or 10 years.

Learn about nostr and nostr event at https://nostr.how/en/the-protocol and at https://github.com/nostr-protocol/nips/blob/master/01.md 


 
## User A "Trusting" user B with expiry time 1 year:
Event structure:
``` 
    {
        "kind": 1,
        "tags": [
            ["p", "<pubkey of the user B>"],
            ["t", "trust"],
            ["expiration", "unix timestamp"],
            // ...
        ],
        "content": "",
        // ..
    }
```

## User A requesting the list of users trusted by user B:
Event structure: 
```
    {
        "authors": [<pubkey of user B>],
        "kinds": [1],
        "#t": ["trust"],
    }
```

## User A requesting the list of users who is trusting user B:
Event structure:
```
   {
        "kinds": [1],
        "#t": ["trust"],
        "#p":[<pubkey of user B>],
    }
```
## User A creating a offer visible to everyone user A is trusting:

Event kind - '4'
Send below direct messase for each user b trusted by user A.
```
    {
        pubkey: <pubkey of user A>,
        created_at: Math.floor(Date.now() / 1000),
        kind: 4,
        tags: [['p', <PublicKey of user b>]],
        content: encryptedMessage + '?iv=' + ivBase64
    }
```
Find more about ivBase64 at https://github.com/nostr-protocol/nips/blob/master/04.md

Plain text is stringified json of the offer content.


## User A creating a offer visible to everyone user A is trusting and everyone trusted by them:

## User A requesting all the offers which are made visible to A

each b1, b2, ... are from list of user trusting A
```
   {
        "authors":[[<pubkey of user b1>,<pubkey of user b2>,...]]
        "kinds": [4],
        "#p":[<pubkey of user A>],
    }
```
## User A requesting all the offers created by A
```
   {
        "authors":[<pubkey of user A>]
        "kinds": [4],
        "#p":[<pubkey of user A>],
    }
```
