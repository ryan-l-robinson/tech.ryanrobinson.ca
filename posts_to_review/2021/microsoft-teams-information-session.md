---
title: "Microsoft Teams: Information Session"
date: 2021-10-12T19:42:00.000Z
author: Ryan Robinson
tags:
  - Microsoft 365
---

I recently gave a presentation introducing some key concepts and answering questions around Microsoft Teams. The context was for those who already have accounts and have used it throughout Covid-19. They didn’t need the day-to-day basics like how to start a meeting, but they needed a better understanding of how all the pieces fit together and when it is best to use what. It was not a full consultation process, but I did put out a call for questions a couple weeks in advance and received several that I made sure to integrate into the presentation.

I’ve edited my notes to be a bit more generic and included those below:

## Caveats

A couple caveats before I begin getting technical:

1. Teams is massive. One of the hard things with talking about Teams is that you can’t easily compare it to just one competitor. It’s a lot of tools rolled into one. And it’s just one part of Microsoft 365. We aren’t going to cover nearly everything that is possible in 45 minutes. I’m going to hopefully give enough of an introduction answering some key questions I’ve heard come up a few times.
2. One of the other hard things talking about Teams is that Microsoft is kind of bad at branding. They use a lot of the same words repeatedly to refer to related but not exactly the same things. So you do get scenarios like having a Teams app on your device which has the Teams app in the sidebar which has multiple Teams in it and all of those Teams are associated with SharePoint Team sites. Don’t feel bad if somebody says a term and you don’t know what it means.

## Big Picture

This is an older diagram that Microsoft has used in the past and I have held on to because I think it is a helpful explanation of the big picture. There are a lot of tools in Microsoft 365 that overlap, but this summarizes pretty well what the most important value is for the big communications pieces.

![Communication Loops: Teams, Yammer, Outlook, SharePoint, Groups](/assets/img/2021/09/Communication-Loops.jpg)

Your inner loop is Teams. This is where you’re going to spend the bulk of your conversations with the people that you work with the most, like your department.

The outer loop is Yammer. It’s an enterprise social network. [Where it can be good is to have organization-wide conversations, especially more casual conversations.](/posts/2021/recommended-yammer/) That’s as much as I’m going to talk about it today.

Email is for targeted communications. It’s also ubiquitous. You’re going to need it sometimes, but if you’re talking to that inner loop, you might want to pause and consider whether it would work better in Teams or not before you send an email.

In the middle of everything is SharePoint. SharePoint can get a lot more complicated than this but for today, you can think of SharePoint as shared file storage.

Underneath it all is Microsoft 365 Groups. These provide the permission structure that spans across all those tools and some more. If you are added to the Group, you get access to all the tools defined for that group: Teams, a shared email, shared calendar, SharePoint files, Planner for task management, and more. Generally speaking, in a large organization, only IT and a few more can create Groups. That’s done intentionally so that there isn’t too much sprawl of thousands of groups and everybody using a different strategy for them. Owners of the groups usually have some control over who is in the group as well as some other settings. Exactly who can control what setting is often determined by a policy set by IT.

## Teams Overview

With that big picture out of the way, I’m going to do a quick walk through of what you see in the Teams app.

Search bar: across the top in the middle is a search bar. This can be used to search across everything in Teams, including people, files, and messages.

It does not search across other aspects of Microsoft 365, such as SharePoint sites not associated with teams or associated with teams you aren’t in. There is a relatively new tool called Microsoft Search that has been slowly rolling out over a few years. That is available in some other places and gives you results from everywhere in Microsoft 365. I will quickly demo that later. The Teams search does not do that yet but probably will eventually. For now, it just gives you results from Teams.

Settings: hidden in the little ellipses menu in the top right are settings. Teams gets updates very often, so I do recommend looking in here every few months because you might find something useful that’s new that would help your work.

Profile: beside that is your profile photo. This is your Microsoft 365 profile. On the desktop and mobile apps, you can also add a personal Teams account. I won’t get into that, but it is also an option.

Running down the main tab menu on the left, this is most often called the app sidebar or app bar. On desktop you can rearrange this menu by clicking and dragging an icon into a different position, but possibly with some limitations set by IT. If you can’t rearrange something, or you do but then it reverts back later, that’s because of [IT policy](/microsoft-365/microsoft-teams/microsoft-teams-app-policies/).

The Activity tab is essentially your notification hub. Anything that has sparked a notification across all your Teams will show up in the Activity section. If you think you missed a notification, this is a good place to look. Treat it the way you treat your Outlook inbox, as the place you check regularly for anything you may have missed. There are some options here like being able to filter your notifications against a search term, and if you right-click on a notification you can mark it as unread. The way I work at least, that can be a useful way to remind myself to come back to it later, if I still have unread notifications.

Chat and Teams I will break down a bit more in a minute, but that’s a lot of the heart of the communication system.

Calendar: the Calendar section is what you would expect from the name. It shows your calendar. Where it is especially useful is with meetings that happen in Teams. You can schedule meetings in here or join meetings previously scheduled from here. That also includes webinars with features like registration forms for people to sign up, and live streamed events which can be viewed by the public.

Files: this is another window into your files. They can be files across your teams as well as in your individual OneDrive. Sometimes you can also add other cloud services like Google Drive and Dropbox in here, but that’s determined by organizational policy.

Calls: the Calls section, in my opinion, is not that useful unless you’re using the [Teams phone system](/posts/2021/microsoft-teams-phone-numbers/). With that, your calls from regular phones would ring in Teams, which could be on a computer or a dedicated phone device. If you aren’t using Teams phone systems, this Calls section is a pretty basic dashboard of recent calls and speed dial.

Then there’s the ellipses. There are plenty of other apps available to install within Teams. Teams really is a platform in its own right, not just an app. Some of these apps that can be added are from Microsoft, some from other providers, and your organization could even build your own app and deploy it just for you. Depending on your workflow, some of these other apps may be helpful to you, so it is worth taking a look at what is available.

## Chat vs Teams

The biggest question I have heard when it comes to communication in Teams is when to use the Chat area and when to use the Teams area.

I’ll do Chat first because it is a bit simpler. You can think of Chat as something like Skype or Skype for Business. You can send short text chats and have audio and video calls with one other person or a small group of people. You can even message people outside your organization if they are using Teams, Skype for Business, or consumer Skype.

It appears as one long running conversation. If I start talking to somebody about one issue, then we switch to a different issue, then go back to the first issue, it’s all going to be jumbled together, so there isn’t an easy way to say “show me everything about this one issue.” The other important detail is that these conversations are visible only to the people in the conversation.

The Teams section is much more extensive. If you’ve used Slack, it’s the closest comparison. It’s a lot more than just Chat. It’s more of a complete project dashboard for a group.

You can be a part of multiple Teams. There is technically a limit but it is very large.

Each Team can have multiple channels. Each channel has threaded conversations, which makes it a lot easier to keep track of multiple conversations happening at once, or to find them later using search. Exactly how to divide up the channels will vary by the team. In my demo channel, we essentially have a channel for each project or technical system we maintain.

Each channel is set up with some tabs at the top. Tabs allow you to add different content, which could be in Microsoft 365 or could be somewhere else. That’s part of how it really becomes a project dashboard. The idea is that if somebody new were to join the project, they would be able to look at this one screen and they would see everything they need to see: past conversations about the project, task lists, documentation. None of that is available if all the conversations happen in Chat instead. To add new tabs, you just use the little + button. As with the apps in the sidebar, there are a lot available.

There’s also a handful of other useful features on the Team that you don’t get with Chat. One of my favourites is that you can share an email to a channel, if the team’s policy allows it. This is great in scenarios like getting an email but you need to consult with your group before you respond, so that you can coordinate who is going to answer what. There is a button in Outlook that can do that, or you can look up the email address for a channel and forward an email to it.

So to summarize: Chat is good when you need to message somebody outside your organization or somebody who you don’t have regular ongoing work with as part of a Team. But the Team should be the default for most conversations with that “inner loop” of people you work with regularly, so that you can have the full advantages of the project dashboard approach.

## Notifications

One of the questions I got was about notifications, specifically how to know when the person you are trying to contact will be notified and when they will not.

One way people get notified is if they are subscribed to a channel. Anybody can choose to subscribe to any channel they are a part of. Then they will be notified based on the settings they choose. I recommend this for anybody who is effectively a project manager to be subscribed to all conversations on the channel. Those people won’t necessarily need to respond to every conversation, but they probably should keep a high-level overview of what’s happening.

If the person you are messaging isn’t subscribed, or if you don’t know whether they are subscribed, you can still notify them using an @ mention:

- @ mention them individually. My tiny tip with this is that after you select the name of the person to @, you can backspace and delete their last name with the @ still working. It’s just a little easier to read and a little more personal.
- @ mention a tag. Tags are set by the team owner. They can be used to specify a subset of the team to easily notify, especially useful when you have big teams that may not always know each other. Say somebody new joins the team and they have a question about a certain subject area, but they don’t know who specifically to ask; they could use the tag instead.
- @ mention the whole group or a channel.

People will also receive a notification if you respond to a conversation started by them. You don’t have to @ message the person if you are replying to their conversation.

And finally, they will be notified if you react (like) to a message by them.

So the rule of thumb is that if you are in doubt and you need somebody or a small group of people to see it, use an @ mention. But you don’t have to when responding to their message or if you know they’re subscribed to the channel.

## Files

I’m going to move on to the biggest set of questions I got which had to do with file management. How do I find my files? Where do I put a new file? Who has access to the file?

I’ll say upfront that there is a bit of a blessing and a curse scenario here. There are a lot of entry points to get to your files. The curse of that is that it can be a bit overwhelming. The blessing is that you have a lot of choices to decide what works best with your workflow and you can ignore the rest. I will talk through some of these options, but you don’t need to be comfortable with all of them.

You can use an analogy like travelling to the office. When we’re all coming to the office again, we’ll all have the same destination, but we all have different starting points – our houses – and different routes to get there. What matters is that we get to campus. What matters in Microsoft 365 is that we are working from the same files. How we access that file is not as important.

This is a vast improvement over the older model where you might end up with lots of copies of the same file floating around because they’ve been emailed as attachments and now everybody has their own copy. When you email a copy as an attachment, you aren’t sharing a destination. You all have your own destinations, your own copies of the files, which can result in different content in the file as people change their own copy over time. As a general rule, do not share a copy of a collaborative file as an attachment to anybody within your organization. You should only resort to attachments if you’re sharing external or if it’s a one-way communication where you don’t need to know if they make any changes to it.

What does help is a little more knowledge of the destination, the file system. This goes back to that diagram from the beginning when I said the file system underneath everything is SharePoint.

One of the questions I got a few times was OneDrive vs SharePoint. OneDrive really is just a simpler version of SharePoint with less features and different default permissions.

In many ways this parallels the Chat vs Teams discussion. Chat is for conversations owned by only a few participants while Teams is for bigger conversations owned by the groups with all the extra features.

OneDrive is for files owned by just you. SharePoint is for files owned by a group with all the extra features.

And the Teams distinction does line up with the file distinction.

If I’m having a chat with somebody and we put a file in the Files tab, where do you think that file is going to be saved? It will be in the OneDrive of the person who created it, shared with the other. The file is owned by who created the file, not a group.

If I put a file in the Files tab from a Teams channel, where do you think that file will go? It will go in the SharePoint site tied to the Team.

How about the video recording of this session right now? I created this meeting myself, sending an invite from Outlook. So who owns this meeting? I do. The recording will end up in my OneDrive.

What if we had a Team that included everybody here and I created this meeting from a channel on that Team? Then the file would be saved on the SharePoint site associated with the Team instead. The meeting is owned by the team and so is the recording.

In other words, Teams is not its own file system. Teams is built on top of SharePoint. Every Team is associated with a SharePoint site. When you look at that Files tab in a Team channel, that’s really a window into a specific folder on a SharePoint site. For the most part, you can do everything you need from Teams and you often don’t need to look at SharePoint directly for these team day-to-day project files. You do have to look at SharePoint directly if you have something like an intranet portal that isn’t linked to Teams, but it’s not unusual if most of the members of a specific team never actually look at the SharePoint site for that specific team directly.

It’s not Teams vs SharePoint. It’s Teams AND SharePoint. It’s Teams as one of the possible routes into the destination of SharePoint. If your channel is already that project dashboard and you’re already in there having conversations anyway, then using that as your main way to get to files is probably the easiest. But it’s not the only way. If you prefer other routes to your files, you can use them.

In some ways I know this can sound very complicated with all the different possible scenarios. But it really comes down to one question: who owns that conversation or that file? Teams is structured in such a way that once you are in the right spot for a conversation, all the files will also be lined up. If I’m having a conversation in one team about a specific project, I don’t need to stop again to think about where to put files related to that project. I can just go to the Files tab and know that everything in there is visible to the team.

If you’re going to make a new file and don’t know where to put it, think about who needs to be able to see the file. And I think it is important to qualify that as who will ever need to see it. Maybe 99% of the time it will be you using the file, but somebody else needs it when they need to cover for you while you’re on vacation. Or if something happens and you suddenly aren’t working starting tomorrow, will everybody else have everything they need to continue your work? So, a lot of things like documentation of processes, or files in a project that others may need to reference in the future, those should all be in SharePoint, typically through a Team.

There may be more things private to you that stay in your OneDrive. In my work, that means things like configuration files for all my developer environment tools, and a copy of the profile photo I use everywhere. Nobody else ever needs to see those. But most of what I’m doing is in SharePoint, through Teams, available to others if or when they need it.

Permissions in both OneDrive and SharePoint can get a lot more complicated than the defaults – you can share from OneDrive with others – but the more you stick to the defaults the easier your life is going to be.

## Syncing Files

That’s the most important piece, understanding the destination and that Teams is one route into that destination. But it’s not the only route. I’ll cover a few more ways to access files and I’ll reiterate what I said earlier: you don’t need to do all of these. You need to find which one or ones fit your workflow.

The next most common one is synchronizing your files. You may want to synchronize files that are in a SharePoint site to your computer. This makes it so that you have a copy on your computer and whenever a change happens in either the cloud version or your desktop version, the change will be synchronized to the other location so that everybody continues to be working from the same content even if technically there is a copy.

Why would you want to do this? There are three major scenarios:

1. If you don’t have reliable Internet and you need to access the file while offline.
2. If you have file types other than Office apps that you need to act on in other ways. For example, in my work, I might have a bunch of image files that I’m going to need to upload to a website. To do that, I’ll need the files on my computer.
3. And the last good reason is simply that you are used to browsing for files in Windows Explorer. You will still need to have enough understanding of the cloud locations in order to start the sync, but after that, you can have the benefits of it syncing to the cloud while still doing everything the way you’re used to doing it on your computer.

There are also now two ways to achieve this. The older way is the Sync button which is visible both in Teams and SharePoint in the browser. Hitting that will open the OneDrive app on your computer to set up the sync.

The newer way is a “Add shortcut to OneDrive” button which shows up in SharePoint but not in Teams.

It’s too big of a tangent to break down the advantages of each, but [I do have a post about it](/posts/2021/desktop-sync-files-onedrive-sharepoint/).

## Other Ways to Access Files

Those are the two ways most intuitive for most people to access their files: within Teams and synchronized to your computer.

But there are a lot more which I will mention but not demo all of them.

I mentioned [Microsoft Search](/posts/2021/microsoft-search-introduction/) earlier. There are a few entry points to Microsoft Search. One of them is a Bing search. Cue the jokes about somebody actually using Bing. But I have my default browser search set to Bing. The main reason is Microsoft Search. If I want to try to start a search, I can enter that in a new tab address bar like I would any other search, and part of the results is this “Work” or “School” section (depending on your organization type). That will give me lots of content across Microsoft 365 including documents, groups, people, Teams conversations, and so on.

A scenario where I use this a lot is if I’ve encountered a new bug on a website. I’ll do my search, and first I’ll look at the Work/School tab to see if this is a problem anybody in my organization has dealt with before. Maybe there’s a file documenting it, or maybe a Teams conversation about it. If I don’t find anything internally, I flip back to the All results tab and see what is available elsewhere.

And speaking of a new tab, if you use [Edge](/posts/2021/web-browser-edge-chrome/) as your primary browser, you can set your New Tab screen to show your Office documents. I like this because I open new tabs a lot and sometimes I’ll quickly glimpse that a colleague has edited a shared file relevant to me, so it gives me the extra prompt to look into that.

There are a few other places that look more or less the same as this. You can go to office.com. There is a Windows and a mobile app simply called Office. If you use Android, there’s a launcher called Microsoft Launcher that can show recent cloud documents. What those all have in common is that the focus isn’t necessarily on browsing everything, but they can prompt you to quickly reload a file that the Microsoft algorithm thinks you may want to see again.

You can also open files directly in the Office apps like Word and Excel, whether web, desktop or mobile. Those actually give you a bit of both where you can see the recommended files or you can browse through SharePoint to find something. If these files are housed in SharePoint or OneDrive, they do not also have to also be synced to your computer to open them in the desktop app. You can open directly from the cloud.

There are also OneDrive and SharePoint mobile apps, which can be good if you want to be able to browse everything a bit more freely or you need to do a lot more work on the go.

You can also browse SharePoint in your desktop browser, as I mentioned before, but there isn’t necessarily navigation to get from one site to another if your organization’s IT hasn’t set that up.