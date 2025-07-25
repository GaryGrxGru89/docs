# Using Celestia as DA

<!-- markdownlint-disable MD033 -->
<script setup>
import constants from '../../.vitepress/constants/constants.js'
</script>

## 🌞 Introduction {#introduction}

This tutorial serves as a comprehensive guide for deploying your rollup on Celestia's data availability (DA) network. From the Rollkit perspective, there's no difference in posting blocks to Celestia's testnets or Mainnet Beta.

Before proceeding, ensure that you have completed the [gm-world](/guides/gm-world.md) tutorial, which covers installing the Testapp CLI and running a rollup against a local DA network.

## 🪶 Running a Celestia light node

Before you can start your rollup node, you need to initiate, sync, and fund a light node on one of Celestia's networks on a compatible version:

Find more information on how to run a light node in the [Celestia documentation](https://celestia.org/run-a-light-node/#start-up-a-node).

::: code-group

```sh-vue [Arabica]
Rollkit Version: {{constants.celestiaNodeArabicaRollkitTag}}
Celestia Node Version: {{constants.celestiaNodeArabicaTag}}
```

```sh-vue [Mocha]
Rollkit Version: {{constants.celestiaNodeMochaRollkitTag}}
Celestia Node Version: {{constants.celestiaNodeMochaTag}}
```

```sh-vue [Mainnet]
Rollkit Version: {{constants.celestiaNodeMainnetRollkitTag}}
Celestia Node Version: {{constants.celestiaNodeMainnetTag}}
```

:::

- [Arabica Devnet](https://docs.celestia.org/how-to-guides/arabica-devnet)
- [Mocha Testnet](https://docs.celestia.org/how-to-guides/mocha-testnet)
- [Mainnet Beta](https://docs.celestia.org/how-to-guides/mainnet)

The main difference lies in how you fund your wallet address: using testnet TIA or [TIA](https://docs.celestia.org/learn/tia#overview-of-tia) for Mainnet Beta.

After successfully starting a light node, it's time to start posting the batches of blocks of data that your rollup generates to Celestia.

## 🏗️ Prerequisites {#prerequisites}

- `gmd` CLI installed from the [gm-world](/guides/gm-world.md) tutorial.

## 🛠️ Configuring flags for DA

Now that we are posting to the Celestia DA instead of the local DA, the `rollkit start` command requires three DA configuration flags:

- `--rollkit.da.start_height`
- `--rollkit.da.auth_token`
- `--rollkit.da.namespace`

:::tip
Optionally, you could also set the `--rollkit.da.block_time` flag. This should be set to the finality time of the DA layer, not its actual block time, as Rollkit does not handle reorganization logic. The default value is 15 seconds.
:::

Let's determine which values to provide for each of them.

First, let's query the DA layer start height using our light node.

```bash
DA_BLOCK_HEIGHT=$(celestia header network-head | jq -r '.result.header.height')
echo -e "\n Your DA_BLOCK_HEIGHT is $DA_BLOCK_HEIGHT \n"
```

The output of the command above will look similar to this:

```bash
 Your DA_BLOCK_HEIGHT is 2127672
```

Now, let's obtain the authentication token of your light node using the following command:

::: code-group

```bash [Arabica Devnet]
AUTH_TOKEN=$(celestia light auth write --p2p.network arabica)
echo -e "\n Your DA AUTH_TOKEN is $AUTH_TOKEN \n"
```

```bash [Mocha Testnet]
AUTH_TOKEN=$(celestia light auth write --p2p.network mocha)
echo -e "\n Your DA AUTH_TOKEN is $AUTH_TOKEN \n"
```

```bash [Mainnet Beta]
AUTH_TOKEN=$(celestia light auth write)
echo -e "\n Your DA AUTH_TOKEN is $AUTH_TOKEN \n"
```

:::

The output of the command above will look similar to this:

```bash
 Your DA AUTH_TOKEN is eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBbGxvdyI6WyJwdWJsaWMiLCJyZWFkIiwid3JpdGUiXX0.cSrJjpfUdTNFtzGho69V0D_8kyECn9Mzv8ghJSpKRDE
```

Next, let's set up the namespace to be used for posting data on Celestia:

```bash
DA_NAMESPACE=00000000000000000000000000000000000000000008e5f679bf7116cb
```

:::tip
`00000000000000000000000000000000000000000008e5f679bf7116cb` is a default namespace for Mocha testnet. You can set your own by using a command similar to this (or, you could get creative 😎):

```bash
openssl rand -hex 10
```

Replace the last 20 characters (10 bytes) in `00000000000000000000000000000000000000000008e5f679bf7116cb` with the newly generated 10 bytes.

[Learn more about namespaces](https://docs.celestia.org/tutorials/node-tutorial#namespaces).
:::

Lastly, set your DA address for your light node, which by default runs at
port 26658:

```bash
DA_ADDRESS=http://localhost:26658
```

## 🔥 Running your rollup connected to Celestia light node

Finally, let's initiate the rollup node with all the flags:

```bash
gmd start \
    --rollkit.node.aggregator \
    --rollkit.da.auth_token $AUTH_TOKEN \
    --rollkit.da.namespace $DA_NAMESPACE \
    --rollkit.da.start_height $DA_BLOCK_HEIGHT \
    --rollkit.da.address $DA_ADDRESS
```

Now, the rollup is running and posting blocks (aggregated in batches) to Celestia. You can view your rollup by using your namespace or account on one of Celestia's block explorers.

For example, [here on Celenium for Arabica](https://arabica.celenium.io/).

Other explorers:

- [Arabica testnet](https://docs.celestia.org/how-to-guides/arabica-devnet#explorers)
- [Mocha testnet](https://docs.celestia.org/how-to-guides/mocha-testnet#explorers)
- [Mainnet Beta](https://docs.celestia.org/how-to-guides/mainnet#explorers)

## 🎉 Next steps

Congratulations! You've built a local rollup that posts data to Celestia's DA layer. Well done! Now, go forth and build something great! Good luck!
