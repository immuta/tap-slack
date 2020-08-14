# notes

* Have to use the method described in [this issue](https://github.com/singer-io/tap-shopify/issues/40), otherwise you get no data from the tap. (Just running `--discover` to get the `catalog.json` does not do it automatically.)
* may be hitting [slack api rate limit](https://api.slack.com/docs/rate-limits)
* as noted [here](https://api.slack.com/authentication/token-types), it appears that the api token that Stephen handed over was a user token (we can tall because the token in `config.json` starts with `xoxp-`)
* when trying to pull `user_groups`, we get the following error:

```
The server responded with: {
   "ok":False,
   "error":"missing_scope",
   "needed":"usergroups:read",
   "provided":"identify,channels:history,channels:read,groups:read,im:read,mpim:read,users:read,users:read.email,users.profile:read,links:read"
}
```

which means that apparently our api token does not have the `"usergroups:read"` permission, but it looks like we should be able to get that, according to [this documentation](https://api.slack.com/scopes/usergroups:read)

* a similar thing happens when trying to pull `teams`/`files`/`remote_files` data. 
    - to be clear, here is the comprehensive list: [`user_groups`, `teams`, `files`, `remote_files`,]

* error for `messages`

```

Errors during transform: [last_read: data does not match {'type': ['string', 'null'], 'format': 'date-time'}, : data does not match {'additionalProperties': False, 'properties': {'channel_id': {'type': ['string', 'null']}, 'blocks': {'items': {'additionalProperties': True, 'properties': {'type': {'type': ['string', 'null']}}, 'type': ['object', 'null']}, 'type': ['array', 'null']}, 'bot_id': {'type': ['string', 'null']}, 'bot_profile': {'additionalProperties': False, 'properties': {'app_id': {'type': ['string', 'null']}, 'deleted': {'type': ['boolean', 'null']}, 'id': {'type': ['string', 'null']}, 'name': {'type': ['string', 'null']}, 'team_id': {'type': ['string', 'null']}, 'updated': {'type': ['string', 'null'], 'format': 'date-time'}}, 'type': ['object', 'null']}, 'client_msg_id': {'type': ['string', 'null']}, 'display_as_bot': {'type': ['boolean', 'null']}, 'file_id': {'type': ['null', 'string']}, 'file_ids': {'items': {'type': ['string', 'null']}, 'type': ['array', 'null']}, 'icons': {'additionalProperties': False, 'properties': {'emoji': {'type': ['null', 'string']}}, 'type': ['null', 'object']}, 'inviter': {'type': ['null', 'string']}, 'is_delayed_message': {'type': ['null', 'boolean']}, 'is_intro': {'type': ['null', 'boolean']}, 'is_starred': {'type': ['null', 'boolean']}, 'last_read': {'type': ['string', 'null'], 'format': 'date-time'}, 'latest_reply': {'type': ['string', 'null']}, 'name': {'type': ['null', 'string']}, 'old_name': {'type': ['null', 'string']}, 'parent_user_id': {'type': ['null', 'string']}, 'permalink': {'format': 'uri', 'type': ['null', 'string']}, 'pinned_to': {'items': {'type': ['null', 'string']}, 'type': ['null', 'array']}, 'purpose': {'type': ['null', 'string']}, 'reactions': {'items': {'additionalProperties': True, 'properties': {'count': {'type': ['integer', 'null']}, 'name': {'type': ['string', 'null']}, 'users': {'items': {'type': ['string', 'null']}, 'type': ['array', 'null']}}, 'type': ['object', 'null']}, 'type': ['array', 'null']}, 'reply_count': {'type': ['integer', 'null']}, 'reply_users': {'items': {'type': ['string', 'null']}, 'type': ['array', 'null']}, 'reply_users_count': {'type': ['integer', 'null']}, 'source_team': {'type': ['null', 'string']}, 'subscribed': {'type': ['boolean', 'null']}, 'subtype': {'type': ['string', 'null']}, 'team': {'type': ['string', 'null']}, 'text': {'type': ['string', 'null']}, 'thread_ts': {'type': ['string', 'null']}, 'topic': {'type': ['null', 'string']}, 'ts': {'type': ['string', 'null'], 'format': 'date-time'}, 'type': {'type': ['string', 'null']}, 'unread_count': {'type': ['null', 'integer']}, 'upload': {'type': ['boolean', 'null']}, 'user': {'type': ['string', 'null']}, 'user_team': {'type': ['null', 'string']}, 'username': {'type': ['null', 'string']}}, 'type': ['object', 'null']}]
```
