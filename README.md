#Overview
Allows you to manage or reference your Trello board through commits to Github. Tag a commit with "Finishes 1234", for example to update that card and move it to a list you specify! 

##Commands
Commit messages are searched for `(start|card|close|fix)e?s? \D?([0-9]+)` to find the card short id. All commands will add the commit message to the card.

- `start` and `per` will move the card to a list specified in configuration by the `start_list_target_id` parameter.
- `finish` and `fix` will move the card to a list specified in configuration by the `finish_list_target_id` parameter.

Examples, assuming the start list is "doing" and the finish list is "ready for release":

```
git commit -m 'Added a class to start 124'
```
This will move card #124 to the "doing" list.

```
git commit -m 'Added a few more tests card 124'
```
This will add a message to card #124 but nothing more (as it is already in "Doing")

```
git commit -m 'Finalized details to finish 124'
```
This will move card #124 to the "ready for release" list.

##Installation

###Gather config values
- **api_key** - Go to [https://trello.com/1/appKey/generate](https://trello.com/1/appKey/generate)
- **oauth_token** - Go to _https://trello.com/1/authorize?response_type=token&name=Trello+Github+Integration&scope=read,write&expiration=never&key=[your-key-here]_ replacing __[your-key-here]__ with the **api_key** from above. Authorize the request and record the token.
- **board_id** - You can get the board id from the URL, for example https://trello.com/board/trello-development/4d5ea62fd76aa1136000000c the board id is _4d5ea62fd76aa1136000000ca_.
- **…list_target_id** - These can be found by opening a card in the list, exporting it as json, and grabbing the "idList" value.

###Deploy to Heroku
Follow these steps replacing the flagged values with the ones you gathered above:

- clone this repo
- `cd github-trello`
- `heroku create`
- `heroku config:add api_key=<API_KEY> oauth_token=<OATH_TOKEN> board_id=<BOARD_ID> start_list_target_id=<ID> finish_list_target_id=<ID> deployed_list_target_id=<ID>`
- `git push heroku master`

Now the server should be running on Heroku.

###Set up GitHub
Simply add you your Heroku app url + "/posthook-github" as a WebHook url under "Admin" for your repository. Example:

`http://crazy-cow-123.herokuapp.com/posthook`

###Set up Heroku Deployment Hook
Add HTTP Post Heroku Deployment hook and point the hook to Heroku app url + "/posthook-heroku"

*--enjoy*