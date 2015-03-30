node-macaddress
===============

Retrieve MAC addresses in Linux, OS X, and Windows.

A common misconception about MAC addresses is that every *host* has a MAC address,
while a host may have multiple MAC addresses – since every network interface may
have its own MAC address.

This library allows to discover the MAC address per network interface and chooses
an appropriate interface if all you're interested in is *one* MAC address identifying
the host system (see `API + Examples` below).

**Features:**

+ works in `Linux`, `Mac OS X`, `Windows`, and on most `UNIX` systems.
+ `io.js` reports MAC addresses in `os.networkInterfaces()`, this library utilizes this information when available.
+ also features a sane replacement for `os.networkInterfaces()` (see `API + Examples` below).

Usage
-----

```
npm install --save node-macaddress
```

```JavaScript
var macaddress = require('./index');
```

API + Examples
--------------

    (async)  .one(iface, callback) → string
    (async)  .one(callback)        → string
    (async)  .all(callback)        → { iface: { type: address } }
    (sync)   .networkInterfaces()  → { iface: { type: address } }

---

### `.one([iface], callback)`

Retrieves the MAC address of the given `iface`.

If `iface` is omitted, this function automatically chooses an
appropriate device (e.g. `eth0` in Linux, `en0` in OS X, etc.).

**Without `iface` parameter:**

```JavaScript
macaddress.one(function (err, mac) {
  console.log("Mac address for this host: %s", mac);  
});
```

```
→ Mac address for this host: ab:42:de:13:ef:37
```

**With `iface` parameter:**

```JavaScript
macaddress.one('awdl0', function (err, mac) {
  console.log("Mac address for awdl0: %s", mac);  
});
```

```
→ Mac address for awdl0: ab:cd:ef:34:12:56
```

---

### `.all(callback)`

Retrieves the MAC addresses for all non-internal interfaces.

```JavaScript
macaddress.all(function (err, all) {
  console.log(JSON.stringify(all, null, 2));
});
```

```JavaScript
{
  "en0": {
    "ipv6": "fe80::cae0:ebff:fe14:1da9",
    "ipv4": "192.168.178.20",
    "mac": "ab:42:de:13:ef:37"
  },
  "awdl0": {
    "ipv6": "fe80::58b9:daff:fea9:23a9",
    "mac": "ab:cd:ef:34:12:56"
  }
}
```

---

### `.networkInterfaces()`

A useful replacement of `os.networkInterfaces()`. Reports only non-internal interfaces.

```JavaScript
console.log(JSON.stringify(macaddress.networkInterfaces(), null, 2));
```

```JavaScript
{
  "en0": {
    "ipv6": "fe80::cae0:ebff:fe14:1dab",
    "ipv4": "192.168.178.22"
  },
  "awdl0": {
    "ipv6": "fe80::58b9:daff:fea9:23a9"
  }
}
```
