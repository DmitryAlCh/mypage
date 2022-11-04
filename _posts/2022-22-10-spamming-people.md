---
layout: post
title:  "Poorman's email marketing"
date:   2022-10-22 18:17:11 +0200
categories: [linux]
last_edit: 2022-10-22
published: false
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
[Mutt](http://www.mutt.org/). I have `Node.js` at my day job, so had to select `Mutt`.
How to set-up Your mutt, You can read [on Medium](https://mritunjaysharma394.medium.com/how-to-set-up-mutt-text-based-mail-client-with-gmail-993ae40b0003)
or [someone's personal blog](https://www.garron.me/en/go2linux/send-mail-gmail-mutt.html).
The only part those tutorials not covered, is how to send formatted text. It turned out to be easy,
just add `set content_type="text/html"` to `.muttrc` file, otherwise it will be plain text only.

### The email-message
The e-mail text istelf should be saved as `html` file. For My "spamming" purposes 
I have been using [zohomail](https://www.zoho.com/mail/), which allows to see the generated 
`html` of the message I have created, so copy to file, and no need to touch `CSS`.
**gmail** has `show original` on reveived or sent messages. 
So get the message saved `message.html`

### The loop
Need a script to read file with `emails.scv`, keeping it simple, just one tab, 
no header row, and not empty lines. For every line, script would call `mutt` with 
correct arguments.
Almost ready script existed on the [internet](https://www.baeldung.com/linux/csv-parsing#parsing-csv-file-into-a-bash-array)
already, took it and added `mutt` call line.
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
`test-list.csv` - the file with recipients


### Recap


