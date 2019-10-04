---
title: "Alerting long commands in zsh"
date: 2019-08-31T19:35:54-07:00
draft: false
---

Update (2019-10-03): I fleshed this out into a plugin! Check it out
<https://github.com/kevinywlui/zlong_alert.zsh>!

I often find myself running commands that take a long time and then coming back
to it later. For example, when I'm running a long computation for my research
or when I'm building a package. I recently discovered this [zbell
gist](https://gist.github.com/oknowton/8346801) which seems to be almost
exactly what I want! I'm trying to learn more shell scripting so I'm going to
reimplement this and explain what I'm doing. The final version will be this


{{< highlight bash>}}
# Ignore commands if they start with the follow
zlong_ignore_cmds='vim ssh'

# Define what a long duration is
zlong_duration=60

# Need to set an initial timestamps otherwise, we'll be comparing an empty
# string with an integer.
zlong_timestamp=$EPOCHSECONDS

# Define the alerting function, do the text processing here
zlong_alert_func() {
    local cmd=$1
    local secs=$2
    local ftime=$(printf '%dh:%dm:%ds\n' $(($secs / 3600)) $(($secs % 3600 / 60)) $(($secs % 60)))
    notify-send "Done: $1" "Time: $ftime"
    echo "\a"
}

zlong_alert_pre() {
    zlong_timestamp=$EPOCHSECONDS
    zlong_last_cmd=$1
}

zlong_alert_post() {
    local duration=$(($EPOCHSECONDS - $zlong_timestamp))
    local lasted_long=$(($duration - $zlong_duration))
    local cmd_head=$(echo $zlong_last_cmd | cut -d ' ' -f 1)
    if [[ $lasted_long -gt 0 && ! -z $zlong_last_cmd && ! *"$cmd_head"*==zlong_ignore_cmds ]]; then
        zlong_alert_func $zlong_last_cmd duration
    fi
    zlong_last_cmd=''
}

add-zsh-hook preexec zlong_alert_pre
add-zsh-hook precmd zlong_alert_post
{{< /highlight >}}

# Overview

The goal of this script is to alert me whenever a command, that has taken over
60 seconds, is complete. I will be use `notify-send` to alert me and also send
a [bell character](https://en.wikipedia.org/wiki/Bell_character).

For my particular desktop setup, `notify-send` will alert me via `dunst` and
the bell will set my `kitty` terminal to urgent which `i3wm` will pick up on
and turn the workspace indicator red.

So my alert function will send me the command that was ran, the duration
formatted nicely, and a bell character.

{{< highlight bash>}}
zlong_alert_func() {
    local cmd=$1
    local secs=$2
    local ftime=$(printf '%dh:%dm:%ds\n' $(($secs / 3600)) $(($secs % 3600 / 60)) $(($secs % 60)))
    notify-send "Done: $1" "Time: $ftime"
    echo "\a"
}
{{< /highlight >}}


## Timing commands

The `zsh/datetime` module gives us the `EPOCHSECONDS` parameter which is the
number of seconds since the unix epoch. To determine the duration, we just need
to subtract `EPOCHSECONDS` when the command is complete from `EPOCHSECONDS`
when the command started. We will use `zsh` hooks for this.

## Zsh hooks

One of the advantages of `zsh` over `bash` is that you get
[hooks](http://zsh.sourceforge.net/Doc/Release/Functions.html#Hook-Functions).
For this script, we're interested in 

- `preexec` which ''executed just after a command has been read and is about to
  be executed.''

- `precmd` which ''executed before each prompt.''

{{< highlight bash>}}

zlong_alert_pre() {
    zlong_timestamp=$EPOCHSECONDS
    zlong_last_cmd=$1
}

zlong_alert_post() {
    local duration=$(( $EPOCHSECONDS - $zlong_timestamp ))
    local lasted_long=$(( $duration - $zlong_duration ))
    if [[ $lasted_long -gt 0 ]]; then
        zlong_alert_func $zlong_last_cmd duration
    fi
}

add-zsh-hook preexec zlong_alert_pre
add-zsh-hook precmd zlong_alert_post
{{< /highlight >}}

## Ignore some commands

I use `vim` and `ssh` often and it doesn't really make sense to count them as a
long command. I store the commands I want to ignore in a comma separated list
at the top, I can extract the head of a command as follows: 
{{< highlight bash>}}
cmd_head=\$(echo $zlong_last_cmd | cut -d ' ' -f 1)
{{< /highlight >}}
and substring testing is done using the star wildcard 

`*"$cmd_head"*==zlong_ignore_cmds`

## Fixing a zbell bug

In the `zbell` gist, there was a super annoying bug! If you run `ls`, wait long
enough, then press ctrl-c, it'll alert you! This is because `preexec` did not
get a chance to run again but `precmd` did so it'll count the duration as the
time since the `ls` command. This was fix by setting `zlong_last_cmd` to be the
empty string at the end of `preexec` then checking to see if the string is
non-empty before sending it to the alert function.

# Alternatives

One alternative to alerting the end of long commands that I should mention is
using Pushbullet. Pushbullet is an app that I have installed on my phone that
can send me notifications. The best part about Pushbullet is that it has an
API. I've use <https://github.com/Red5d/pushbullet-bash> to send me an alert of
my phone whenever a long computation I was doing for my research has finished.

Another cool `zsh` feature is `REPORTTIME`. This won't alert you but it'll tell
you how long a command has run for. For example, by setting `REPORTTIME=3`,
`zsh` will tell me the run duration of any command that takes more than 3
second.
