---
description: Frequently asked questions.
---

# FAQ

<details>

<summary>What to do if I am stuck with pending transactions on a fork network?</summary>

You can end up in this situation if you are following the [onchain tutorial](tutorials/onchain-tutorials/tuto.md) or the [onchain sample project](run-example-apps.md). If it is the case, you are unable to make any transaction work on your local chain, and you surely view this type of message on your MetaMask:

![MetaMask connected to a locally forked Mumbai Testnet](<../.gitbook/assets/Capture d’écran 2023-05-12 à 01.10.06.png>)

![Pending MetaMask transactions](<../.gitbook/assets/Capture d’écran 2023-05-12 à 01.11.27.png>)

To get rid of this issue, you will need to go to "Settings > Advanced > Clear activity tab data" while connected to the fork chain (so anvil node is still running).

![MetaMask settings to clear activity tab data](<../.gitbook/assets/Capture d’écran 2023-05-12 à 01.18.14 (1).png>)

You can then stop your anvil local node and the frontend of your application before relaunching the anvil node with `yarn anvil` and then your frontend with `yarn dev`. If you are still stuck with this restart, come ask your questions in our [Discord](https://discord.gg/sismo) in the **#dev-support** channel. &#x20;

</details>
