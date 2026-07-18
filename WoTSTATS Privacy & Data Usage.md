# WoTSTATS privacy and data usage notice

**Effective date:** 2026-07-18

WoTSTATS is a Discord bot that connects Discord users with World of Tanks data. It is operated by IVORK (`140902977618706432`) and is not endorsed by Wargaming.net. World of Tanks and Wargaming.net are trademarks or registered trademarks of their respective owners. WoTSTATS is independent of WoTstats.org.

This notice describes the rebuilt WoTSTATS service. It does not replace the separate historical notice used by the legacy bot.

## What WoTSTATS collects

WoTSTATS collects only the information needed to provide its features, operate the service, and investigate failures.

### Discord information

- Your Discord user ID when you link a World of Tanks account, change a personal setting, unlink, or use an invited support conversation.
- A server's Discord ID, owner ID, observed inviter ID where available, initial member-count classification, and its WoTSTATS settings.
- Discord channel or role IDs only when a server administrator configures a feature that needs them.
- WoTSTATS does not build or retain a general directory of every member in a Discord server.

### World of Tanks information

- The selected realm (`SEA`, `EU`, or `NA`), Wargaming account ID, and current nickname used to identify a linked account.
- An encrypted Wargaming access token after successful authorization. The encryption key is stored separately from the database. Tokens, authorization callback parameters, and raw provider responses are never written to application logs.
- Public account, vehicle, and clan information requested for bot commands.
- For commands about your own linked account, the service may retrieve economy fields such as credits, gold, free experience, and bonds. These fields are fetched for that response and are not stored as command telemetry or durable private-stat records.

WoTSTATS does not receive your Wargaming password, email address, or phone number. Player-specific public and private account requests use the official Wargaming API. The optional WN8 enrichment uses a cached global expected-values table published by ModXVM; WoTSTATS does not send ModXVM your account ID, nickname, Discord ID, realm, command context, Wargaming token, or private account fields. WN8 is calculated inside WoTSTATS from that global table and official public per-tank totals.

When the optional `/fstats` command is enabled and invoked, WoTSTATS first resolves the requested player through Wargaming, then sends Tomato.gg that Wargaming account ID and mapped realm to request 30-day and 100-battle recent public statistics. Tomato.gg also receives ordinary request metadata such as the WoTSTATS server's network address and request time. WoTSTATS does not send Tomato.gg a Discord ID or nickname, Wargaming nickname or access token, private economy field, guild/channel/message context, or another player's battle participants. The Tomato response is used for that command response and is not stored as local player history or raw provider data. Existing minimized command telemetry does not store the target or returned statistics.

### Usage and error information

- Minimized command telemetry records the feature used, outcome, response type, duration band, environment, and—when applicable—the server and channel IDs. It does not record the invoking user's ID, command arguments, message content, or returned private values.
- Sanitized error records may include a technical fingerprint, release, feature, provider, realm, non-secret request shape, and the exact Discord user/server/channel or World of Tanks account IDs needed to reproduce and investigate a failure.
- Routine logs use allowlisted structured fields. Message bodies, attachments, command arguments, access tokens, authorization state, and callback query strings are excluded.

## Who can see private account information

Private World of Tanks fields are available only when a linked user requests their own account. Targeting another player never reveals that player's private fields, regardless of anyone's privacy setting.

`/privacy` offers three response modes:

| Mode | Command with no target, resolving to you | Command explicitly targeting you | Command targeting another player |
|---|---|---|---|
| **Public** (default) | Public, including your private fields | Public, including your private fields | Public, without private fields |
| **Balanced** | Public, without private fields | Ephemeral, including your private fields | Public, without private fields |
| **Private** | Ephemeral, including your private fields | Ephemeral, including your private fields | Ephemeral, without private fields |

For **Balanced** explicit targets, WoTSTATS compares the resolved Wargaming realm and account ID with your linked account. If that exact comparison cannot finish safely before Discord's initial-response deadline, the response fails private (ephemeral) rather than risk making an explicit-self response public.

Authorization, privacy changes, server-setting changes, and other security-sensitive interactions remain ephemeral regardless of this preference. Discord controls ephemeral-message delivery within its platform.

If an authenticated linking attempt targets a World of Tanks account that is already linked to a different Discord user, WoTSTATS rejects it and leaves the original link unchanged. The existing owner may receive a rate-bounded security notice. That notice does not identify the other Discord user or include the account ID, nickname, realm, guild/channel, credential, or callback data. The failed authorization-session metadata and delivered notice work item follow the 30-day periods below.

## Support direct messages

WoTSTATS does not monitor unsolicited direct messages. A support reply window opens only after the owner successfully sends you a message through the bot and remains open for 24 hours. Direct messages outside an active window are immediately ignored and are not logged.

During an active window, a bounded copy of your reply and up to three attachment links may be forwarded to a restricted support channel in the operator's Discord test/support server. WoTSTATS does not store that message body or its attachments in its application database or logs. The forwarded Discord message remains subject to Discord's own storage, privacy, and deletion behavior.

## How long information is kept

| Information | Retention or action |
|---|---|
| Raw authorization state | Never stored. Only a one-way digest is stored, and a pending session expires after 10 minutes. |
| Finished authorization-session metadata | Deleted after 30 days. It contains no raw authorization state or token. |
| Encrypted Wargaming credential | Kept while the account is actively linked. On `/unlink`, access is disabled locally immediately and a bounded official logout is attempted. Local ciphertext is erased after success, an invalid-token response, an unusable local envelope, or the final retry even if Wargaming remains unavailable. |
| Active link and minimized account lifecycle/audit metadata | Kept while needed to operate the link and investigate consequential account changes. A deletion request can remove or de-identify the applicable direct identifiers, subject to any narrowly required security evidence. |
| Server configuration | Marked inactive when the bot leaves and deleted after 30 days, including subordinate channel and role settings. |
| Support reply window metadata | Usable for 24 hours; closed metadata is deleted after 30 days. Message content is not stored in the application database. |
| Command usage telemetry | Deleted after 30 days. If longer-lived aggregate product counts are created, they will contain no channel or raw user/server ID. |
| Successful job history and delivered work items | Deleted after 30 days. Failed, skipped, or cancelled job records are deleted after 90 days. |
| Routine structured logs | Target retention of 30 days with size limits; secret and message-content fields are prohibited. |
| Sanitized reproducible errors | May be retained indefinitely so old or rarely reported bugs can be investigated. Exact user, server, channel, or game-account identifiers are stored separately under restricted access and are erased when a valid deletion request applies; non-identifying technical history may remain. |
| Public/provider cache | Contains only normalized global/public reference data with source, freshness, and bounded technical-status metadata—never an access token, Discord identifier, player identifier, or private economy field. Entries that remain expired for more than 30 days are deleted. |

The current isolated staging service contains no imported live-user dataset. Its public callback route accepts only the account-link callback and a minimal liveness check; readiness and operator detail remain private. Its local same-disk database dump is operational rehearsal evidence, not disaster recovery. An encrypted off-host production backup policy and its retention period must be tested and added to this notice before production launch; this notice makes no claim that such backups already exist.

## Other services

WoTSTATS communicates with:

- Discord, to register and respond to bot interactions and deliver invited support messages;
- Wargaming's official API, to authorize linked accounts and retrieve requested World of Tanks information;
- ModXVM, to retrieve a provider-published global static WN8 expected-values table without sending a user or player identifier;
- Tomato.gg, only when optional `/fstats` is enabled and invoked, to retrieve recent public statistics using the requested Wargaming account ID and realm; and
- Cloudflare, when the approved public callback hostname is enabled, to route and protect the web callback.

Those providers process information under their own terms and privacy notices. Tomato.gg's policy is available at [tomato.gg/privacy-policy](https://tomato.gg/privacy-policy). Other optional statistics sites, Top.gg posting, clan-reserve automation, and premium/test features are not enabled. If a later feature changes the data shared or collected, this notice must be updated before that feature is enabled.

## Your choices and requests

- Use `/privacy` to change how your own command responses are shown.
- Use `/unlink` to disconnect your Discord and World of Tanks accounts and begin local credential erasure/provider logout.
- Contact the operator through the support server at [ivork.com/discord](http://ivork.com/discord) to ask what direct identifiers are held, request deletion, or report a privacy concern.

A valid deletion request erases the applicable exact identifiers from retained diagnostic records while allowing non-identifying technical history to remain. The operator may ask for enough information to verify that the requester controls the relevant Discord or World of Tanks identity.

Questions about this notice may be directed to IVORK through the support server above.
