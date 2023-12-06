# L402 (Lightning HTTP 402) API Key proxy

Aperture is your portal to the Lightning-Native Web. Aperture is used in
production today by [Lightning Loop](https://lightning.engineering/loop), a
non-custodial on/off ramp for the Lightning Network.

Aperture is a HTTP 402 reverse proxy that supports proxying requests for gRPC
(HTTP/2) and REST (HTTP/1 and HTTP/2) backends using the [L402 Protocol
Standard](https://lsat.tech/). L402 is short for: the Lightning HTTP 402
protocol.  L402 combines HTTP 402, macaroons, and the Lightning Network to
create a new standard for authentication and paid services on the web.

L402 is a new standard protocol for authentication and paid APIs developed by
Lightning Labs. L402 API keys can serve both as authentication, as well as a
payment mechanism (one can view it as a ticket) for paid APIs. In order to
obtain a token, we require the user to pay us over Lightning in order to obtain
a preimage, which itself is a cryptographic component of the final L402 token

The implementation of the authentication token is chosen to be macaroons, as
they allow us to package attributes and capabilities along with the token. This
system allows one to automate pricing on the fly and allows for a number of
novel constructs such as automated tier upgrades. In another light, this can be
viewed as a global HTTP 402 reverse proxy at the load balancing level for web
services and APIs.

## Installation / Setup

**lnd**

* Make sure `lnd` ports are reachable.

**aperture**

* Compilation requires go `1.19.x` or later.
* To build `aperture` in the current directory, run `make build` and then copy the
  file `./aperture` from the local directory to the server.
* To build and install `aperture` directly on the machine it will be used, run the
  `make install` command which will place the binary into your `$GOPATH/bin`
  folder.
* Make sure port `8081` is reachable from outside (or whatever port we choose,
  could also be 443 at some point)
* Make sure there is a valid `tls.cert` and `tls.key` file located in the
  `~/.aperture` directory that is valid for the domain that aperture is running on.
  Aperture doesn't support creating its own certificate through Let's Encrypt yet.
  If there is no `tls.cert` and `tls.key` found, a self-signed pair will be
  created.
* Make sure all required configuration items are set in `~/.aperture/aperture.yaml`,
  compare with `sample-conf.yaml`.
* Start aperture without any command line parameters (`./aperture`), all configuration
  is done in the `~/.aperture/aperture.yaml` file.

## Kaspar

Prerequisites:
* run btcd and lnd: https://github.com/lightningnetwork/lnd/tree/master/docker
* Alice address: rkneX4ETvnn51qmrDrDawBjjX83WdysA1U
* export MINING_ADDRESS=rkneX4ETvnn51qmrDrDawBjjX83WdysA1U && docker-compose up -d btcd
* docker inspect bob | grep IPAddress
* alice $  lncli --network=simnet connect 0235dd121f9f15c5867d4a304b1199cff3418e886b7b7fc0b057d09f66d3ce0f3f@172.23.0.4
* lncli --network=simnet openchannel --node_key=0235dd121f9f15c5867d4a304b1199cff3418e886b7b7fc0b057d09f66d3ce0f3f --local_amt=1000000
* create invoice: lncli --network=simnet addinvoice --amt 100
* pay invoice: lncli --network=simnet sendpayment --pay_req=lnsb10u1pjhqd09pp52kx3sjxxp2crrgdlk9pfx7y9l0djda067hvxwd9xn0a5rqgwk9xqdqqcqzzsxqyz5vqsp5t8fsuj7qft8e5pzg9vas4ufxrpmkvy4vqy00v3rrgwagjr5ga6ts9qyyssqsxghpgkd36wep22kls2x6mzlyk423lxak549ruwx70h2nwx2nstjqhrkae7qg0kzf8v2tuda4qy2vg2kmd2ajq43akwzf42y3adc63qqzhcndy

bunch of more scripts....

Send invoice:
```
lncli --network=simnet addinvoice --amt=10000
{
        "r_hash": "6d7468997e2e31a986c95932948c2115c49226000b8220ee85236481107bb10a", 
        "pay_req": "lnsb1u1pjhqvsgpp5d46x3xt79cc6npkftyeffrppzhzfyfsqpwpzpm59ydjgzyrmky9qdqqcqzzsxqyz5vqsp59je6cmy92w90080wyjn2wx6tvuvz9hw94dv6qnfy30c5utkx8g0q9qyyssqtqtcw5ukuqk5xtezlnn4c966nfcyf8l9sxq53q0l8ss2trr26ms5ehrn27nwtkrr23t3jyg5nh9hzeq2wx3hjsmfqa886r322azpnuqqw94fls", 
}
```

```shell

```

Prerequisites:

```shell
docker pull lightninglabs/lnd:v0.14.1-beta
docker run \
  --name=lnd-testnet \
  -p 8080:8080 \
  -v $PWD/.lnd:/root/.lnd \
  -it lightninglabs/lnd:v0.14.1-beta \
  --bitcoin.active \
  --bitcoin.testnet \
  --bitcoin.node=neutrino \
  --neutrino.connect=faucet.lightning.community
  
docker exec -it lnd-testnet lncli create
```

Local Test Wallet
---------------BEGIN LND CIPHER SEED---------------
 1. about    2. undo     3. fatal      4. circle
 5. ignore   6. sign     7. media      8. cheap
 9. trial   10. mad     11. theme     12. cricket
13. drive   14. smile   15. wasp      16. gentle
17. stamp   18. engine  19. strategy  20. honey
21. until   22. used    23. hospital  24. feature
---------------END LND CIPHER SEED-----------------


* LND RPC server: `localhost:10009`
* LND gRPC server: `localhost:8080`

Alternatively:

https://cosmopolitan-wandering-needle.btc-testnet.quiknode.pro/cda8b12f75bbddaf2e2bb9a51fe401cf149685af/

Setup:

```
docker build . -t aperature
docker run \
  -p 8081:8081 \
  -p 9000:9000 \
  -p 9999:9999 \
  -p 11010:11010 \
  --name aperture \
  -it aperature aperture
```

Then go over to: https://github.com/KasparPeterson/LangChainBitcoin



docker compose run \
  --service-ports lnd \
  -d \
  --name alice \
  --volume simnet_lnd_alice:/root/.lnd \ 
  lnd
