---
id: is-aproved
title: isApproved
keywords: 
- 'plasma client, erc721, isApproved, polygon, sdk'
description: 'Checks if a token is approved for a specified tokenId.'
---

`isApproved` method checks if token is approved for specified tokenId. It returns boolean value.

```
const erc721Token = plasmaClient.erc721(<token address>, true);

const result = await erc721Token.isApproved(<tokenId>);

```
