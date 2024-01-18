# About
This document shows and explains what data is sent to BotBuilders via the conversation context in order to know know to use it.

## Custom Contexts

## Main Context
| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| botName | Bot's Name | String |
| [snappylabs](#snappy-labs) | Snappylabs Information | Object |

## Snappy Labs
The following data is inside: `context.snappylabs` or `$snappylabs`

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| [integrations](#integrations) | Bot's enabled integrations | Object |
| [promotions](#promotions) | Bot's promotions configuration | Object |
| [mainMenu](#main-menu) | Bot's Main Menu configuration | Object |
| [branches](#branches) | Bot store's branches | Object |



### Integrations
The integrations object gives the bot/Watson information about what integrations are currently enabled

In this sub-object (**$snappylabs.integrations**) each key represents an integration and the value tells if enabled.

> **Example:** *if $snappylabs.integration.vtex = true* 

> **Important:** *This table is automatically filled, meaning that for each new integration a new key will appear. This example may be missing some key names.*

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| [Integration Name] | Is **[Integration Name]** enabled? | Boolean |
| castillo | Is **Castillo** enabled? | Boolean |
| forus | Is **Forus** enabled? | Boolean |
| freshdesk | Is **Freshdesk** enabled? | Boolean |
| glamit | Is **Glamit** enabled? | Boolean |
| googleAnalytics | Is **GoogleAnalytics** enabled? | Boolean |
| markova | Is **Markova** enabled? | Boolean |
| vtex | Is **VTEX** enabled? | Boolean |
| woowup | Is **Woowup** enabled? | Boolean |
| zendesk | Is **Zendesk** enabled? | Boolean |

### Promotions
The promotions sub-object tells Watson information about the bot's store promotions.

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| hasPromotions | Does this bot's store has currently active promotions | Boolean |

> **hasPromotions:** Will return **true/false** if there are **any** currently active promotions and is updated for each message sent to Watson.

### Main Menu
The Main Menu sub-object tells Watson information about the bot's configured Main Menu (Quick Reply 1).

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| hasMenu | Does this bot's have a configured main menu? | Boolean |
| question | The main menu question, if any | String |
| items | Possible main menu replies, if any | [String] |

### Branches
> **Important:** This sub-object is currently not in use.

This object will tell Watson information about a store's branches, for example:
**Last Branch Shown**, **Store's Branches** and **Nearest Branch**
