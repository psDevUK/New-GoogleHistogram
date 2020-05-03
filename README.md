# New-GoogleHistogram
Google Histogram Chart UniversalDashboard based on https://www.npmjs.com/package/react-google-charts

# Show Some Respect
If you are just using universaldashboard.community version think about putting your hand in your wallet/purse and purchase
the enterprise edition. I personally own 2 licenses for this, as I think the software is extremely well priced and I know that
by buying the product I am helping the developer carry on making the product even better. So please show some respect and go buy
your copy of Enterprise edition. I can certainly vouch it is well worth the money and I have been able to produce my company
high-end looking dashboards using just my powershell skills, to display the desired information.

## New-GoogleHistogram
To my knowledge this chart does not yet exist in **UniversalDashboard** so I am bring in the new charting component right here.
So when I first saw this chart and the demo, I kind of skipped past it not seeing the full potential of it.  After going on a 
mad one this week and this being the 6th component I am publishing to github in as many days. I was running out of Google charts
to turn into components, as I released a few that I teamed up with someone else to do.
 After looking at this chart several more times, I was thinking this would actually be really useful for my sales teams, if I say
grouped a few weeks worth of sales by products...you could then easily see all the products that are not selling that well. So
putting my geek cap on I thought of a good non-sales way to use this chart which I will show in the demonstration
 
## Parameters
Really simple, only 2 additional parameters apart from the default Id  parameter and they are:-

* **-Data** 
This is a 2-dimensional-multi array which has to be passed, a name and a value, this has to include headers, see demonstration

* **-Color**
Allows you to specify a string colour for the chart. The default is set to blue should you wish not to use this parameter

## Demonstration

Thought it would be good in this demo to show you how you can pull this information from a CSV file....for demo purposes I will
also create that CSV on-the-fly using the get-process command so that this example dashboard should work on any PC, and will
easily show you which processes are hogging the CPU.  Please note I think this only supports one decimal place. So please be 
careful with those decimals.

```
Import-Module -Name UniversalDashboard.Community -RequiredVersion 2.8.1
Import-Module -Name UniversalDashboard.GoogleHistogram
Get-UDDashboard | Stop-UDDashboard
Start-UDDashboard -Port 10005 -Dashboard (
    New-UDDashboard -Title "Powershell UniversalDashboard" -Content {
        $MultiArray = @()
        Get-Process | Sort-Object -Unique Name | Select-Object Name, @{Label = "CPU"; Expression = { [math]::Round($_.CPU, 1) } } | Export-Csv C:\UD\Test.csv -NoTypeInformation
        Import-Csv C:\UD\Test.csv | % { $MultiArray += (, ($_.Name, $_.CPU)) }
        New-UDLayout -Columns 2 -Content {
            New-GoogleHistogram -Data @($MultiArray) -Color 'green' -Title "Process usage by CPU"
        }
    }
)
```
![placeholder](https://raw.githubusercontent.com/psDevUK/New-GoogleHistogram/master/GoogleHistogram.gif
"Demonstration Chart")
  Boom there you have it the Google Histogram Chart for UniversalDashboard
