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

### Using docker / natively built binaries

```javascript
// Native binary
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

### Speed comparison
Docker and native binary compilers process large contract sets much faster than solcjs. However, if you're just compiling a few contracts at a time, the speedup isn't significant relative to the overhead of running a command (see below). The first time truffle uses a docker version there's some extra overhead as it caches the solc version string and a solcjs companion compiler. All subsequent runs should be at full speed. 

Times to `truffle compile` on a MacBook Air 1.8GHz, Node 8.11.1

| Project              | # files | solcjs | docker | bin |
|----------------------|---------:| ------:|--------:|-----------:|
| truffle/metacoin-box |       3 |   4.4s |   4.4s |      4.7s |
| gnosis/pm-contracts  |      34 |  21.7s |  10.9s |     10.2s |
| zeppelin-solidity    |     107 |  36.7s |  11.7s |     11.1s |


For help installing a natively built compiler, see the Solidity docs [here](https://solidity.readthedocs.io/en/v0.4.23/installing-solidity.html#binary-packages).
