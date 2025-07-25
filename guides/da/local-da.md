# Using Local DA

<!-- markdownlint-disable MD033 -->
<script setup>
import constants from '../../.vitepress/constants/constants.js'
</script>

## Introduction {#introduction}

This tutorial serves as a comprehensive guide for using the [local-da](https://github.com/rollkit/local-da) with your chain.

Before proceeding, ensure that you have completed the [quick start](/guides/quick-start.md) or [build a chain](/guides/gm-world.md) tutorial, which covers installing the rollkit CLI, building your chain, and running your chain.

## Setting Up a Local DA Network

To set up a local DA network node on your machine, run the following script to install and start the local DA node:

```bash-vue
curl -sSL https://rollkit.dev/install-local-da.sh | bash -s {{constants.localDALatestTag}}
```

This script will build and run the node, which will then listen on port `7980`.

## Configuring your rollup to connect to the local DA network

To connect your rollup to the local DA network, you need to pass the `--rollkit.da.address` flag with the local DA node address.

## Run your rollup

Start your rollup node with the following command, ensuring to include the DA address flag:

::: code-group

```sh [Quick Start]
{BINARY} start --rollkit.da.address http://localhost:7980
```

```sh [gm-world Chain]
{BINARY} start \
    --rollkit.node.aggregator \
    --rollkit.da.address http://localhost:7980 \
```

:::

You should see the following log message indicating that your rollup is connected to the local DA network:

```shell
11:07AM INF NewLocalDA: initialized LocalDA module=local-da
11:07AM INF Listening on host=localhost maxBlobSize=1974272 module=da port=7980
11:07AM INF server started listening on=localhost:7980 module=da
```

## Summary

By following these steps, you will set up a local DA network node and configure your rollup to post data to it. This setup is useful for testing and development in a controlled environment. You can find more information on running the local-da binary [here](https://github.com/rollkit/rollkit/blob/main/da/cmd/local-da/README.md)
