---
title: "Marketing Dashboard on Google Spreadsheets"
date: 2018-04-29
draft: false
description: "How to create automated dashboards on google spreadsheets"
tags: ["Learning", "NOC Internship", "Work", "Google Spreadsheets", "Automation","Project","Marketing", "Google App Scripts" ]
categories: ["Learning", "NOC Internship", "Work"]
---

# How I created an automated/self-updating marketing dashboard on Google Spreadsheets

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
| ------------- | :----------- | -------- |
| Sales Reports   | Number/Revenue from Paid Users, Revenue lost from downgrades/churn, Revenue gain from upgrades | Spreadsheet itself (manual input)     |
| Marketing Reports     | Marketing Spent, CPL, Conversion Funnels, Lead-Paid %      | Adwords, Google Analytics, Facebook Ads, Bing Ads, LinkedIn Ads       |

From there we will be able to identify a list of things to do:

- [ ] Retrieve raw data from Adwords, FB, LinkedIn, Bing, Google Analytics
- [ ] Ensure that data is input in the right location on the spreadsheet
- [ ] Set-up trigger to run the script weekly (auto-update)


## Retrieving raw data from the available APIs

### Adwords
I used this [Adwords Script provided by Google](/Users/brenda/Documents/Self Learning/PersonalBlog/quickstart/public/tags/getting-started/index.html) to automatically pull the daily spend, clicks and conversions for the company's Adwords account

### Facebook Ads
Only the first 17 lines would be relevant as those help to pull the data out. The subsequent codes just shows where the data would be written to. 
```javascript
function pull_last_week_fb_data() {
  //call the api, to schedule for weekly
  var response = UrlFetchApp.fetch("https://graph.facebook.com/v2.12/act_108344393100552/insights/me?access_token=<YOUR ACCESS TOKEN>&level=account&date_preset=last_week_mon_sun&fields=impressions,spend,actions");
  //Logger.log(response.getContentText());
  
  var json = response.getContentText();
  var data = JSON.parse(json);
  spend = data.data[0].spend;
  actions_array = data.data[0].actions;

  var num_leads = 0;
  for (var idx in actions_array){
    entry = actions_array[idx]
    if (entry.action_type == "leadgen.other") {
      num_leads = entry["value"]
    }
  }

    //extra code for me to write the data into the spreadsheet
    //use logger.log() for testing and debugging
  var ss = SpreadsheetApp.openByUrl(<URL OF SPREADSHEET YOU ARE USING>);
  var sheet = ss.getSheetByName(<NAME OF SHEET DATA IS GOING TO BE WRITTEN TO>);
  var last_week_col_num = sheet.getRange(32,2).getValue();

  sheet.getRange(41,last_week_col_num).setValue([spend]);
  sheet.getRange(42,last_week_col_num).setValue([spend/num_leads]);
  sheet.getRange(43,last_week_col_num).setValue([num_leads]);
}
```

### Google Analytics
Even though GoogleSpreadsheets has the GoogleAnalytics add-on, I found that the data pulled from there doesn't reflect what is shown on the GA web app. After days of googling, I gave up and decided to pull the data manually. I can't remember who this was referenced from exactly but this was the code I used to pull the GA data.

```JAVASCRIPT
function myFunction() {
  getGAdata("gadata","gadataWEEK","ga:106932367","2018-01-01","yesterday","ga:isoWeek,ga:year","ga:users,ga:goal14Completions","ga:isoWeek")
  getGAdata("gadata","gadataMONTH","ga:106932367","2018-01-01","yesterday","ga:month,ga:year","ga:users,ga:goal14Completions","ga:month")
}

function getGAdata(reportName,sheetName,gaid,start,end,dims,mets,sort,opt_filters) {
  var query = {
    "optionalArgs": {
      "dimensions": dims,
      "max-results": "10000",
      "samplingLevel": "HIGHER_PRECISION"
    },
    "ids": gaid,
    "metrics": mets,
    "start-date": start,
    "end-date": end
  }
  
  if(sort){
   query.optionalArgs.sort = sort; 
  }
  if(opt_filters){
    query.optionalArgs.filters = opt_filters; 
  }
  
  var results = Analytics.Data.Ga.get(query.ids, query['start-date'], query['end-date'], query.metrics, query.optionalArgs);
  
  var sheet = SpreadsheetApp.openByUrl(<URL OF SPREADSHEET USED>).getSheetByName(sheetName);
  outputToSpreadsheet(results, sheet,reportName,results.containsSampledData);
}

function outputToSpreadsheet(results, sheet, reportName, sampleDataFlag) {
  //self-added
  //var ss = SpreadsheetApp.openByUrl(<URL OF SPREADSHEET USED>);
  //var sheet = ss.getSheetByName("gadata");
  
  // strings & nums
  var reportName = reportName||"n/a";

  // arrays
  var headerNames = [];
  
  // Grab the headers.
  for (var i = 0, header; header = results.getColumnHeaders()[i]; ++i) {
    headerNames.push(header.getName());
  }
  // Print the headers to top row
  sheet.getRange(1, 1, 1, headerNames.length)
      .setValues([headerNames]);
    
  // objs
  var lastRow = sheet.getLastRow() + 1;
  var gaRows = results.getRows();      //Get total amount of rows
  var formattedDate = Utilities.formatDate(new Date(), "PST", "yyyy-MM-dd' 'HH:mm:ss");

  // Print the rows of data.
  sheet.getRange(lastRow, 1, gaRows.length, headerNames.length)
      .setValues(results.getRows());
  
  // Debug Key
  // This is optional and something I like to use.
  // Print a random key to log API queries, consists of total amount of rows a query has
  // followed by a random number + date + report + the sample data flag. 
  // For example 50.098098707 6/28/2016 Report Name sample data: False 
  // means 50 rows and the random number that can act as a key  + the date it was pulled 
  // + the report name + if it includes sample data 
  // 
  sheet.getRange(lastRow, headerNames.length+1, gaRows.length, 1)
  .setValue(Math.random()+gaRows.length + " " + formattedDate + " " + reportName + " sample data:" + sampleDataFlag);
}

```

{{<figure src="/static/projects/automated-marketing-dashboard/example-of-ss.png" caption="Example of how the pulled data from Google Analytics appears on the spreadsheet" width="500">}}

---

{{<figure src="/static/projects/automated-marketing-dashboard/marketing-dashboard.png" caption="Snippet of the Final Dashboard" width="500">}}


Now the final step would be to set up triggers for the scripts to run automatically. This is easy. 

Under the script.google.com console, go to `Edit > All your triggers` and set the trigger.


> And now you have a marketing dashboard that updates automatically. Viola!

