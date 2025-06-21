# Custom Splunk Shodan Command
This is a quick and dirty custom Splunk command to take a search query and retreive the results from Shodan's API. Results are returned directly in Splunk where they can be further processed.

Only setup required is dropping your API key in the shodan_configuration.csv lookup which is already present in the app. From there, follow the syntax below to use :)

> | shodan search_string="ssl: apple.com"
