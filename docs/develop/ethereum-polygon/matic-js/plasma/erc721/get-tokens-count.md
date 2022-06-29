---
id: get-tokens-count
title: getTokensCount
keywords: 
- 'plasma client, erc721, getTokensCount, polygon, sdk'
description: 'Returns tokens count for specified user.'
---

`getTokensCount` method returns tokens count for specified user.

```
const erc721Token = plasmaClient.erc721(<token address>);

const result = await erc721Token.getTokensCount(<user address>);

```
