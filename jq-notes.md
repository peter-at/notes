
## top


## deep select

```
cat bcc-v8-capacity.json |jq '.. | ."target"?  | select(. != null)'
```

## reshaping element
```
time os hypervisor show "rr-cloud-r036n14-w.bcc.bloomberg.com" -c aggregates -c free_ram_mb -c memory_mb -c memory_mb_used -c hypervisor_hostname -f json |jq '. | {"bccZoneAggregate":.aggregates[0], "free_ram_mb":.free_ram_mb, "bccHypervisor":.hypervisor_hostname, "memory_mb":.memory_mb, "memory_mb_used":.memory_mb_used, "memory_used_pct":(.memory_mb_used/.memory_mb)}'
{
  "bccZoneAggregate": "prod-3",
  "free_ram_mb": 1308751,
  "bccHypervisor": "rr-cloud-r036n14-w.bcc.bloomberg.com",
  "memory_mb": 1546831,
  "memory_mb_used": 238080,
  "memory_used_pct": 0.1539146810478973
}

real	0m3.636s
user	0m0.958s
sys	0m0.205s
```

## https://lzone.de/cheat-sheet/jq
```
Example-wise the jq manpage is not really helpful. Let's document some simple examples here...

To test queries live use https://jqplay.org/

## Output Formatting

If you do only care about output formatting (pretty print) run

    jq . my.json

Note: for redirection you need to pass a filter too to avoid a syntax error:

    jq . my.json > output.json

## Simple Extraction

Consider this example document

    {
        "timestamp": 1234567890,
        "report": "Age Report",
        "results": [
            { "name": "John", "age": 43, "city": "TownA" },
            { "name": "Joe",  "age": 10, "city": "TownB" }
        ]
    }

To extract top level attributes "timestamp" and "report"

    jq '.[] | {timestamp,report}'

To extract name and age of each "results" item

    jq '.results[] | {name, age}'

Filter this by attribute

    jq '.results[] | select(.name == "John") | {age}'      # Get age for 'John'
    jq '.results[] | select(.name | contains("Jo"))'       # Get complete records for all names with 'Jo'


## Using jq in Shell Scripts

From https://www.terraform.io/docs/providers/external/data_source.html

### Parsing JSON into env vars

To fill environment variables from JSON object keys (e.g. $FOO from jq query ".foo")

    export $(jq -r '@sh "FOO=\(.foo) BAZ=\(.baz)"')

To make a bash array
   read -a bash_array < <(jq -r .|arrays|select(.!=null)|@tsv)
    
### JSON template using env vars

To create proper JSON from a shell script and properly escape variables:

    jq -n --arg foobaz "$FOOBAZ" '{"foobaz":$foobaz}'


### URL Encode
Quick easy way to url encode something
 
   date | jq -sRr @uri
```
