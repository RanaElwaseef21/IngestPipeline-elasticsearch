# IngestPipeline-elasticsearch (Key , value )
Configuration of Ingest Pipeline on Elastic Search  Key value pair
to convert Input { "key": "triggerId", "value": "111" }, { "key": "msisdn", "value": "123345677" } 
Output { triggerId:111 , msisdn:123345677 }

**Foreach Processor**
 ```
 {
  "foreach": {
    "field": "layer.requestParameters",
    "processor": {
      "set": {
        "field": "layer.requestParameters.{{_ingest._value.key}}",
        "value": "{{_ingest._value.value}}"
      }
    },
    "ignore_failure": false
  }
}
```


**OR For each processor with new Parent**
```
{
        "foreach": {
          "field": "layer.requestParameters",
          "processor": {
            "set": {
              "field": "layer.newparent.{{_ingest._value.key}}",
              "value": "{{_ingest._value.value}}"
            }
          },
          "ignore_failure": false
        }

}
{
	"rename": {
	  "field": "layer.requestParameters",
	  "target_field": "layer.requestParametersOLDFORMAT",
	  "ignore_failure": true
	}
},
{
	"rename": {
	  "field": "layer.newparent",
	  "target_field": "layer.requestParameters",
	  "ignore_failure": true
	}
}
```
