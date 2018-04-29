---
title: "Self-Updating Marketing Dashboard on Google Spreadsheets"
date: 2018-04-29
draft: false
tags: ["Learning", "NOC Internship", "Work", "Google Spreadsheets", "Automation","Project" ]
categories: ["Learning", "NOC Internship", "Work", "Google Spreadsheets", "Automation","Project" ]
---

# How I created an automated marketing dashboard on Google Spreadsheets

## Background

Perhaps I should first provide some background/context on this project that I worked on. For those that don't know, I am interning at a commercial real estate tech startup in NYC for a year as part of a school programme that my university provided. 

So when I first entered my internship company, I noticed that sales/marketing data were all being tracked on Google Spreadsheets. This is common for a startup however there were a few issues:

1. **It isn't automated** - This meant that someone (the previous intern before me), had to manually update the dashboard weekly by pulling the formulas across, cross-referencing marketing spend numbers from various platforms and copying them back to this main dashboard that we had on Google Spreadsheets. 
2. **It was clunky and slow** - The number of formulas used only increased as the weeks passed. There was a point in time when the spreadsheet was linking 5/6 different spreadsheets, and I swear I was so afraid to type in anything on the spreadsheet in fear of something breaking/crashing. Typing in a single cell would take minutes as I had to wait for formulas to load.
3. **It was prone to errors** - This was a major problem. It was difficult to identify and fix errors as well when everything was running so slowly.

After a while, maintaining the dashboard became a chore and would take me more than an hour each time just to clean up and ensure that every formula was linked correctly. 


## Time for change
This was not sustainable with the number of paid users coming on board every week and it was time to redo the way we were tracking these numbers. It took me a while to figure out and learn, but eventually I created a dashboard that was 100% self-sustaining (although I still do a quick 5-minutes check-in on it weekly to ensure everything has been updated smoothly)

# Creating the Automated Dashboard

## Requirements
Let's work backwords. First we identify what we want to track, and then we find out how we can get the data to track these numbers. 

| Purpose        | Metrics           | Source of Data      |
| ------------- | :-----------: | --------: |
| Sales Reports   | Number/Revenue from Paid Users, Revenue lost from downgrades/churn, Revenue gain from upgrades | Spreadsheet itself (manual input)     |
| Marketing Reports     | Marketing Spent, CPL, Conversion Funnels, Lead-Paid %      | Adwords, Google Analytics, Facebook Ads, Bing Ads, LinkedIn Ads       |