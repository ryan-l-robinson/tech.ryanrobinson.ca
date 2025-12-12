---
title: 'Microsoft 365: Tools for Recognizing Sensitive Data'
date: '2021-09-27T07:46:00-04:00'
author: 'Ryan Robinson'
description: "It's important to be able to recognize sensitive data. Here are some tools to help."
tags:
  - Microsoft 365
  - Security
---

There are a handful of tools in Microsoft 365 to help you with information protection. Which one is the best option will depend on the scenario you will need to use it for. Once you’ve identified sensitive data using one of these tools, you can use it apply compliance rules like blocking sharing outside the organization or retaining for a certain number of years.

## Sensitive Information Types (DLP)

Sensitive information types, the backbone of data loss protection (DLP), relies on pattern recognition. Many sensitive types of data such as credit cards, passport numbers, and social insurance numbers have strict formatting requirements. These formatting requirements can be used to help identify them when they appear in a document, email, Teams chat, etc.

Many common sensitive information types are already defined in Microsoft 365. If there’s something missing, you can add your own custom types.

## Document Fingerprint

Document fingerprints are valuable for documents that were created from a shared template. If you use a template document as the starting point for all invoices, and you want to implement some compliance policy on all invoices, a document fingerprint is the answer. This tool identifies the sensitivity of the document based on recurring word patterns.

## Keyword Dictionary

A keyword dictionary is useful when you want to enact a policy on any instances of a specific set of data, where that specific set of data may change occasionally. A good example for this is employee IDs. Occasionally you’ll need to add a new employee ID or remove an old employee ID from the dictionary, but you always want to keep anything with those IDs protected.

## Trainable Classifier

The trainable classifier is a newer tool within Microsoft 365. Unless the other types, the trainable classifier does not depend on you being able to explicitly define what is and is not the definition of the data type. Instead, it relies on machine learning.

This makes it a great candidate for scenarios like resumes. Often HR will want to hold resumes of job applicants for a certain period of time but then have them automatically deleted. In the meantime they don’t want those resumes to ever be shared outside the organization. But a resume isn’t clearly defined: there are no guarantees of having the exact same words in a sequence, or templates, or strict numerical formats. Humans just know what a resume looks like because we’ve seen lots of them.

Trainable classifiers will apply that same approach but with machine learning. You’ll need to provide many examples of what the data type looks like and occasionally monitor it to see if it missed anything or flagged anything it shouldn’t.
