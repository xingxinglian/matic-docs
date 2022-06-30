---
id: is-exited-many
title: isExitedMany
keywords: 
- 'plasma client, erc721, isExitedMany, polygon, sdk'
description: 'Checks if a withdraw has been exited.'
---

`isExitedMany` method check if a withdraw has been exited. It returns boolean value.

```
const erc721Token = plasmaClient.erc721(<token address>);

const result = await erc721Token.isExitedMany(<exit tx hash>);

```
