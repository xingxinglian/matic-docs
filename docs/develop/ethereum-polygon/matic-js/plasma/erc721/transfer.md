---
id: transfer
title: transfer
keywords: 
- 'plasma client, erc721, transfer, polygon, sdk'
description: 'Transfer tokens from one user to another user.'
---

`transfer` method transfer tokens from one user to another user.

```
const erc721Token = plasmaClient.erc721(<token address>);

const result = await erc721Token.transfer(<tokenid>,<from>,<to>);

```
