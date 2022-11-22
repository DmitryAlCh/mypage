---
layout: post
title:  "Poorman's email marketing"
date:   2022-11-22 08:17:11 +0200
categories: [linux]
last_edit: 2022-11-22
published: true
---

TLDR: `Mutt` and simple scripting.  
`> mutt -s "email subject" "recipent@whatever.com" < "message.html"` in a loop.  
Had to send unhealthy amount of emails. Was surprised that sending same email to 
a list of adresses is not a common feature in email-clients. Small journey. 

Although there are e-mail marketing platforms online, felt like it would be an overkill. 
All I need is to loop through 150 emails and send same formatted text to all, 
one by one. 

### The tool
The candidates were [NodeMailer](https://nodemailer.com/about/) and
[Mutt](http://www.mutt.org/). I have `Node.js` at my day job, so had to select
`Mutt`. How to set-up Your mutt, You can read [on
Medium](https://mritunjaysharma394.medium.com/how-to-set-up-mutt-text-based-mail-client-with-gmail-993ae40b0003)
or [someone's personal
blog](https://www.garron.me/en/go2linux/send-mail-gmail-mutt.html). The only
part those tutorials not covered, is how to send formatted text. It turned out
to be easy, just add `set content_type="text/html"` to `.muttrc` file,
otherwise it will be plain text only.

### The email-message
The e-mail text istelf should be saved as `html` file. For My "spamming" purposes 
I have been using [zohomail](https://www.zoho.com/mail/), which allows to see the generated 
`html` of the message I have created, so copy to file, and no need to touch `CSS`.
**gmail** has `show original` on reveived or sent messages. 
Some fancy text with formatting like this:
<p align="center">
    <img alt="formatted email text screenshot" src="{{site.base_url}}/assets/images/formatted-email-text.png" />
</p>
Becomes a `HTML` like: 
```
<div>
    <br>
</div>
<div style="line-height: 2;">
    <span class="size" style="font-size: 18.666666666666664px">
        Fancy email with
        <span class="colour" style="color:#00cc00">
        </span>
        <span class="highlight" style="background-color:#e5ccff">
            <span class="colour" style="color:#00cc00">
                colors
            </span>
        </span>
        <br>
    </span>
</div>
<div style="line-height: 2;">
    <span class="size" style="font-size: 18.666666666666664px">
        <span class="highlight" style="background-color:#ffffff">
            and
        </span>
        custom line-spacing
        <br>
    </span>
</div>
<div style="line-height: 2;">
    <span class="size" style="font-size: 18.666666666666664px">
        Regards.
    </span>
</div>
```
Saving it to `message.html`. (Yes, font-size looks odd)

### The loop
Need a script to read file with recipients `test-list.csv`. Keeping it simple, just one tab, 
no header row, and no empty lines. For every line, script would call `mutt` with 
correct arguments.
Almost ready script existed on the [internet](https://www.baeldung.com/linux/csv-parsing#parsing-csv-file-into-a-bash-array), 
took it and added `mutt` call line.
```
#! /bin/bash 
arr_csv=() 
while IFS= read -r line 
do
    arr_csv+=("$line")
done < test-list.csv

echo "Sending emails to selected list:"
index=0
for email in "${arr_csv[@]}"
do
    echo "Sending emai to: $email"
    mutt -s "test subject" $email < message.html
    sleep 5
	((index++))
done
```
Saving it to `send-in-loop.sh`

### Email list
`test-list.csv` - the file with recipients, adding just 2 of my emails 
```
> cat test-list.csv
dmi.al.ch@gmail.com
dmi.al.ch@zohomail.eu
```

### Recap
All files `message.html`, `test-list.csv` and `send-in-loop.sh` should be in one 
folder.
```
> ls -1
message.html
send-in-loop.sh
test-list.csv
```
Such a fancy output of `ls` command, mystic argument `1` does this.
Now all that is left is to call the script by
```
> bash send-in-loop.sh
Sending emails to selected list:
Sending email to: dmi.al.ch@gmail.com
Sending email to: dmi.al.ch@zohomail.eu
```
And received email looks same as expected:
<p align="center">
    <img alt="received email" src="{{site.base_url}}/assets/images/email-received.png" />
</p>

### Precautions
Need to have that `sleep 5` in bash script otherwise email provider will block You.
At least I gotten a temporary block on 10th email. 
Which is another reason to set up own email server, that is actually not a
straightforward task, so later.

