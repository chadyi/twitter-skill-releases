---
name: twitter-skill
description: Search X/Twitter, list connected accounts, upload images, publish posts, reply, and delete posts through the locally installed Twitter Skill desktop CLI. Use when the user asks to find information on Twitter/X or operate an account connected in Twitter Skill.
---

# Twitter Skill

Use the desktop app's local CLI for every X operation. It returns JSON on stdout and sends X requests from the user's device. Keep the desktop app running and preserve its local login session.

Resolve the CLI before running a command:

- Windows PowerShell: `$twitterSkill = Join-Path $env:APPDATA 'Twitter Skill\bin\twitter-skill.cmd'`
- macOS shell: `twitter_skill="$HOME/Library/Application Support/Twitter Skill/bin/twitter-skill"`

On Windows, invoke commands with `& $twitterSkill`. On macOS, invoke them with `"$twitter_skill"`.

## Accounts

List connected accounts when the user names an account but its account ID is unknown:

```text
accounts
```

Omit `--account-id` to use the default account.

## Search

```text
search --query "from:OpenAI agents" --count 20 --product Latest
```

Pass the returned cursor with `--cursor` for the next page. Valid products are `Latest`, `Top`, `Photos`, and `Videos`.

## Publish

Publish immediately when requested. Do not ask for a second confirmation unless the user asks for a draft or preview.

```text
post --text "Post text" --account-id 12
```

Reply with `--reply-to TWEET_ID`. Add up to four uploaded media IDs with repeated `--media-id` arguments.

Upload an image before attaching it:

```text
upload "/path/to/image.png" --account-id 12
```

Delete a post:

```text
delete TWEET_ID --account-id 12
```

Never request, display, or write X cookies. Report CLI errors directly. Do not retry a timed-out publish command because the post may already exist.
