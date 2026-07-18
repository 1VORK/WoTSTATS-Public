# WoTSTATS Sol privacy and data usage notice

**Effective date:** 2026-07-18

WoTSTATS Sol is a Discord bot that connects Discord users with World of Tanks data. It is operated by IVORK (`140902977618706432`) and is not endorsed by Wargaming.net. World of Tanks and Wargaming.net are trademarks or registered trademarks of their respective owners. WoTSTATS Sol is independent of WoTstats.org.

This notice describes the rebuilt WoTSTATS Sol service. It does not replace the separate historical notice used by the legacy bot.

## What WoTSTATS Sol collects

WoTSTATS Sol collects only the information needed to provide its features, operate the service, and investigate failures.

### Discord information

- Your Discord user ID when you link a World of Tanks account, change a personal setting, unlink, or use an invited support conversation.
- A server's Discord ID, owner ID, observed inviter ID where available, initial member-count classification, and its WoTSTATS Sol settings.
- Discord channel or role IDs only when a server administrator configures a feature that needs them.
- WoTSTATS Sol does not build or retain a general directory of every member in a Discord server.

### World of Tanks information

- The selected realm (`SEA`, `EU`, or `NA`), Wargaming account ID, and current nickname used to identify a linked account.
- An encrypted Wargaming access token after successful authorization. The encryption key is stored separately from the database. Tokens, authorization callback parameters, and raw provider responses are never written to application logs.
- Public account, vehicle, and clan information requested for bot commands.
- For commands about your own linked account, the service may retrieve economy fields such as credits, gold, free experience, and bonds. These fields are fetched for that response and are not stored as command telemetry or durable private-stat records.

WoTSTATS Sol does not receive your Wargaming password, email address, or phone number. The rebuilt first-wave service uses the official Wargaming API only. It does not share Wargaming tokens or private account fields with optional statistics providers.

### Usage and error information

- Minimized command telemetry records the feature used, outcome, response type, duration band, environment, and—when applicable—the server and channel IDs. It does not record the invoking user's ID, command arguments, message content, or returned private values.
- Sanitized error records may include a technical fingerprint, release, feature, provider, realm, non-secret request shape, and the exact Discord user/server/channel or World of Tanks account IDs needed to reproduce and investigate a failure.
- Routine logs use allowlisted structured fields. Message bodies, attachments, command arguments, access tokens, authorization state, and callback query strings are excluded.

## Who can see private account information

Private World of Tanks fields are available only when a linked user requests their own account. Targeting another player never reveals that player's private fields, regardless of anyone's privacy setting.

`/privacy` offers three response modes:

| Mode | Command with no target, resolving to you | Command explicitly targeting you | Command targeting another player |
|---|---|---|---|
| `public` (default) | Public, including your private fields | Public, including your private fields | Public, without private fields |
| `ephemeral` | Ephemeral, including your private fields | Ephemeral, including your private fields | Ephemeral, without private fields |
| `balanced` | Public, without private fields | Ephemeral, including your private fields | Public, without private fields |

Authorization, privacy changes, server-setting changes, and other security-sensitive interactions remain ephemeral regardless of this preference. Discord controls ephemeral-message delivery within its platform.

## Support direct messages

WoTSTATS Sol does not monitor unsolicited direct messages. A support reply window opens only after the owner successfully sends you a message through the bot and remains open for 24 hours. Direct messages outside an active window are immediately ignored and are not logged.

During an active window, a bounded copy of your reply and up to three attachment links may be forwarded to a restricted support channel in the operator's Discord test/support server. WoTSTATS Sol does not store that message body or its attachments in its application database or logs. The forwarded Discord message remains subject to Discord's own storage, privacy, and deletion behavior.

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
| Public/provider cache | Used only through its recorded freshness period and contains no access token or private economy fields. |

The current isolated staging service contains no imported live-user dataset and has no public callback route. Its local same-disk database dump is operational rehearsal evidence, not disaster recovery. An encrypted off-host production backup policy and its retention period must be tested and added to this notice before production launch; this notice makes no claim that such backups already exist.

## Other services

WoTSTATS Sol communicates with:

- Discord, to register and respond to bot interactions and deliver invited support messages;
- Wargaming's official API, to authorize linked accounts and retrieve requested World of Tanks information; and
- Cloudflare, when the approved public callback hostname is enabled, to route and protect the web callback.

Those providers process information under their own terms and privacy notices. Optional statistics sites, Top.gg posting, clan-reserve automation, and other premium/test features are not part of the rebuilt first-wave service. If a later feature changes the data shared or collected, this notice must be updated before that feature is enabled.

## Your choices and requests

- Use `/privacy` to change how your own command responses are shown.
- Use `/unlink` to disconnect your Discord and World of Tanks accounts and begin local credential erasure/provider logout.
- Contact the operator through the support server at [ivork.com/discord](http://ivork.com/discord) to ask what direct identifiers are held, request deletion, or report a privacy concern.

A valid deletion request erases the applicable exact identifiers from retained diagnostic records while allowing non-identifying technical history to remain. The operator may ask for enough information to verify that the requester controls the relevant Discord or World of Tanks identity.

Questions about this notice may be directed to IVORK through the support server above.
