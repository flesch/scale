# Scale
Scale a Heroku web or worker process and notify a HipChat room about it.

## Installation

```
$ [ -d bin ] || mkdir bin; cd bin; curl -sO https://raw.github.com/flesch/scale/master/bin/scale; chmod +x scale
```
The above will drop `scale` into your app's `bin` folder.

## Setup

While `scale` does allow you to pass your Heroku and HipChat keys on the command line, it's probably easier to set these up as environmental variables.

```
$ heroku config:add HEROKU_API_KEY=[key] HIPCHAT_API_KEY=[key] HIPCHAT_ROOM_ID=[id]
```

## Example

```
$ [heroku run] bash bin/scale --app [name] --web [n] --heroku [key] --hipchat [key] --room [id] --force
```

If you're not using the Heroku Scheduler below, you don't need to run `scale` on a terminal attached to your Heroku app.

## Scheduler

While you can use `scale` as a wrapper around `heroku ps:scale`, it really shines when you use the [Heroku Scheduler](https://addons.heroku.com/scheduler)[^fn-scheduler] addon to auto-scale your app.

After adding the Heroku Scheduler addon, create 2 jobs (one to scale up and one to scale down). By default, the script won't run on the weekends[^fn-weekends]. If you need to run on the weekend, use the `--force`.  

## Help

```
         --app    # Your Heroku app name.
                  # This can also be set as an environmental variable: HEROKU_APP_NAME
       --force    # Include --force to always run the script, even on weekends.
     --web [n]    # Scale the web process.
  --worker [n]    # Scale the worker process.
      --heroku    # Your Heroku API key.
                  # Find your API key here: https://dashboard.heroku.com/account
                  # This can also be set as an environmental variable: HEROKU_API_KEY
     --hipchat    # Your HipChat Auth token.
                  # Find or generate a token here: https://hipchat.com/admin/api
                  # This can also be set as an environmental variable: HIPCHAT_API_KEY
   --room [id]    # Specify which HipChat room to post to.
                  # Find the Room ID here: https://hipchat.com/rooms/ids
                  # This can also be set as an environmental variable: HIPCHAT_ROOM_ID
```
## License

Released under the MIT License: [http://flesch.mit-license.org](http://flesch.mit-license.org)


[^fn-scheduler]: About the [Heroku Scheduler](https://devcenter.heroku.com/articles/scheduler).
[^fn-weekends]: My team has specific scale requirements. Weekday mornings we scale to 2 web processes, and then back to 1 at night. Our app isn't used on the weekends, so we don't scale at all.
