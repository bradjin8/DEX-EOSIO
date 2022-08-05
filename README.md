# The first listing free decentralized exchange.
Documentation: [docs.alcor.exchange](https://docs.alcor.exchange)


With Alcor Exchange you can trade any EOS.IO tokens for system EOS tokens, atomically, without the participation of third parties! The token's contract should comply with the [eosio.token](https://github.com/EOSIO/eosio.contracts/tree/master/contracts/eosio.token) standard or be a [Simple Assets](https://github.com/CryptoLions/SimpleAssets) fungible token.

Props:
1. Fully-onchain.
2. Free CPU program.

[Join our telegram!](https://t.me/+JYnCfHS9qvw0Yzc1)

## Chains:
1. EOS Mainnet
2. WAX
3. Telos
4. Proton

## Build Setup

``` bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ NETWORK=<eos/wax/jungle/local> DISABLE_DB=true yarn run dev

# build for production and launch server
$ NETWORK=<eos/jungle/local> DISABLE_DB=true yarn run build
$ yarn start

# generate static project
$ yarn run generate

# Docker run:
docker run -d --restart=unless-stopped -p 127.0.0.1:27017:27017 --name mongo -m=3g mongo:4.4 --bind_ip 0.0.0.0

docker run -it -p 7002:7000 --restart=unless-stopped -d --label=com.centurylinklabs.watchtower.lifecycle.post-check="rm -rf /data/nginx/cache/eostokens && service nginx reload" --label=com.centurylinklabs.watchtower.enable=true --name alcor-ui --add-host=host.docker.internal:172.17.0.9 avral/alcor-ui
```

## Created:
[monarch Lee](https://www.linkedin.com/in/lee-cheng-ri-9086111a1/)
