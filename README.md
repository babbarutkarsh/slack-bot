SLACK BOT
`mybot` is an working Slack bot written in Go. Fork it and use it to build
your very own cool Slack bot!

REQUIREMENTS:
You need to have a Slack account (with enough privileges to create a bot), and know a bit of Go. Let’s start!

Go to https://YOURORG.slack.com/services/new/bot and create a new Slack bot by choosing a username (You’ll need to substitute YOURORG with your Slack account’s domain). Copy the API Token from the next page, and save it for later use.

At this point, you should be able to see the bot username in your Slack client, in a logged out state. It joins #general by default.

Now let’s get the mybot code from github to hack on:

go get https://github.com/babbarutkarsh/mybot
You should have the binary in $GOPATH/bin after it completes. Run it to see what it does:

$ mybot
usage: mybot slack-bot-token
$ mybot xxxx-99999999999-USE.THE.TOKEN.FROM.ABOVE
mybot ready, ^C exits
mybot now is ready to fetch you stock quotes as in the screenshot above. Hit ^C when you’re done playing with it, and let’s see some code!


Code walkthrough:
First up, we need to use the token to start a Real Time Message API session. We use the rtm.start API for this. As the documentation states, we need to pass the token to rtm.start, and it responds with a whole bunch of stuff. Thankfully, we only need a couple of things from this response.

Here’s how we make the call (file slack.go, method slackStart):

```
url := fmt.Sprintf("https://slack.com/api/rtm.start?token=%s", token)
resp, err := http.Get(url)
//...
body, err := ioutil.ReadAll(resp.Body)
//...
var respObj responseRtmStart
err = json.Unmarshal(body, &respObj)
```

Starting a websocket connection using this package is easy (file slack.go, method slackConnect):

ws, err := websocket.Dial(wsurl, "", "https://api.slack.com/")