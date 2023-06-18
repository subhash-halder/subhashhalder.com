---
title: "How to Stay on Top of Your Gmail Inbox with AppScript"
description: "Gmail is a powerful tool, but it can be difficult to keep your inbox organized. AppScript is a JavaScript library that can help you automate tasks in Gmail, freeing up your time to focus on more important things."
author: subhash
date: 2022-05-23T18:00:00+05:30
categories: ["Productivity", "Gmail", "AppScript", "Organization", "Automation"]
tags: ["organize your gmail inbox", "use appscript to automate your gmail inbox", "how to stay on top of your gmail inbox", "the ultimate guide to organizing your gmail inbox with appscript"]
math: true
mermaid: true
---

Gmail is a powerful tool, but it can be difficult to keep your inbox organized. **Apps Script** is a scripting language like JavaScript that can help you automate tasks in Gmail, freeing up your time to focus on more important things.

## What is Apps Script?
Google Apps Script is a cloud-based scripting platform that lets you integrate with and automate tasks across Google products. It's a powerful tool that can be used to automate tasks. It's based on JavaScript, so if you know JavaScript, you'll be able to pick it up quickly. And if you don't know JavaScript, there are plenty of resources available to help you learn.

## Why not using Gmail filter?
Gmail filters are very effective for filtering and categorizing emails with simple terms like email patterns or simple regular expressions. However, they can be difficult to use when you need to filter for more complex patterns, such as if a pattern is only present in the subject line or if you are only mentioned in the email body.

## Why I needed this process?
I was getting a lot of email notifications from GitHub, but most of them were from the build process and were not that important. The sheer number of these notifications made it difficult to see the important ones. So, I thought of creating a filter that would categorize GitHub emails into two categories **github-important** and **github-not-important**. So I created two labels with the name, now how do i categorize the email that I am getting in Inbox? I have tried with simple filter option but it doesn't work for every case.

## How to write Apps Script?
To write google Apps Script you have to go the URL [https://script.google.com/](https://script.google.com/) and start writing the script. Give a name to the project so that you can search it easily afterword.


## Lets start with the script

Here is a simple script that will print 10 email subject and every email in the thread from *notifications@github.com*.

```js
function githubNotiCategorizer() {
  const query = `from:(notifications@github.com) label:inbox`;
  const emails = GmailApp.search(query, 0, 10);
  for(const email of emails) {
    const subject = email.getFirstMessageSubject();
    console.log("Message Subject", subject);
      const messages = email.getMessages();
      for (const message of messages) {
        const messageBody = message.getBody();
        console.log("Message Body: ", messageBody)
      }
    }
  }
}
```

Copy paste the above code in the **editor** of the script project, to run it select the function name in the toolbar drop down and click the run button, you will be able to see the logs of the email from GitHub notification (if any).

## Next step
If you are able to run the code then you are few step away from organizing your GitHub notification.
1. Create two Gmail Label like **github-important** and **github-not-important**.
2. We need to modify the script to distribute the notification between two label and we will remove the inbox label so inbox doesn't contain these emails.
3. Schedule this code to run every 1 hour ( customizable as your need ).

## Final Script
```js
function githubNotiCategorizer() {
  const query = `from:(notifications@github.com) label:inbox`;
  const emails = GmailApp.search(query, 0, 10);
  const myNameRegex = /subhash/i; // will match if have my name.
  const subjectFilterRegex = /^((?!(run failed)).)*$/i; // will match if "run failed" is not available.
  for(const email of emails) {
    let isImportant = false;
    const subject = email.getFirstMessageSubject();
    if(subject.match(subjectFilterRegex)) {
      isImportant = true;
    }
    const messages = email.getMessages();
    for (const message of messages) {
      const messageBody = message.getBody();
      if(messageBody.match(myNameRegex)) {
        isImportant = true;
        break;
      }
    }
    if(isImportant) {
      email.addLabel(GmailApp.getUserLabelByName('github-important'));
      email.moveToArchive(); // remove the inbox label so it will not be shown in the inbox
    } else {
      email.addLabel(GmailApp.getUserLabelByName('github-not-important'));
      email.moveToArchive(); // remove the inbox label so it will not be shown in the inbox
    }
  }
}
```

## Scheduler
Now you have a script that can categorize your GitHub notifications, but how can we schedule it so it can run on new messages. Google script has that feature also you can activate the scheduler to run this function every hour ( as frequent you needed ) with the following script. Running the script by selecting the function name from the dropdown and pressing the play button will create a trigger.

```js 
function setTrigger() {
  // This will run the **githubNotiCategorizer** every hour
  ScriptApp.newTrigger('githubNotiCategorizer').timeBased().everyHours(1).create();
}
```

I the left panel of the script editor there is a menu of trigger which will have all the trigger, you can see your trigger is registered over there.
You can manually create the trigger also without writing the above code.


Now you know how to do it, you can do all sorts of cool automation with app script. I will write more about app script later, please let me know in the comment if you have any cool idea.

