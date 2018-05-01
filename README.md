# truffle-compiler
Compiler helper and artifact manager

It's possible to configure truffle to use:
+ any solc-js version listed at [solc-bin](http://solc-bin.ethereum.org/bin/list.json)
+ a natively compiled solc binary
+ dockerized solc from one of images published [here](https://hub.docker.com/r/ethereum/solc/tags/).

### Listing available versions at the command line
```shell
$ truffle compile --list                   # Recent stable releases from solc-bin (JS)
$ truffle compile --list prereleases       # Recent prereleases from solc-bin (JS)
$ truffle compile --list docker            # Recent docker tags from hub.docker.com
$ truffle compile --list releases --all    # Complete list of stable releases.
```

### Specifying a solcjs version in `truffle.js`
```javascript
module.exports = {
  networks: {
    ... etc ...
  },
  compiler: {
     solc: <string>       // Version. ex:  "0.4.20". (Default: truffle's installed solc)
  }
};
```

### Using docker / natively compiled binaries

These run **a lot** faster than solcjs. `openzeppelin-solidity` (107 contracts) compiles twice as fast.

```javascript
// Native
compiler: {
  solc: "native"
}

// Docker
compiler: {
  solc: "0.4.22",   // Any published image name
  docker: true
}
```

Solc docker images should be installed locally by running:
```shell
$ docker pull ethereum/solc:0.4.22  // Example image
```

For help installing a natively built compiler, see the Solidity docs [here](https://solidity.readthedocs.io/en/v0.4.23/installing-solidity.html#binary-packages).