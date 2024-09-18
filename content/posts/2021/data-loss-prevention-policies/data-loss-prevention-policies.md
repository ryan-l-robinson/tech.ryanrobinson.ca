---
title: "Data Loss Prevention (DLP) Policies"
date: "2021-05-03T12:04:00-04:00"
tags:
  - "Microsoft 365"
  - MS-101
  - Security
---

Data Loss Prevention in Microsoft 365 is a feature that helps prevent loss of sensitive data (that makes sense) coming out of your system. This can be within emails or within files, although the latter requires a higher license. Here’s how it works.

## Sensitive Data Types

At the heart of the DLP functionality is the idea of sensitive data types and the ability to recognize those types automatically using pattern recognition.

A common example is a credit card. In general, you don’t want people in your organization to be able to share credit card information (especially not by email). Credit card numbers follow a particular pattern of 16 digits. That makes it easier for the Microsoft pattern recognition to detect.

Other patterns may not be as easy to identify, but Microsoft is able to draw on a massive amount of data points and come up with a level of confidence. It may come back saying that it is 70% confident that this document contains a credit card number.

You can also add your own sensitive data types and train them with enough data for the machine learning machine to establish some confidence in what does and does not meet that type. Perhaps you have your own internal system with ID numbers for clients that you want to protect in some way; you could set that up as a sensitive data type.

!["Current sensitive info types available by default"](./dlp-sensitive-information-types.png)

## DLP Rules

The next question is what you want to do when that sensitive data is identified. This is where the DLP rules come in. With DLP rules you can set multiple options:

- What sensitive data type(s) should the rule apply to
- Name of the policy
- What systems should the rule be monitoring, for example Exchange, OneDrive, SharePoint
- What to do when the data type is detected: notify the user, email an admin, and/or restrict access to the content

!["DLP policy setting options"](./dlp-policy-settings.png)

## Other Uses of Sensitive Data Types

These sensitive data types can also inform other types of security and compliance rules throughout your Microsoft 365. For example, you can have retention rules based on the sensitive data type scanner which says, for example: if the file has a passport number, retain for 7 years after last modification and then delete.

!["Making a retention tag based on sensitive information type"](./retention-tag.png)

## Learn More

This is a very broad introduction to the idea. To dig in deep to what is possible, [check out the Microsoft docs](https://docs.microsoft.com/en-us/exchange/security-and-compliance/data-loss-prevention/data-loss-prevention).
