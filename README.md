# Custom Splunk Shodan Command
This is a quick and dirty custom Splunk command to take a search query and retreive the results from Shodan's API. Results are returned directly in Splunk where they can be further processed.

Only setup required is dropping your API key in the shodan_configuration.csv lookup which is already present in the app. From there, follow the syntax below to use :)

> | shodan search_string="<SEARCH_STRING>"

## Demo
![Alt text](Demo1.png)

The command can be used to search Shodan and return the results in json format to the `data` field

---
![Alt text](Demo2.png)

Passing a `collect` command after the `shodan` command allows you to save the output data to an index where it can be further searched and extracted

---
![Alt text](Demo3.png)

Data saved to an index will appear in the natural json format, you may choose to do additional extractions via spath to search specific field data

---
![Alt text](Demo4.png)

See above for an example dataset in the sample dashboard. The source code for this dashboard is provided in this repository if you need something to get you started :)

> NOTE: If you are using `spath` to parse out large results you may need to create a local limits.conf file and specify a higher `extraction_cutoff` value to prevent clipping of data. This process is described here: https://docs.splunk.com/Documentation/Splunk/9.4.2/SearchReference/Spath
