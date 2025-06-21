# Simple Attack Surface Management with Shodan - Directly in Splunk
This app is a combination of custom commands and dashboards that enables you to easily query Shodan directly in Splunk. The included dashboard allows for easy visualization of the data and even calls out CVE's present in your ASM data that are on CISA's Known Exploited Vulnerability list. 

The only setup required is dropping your API key in the shodan_configuration.csv lookup which is already present in the app. From there, follow the syntax below to use :)

> | shodan search_string="<SEARCH_STRING>"

That syntax will return the entire JSON blob that Shodan returns, if you want to break up the results into individual rows/events you can use this syntax:

> | shodan search_string="ssl:DOMAIN_HERE"
> | table data
> | spath input=data path=matches{} output=matches 
> | table matches
> | mvexpand matches

If you would like to save the returned results to an index you can add a collect command as well:

> | shodan search_string="ssl:DOMAIN_HERE"
> | table data
> | spath input=data path=matches{} output=matches 
> | table matches
> | mvexpand matches
> | rename matches AS _raw
> | collect index=main source=shodan

Finally, you might notice a CSV version of CISA's KEV is already included in the app. This might be outdated so it is recommended to use the custom `updatelookup` command to pull a fresh copy:

> | updatelookup url="https://www.cisa.gov/sites/default/files/csv/known_exploited_vulnerabilities.csv" lookup_name="kev"

Both items (searching/collecting Shodan data & updating the KEV) have saved searches included with the app that are disabled by default. You can enable those and both processes will automatically run daily.

## Demo
![Alt text](Demo1.png)

The `shodan` command can be used to search Shodan and return the results in json format to the `data` field

---
![Alt text](Demo2.png)

Passing a `collect` command after the `shodan` command allows you to save the output data to an index where it can be further searched and extracted. `Spath` is also used here to break up each result into its own event.

---
![Alt text](Demo3.png)

Data saved to an index will appear in the natural JSON format, running `| spath` after specifying the index/source/sourcetype will extract the fields.

> NOTE: If you are using `spath` to parse out large results you may need to create a local limits.conf file and specify a higher `extraction_cutoff` value to prevent Splunk from ommitting certain fields during extraction. 
> This is usually performed by creating the file `/opt/splunk/etc/system/local/limits.conf` (be sure to run `chown splunk:splunk limits.conf`) and adding the content below:

> [spath]

> extraction_cutoff = 5000000

> extract_all = true

> Save that file, restart Splunk and you should be golden. This process is also described here: https://docs.splunk.com/Documentation/Splunk/9.4.2/SearchReference/Spath
---
![Alt text](Demo4.png)

The included dashboard displays basic information about the results

Happy Hunting :)
