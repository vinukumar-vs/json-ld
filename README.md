# json-ld
As part of Sunbird dail-service, we are using JSON-LD to define DIALCode context information. 

> Discussion: [Question on DIAL URL · Discussion #2 · sunbird-specs/DIAL-specs]()


JSON-LD Samples

## [Context whitout @graph](https://json-ld.org/playground/#startTab=tab-flattened&copyContext=true&json-ld=%7B%22%40context%22%3A%7B%22schema%22%3A%22http%3A%2F%2Fschema.org%2F%22%2C%22framework%22%3A%7B%22%40id%22%3A%22schema%3Aname%23framework%22%2C%22%40type%22%3A%22schema%3Aname%22%7D%2C%22board%22%3A%7B%22%40id%22%3A%22schema%3Aname%23board%22%2C%22%40type%22%3A%22schema%3Aname%22%7D%2C%22medium%22%3A%7B%22%40id%22%3A%22schema%3Aname%23medium%22%2C%22%40type%22%3A%22schema%3Aname%22%7D%2C%22gradeLevel%22%3A%7B%22%40id%22%3A%22schema%3Aname%23grade_level%22%2C%22%40type%22%3A%22%40id%22%2C%22%40container%22%3A%22%40list%22%7D%2C%22subject%22%3A%7B%22%40id%22%3A%22schema%3Aname%23subject%22%2C%22%40type%22%3A%22%40id%22%2C%22%40container%22%3A%22%40list%22%7D%2C%22textbook%22%3A%22schema%3ABook%22%2C%22textBookUnit%22%3A%22schema%3AChapter%22%2C%22certificate-test%22%3A%22schema%3ACourse%22%7D%2C%22%40id%22%3A%22http%3A%2F%2Fexample.org%2Fdialcode%2F1234%22%2C%22%40type%22%3A%22schema%3ACode%22%2C%22board%22%3A%22AP%22%2C%22framework%22%3A%22NCF%22%2C%22medium%22%3A%22English%22%2C%22subject%22%3A%5B%22Maths%22%2C%22Social%22%5D%2C%22gradeLevel%22%3A%22Grande%201%22%2C%22textbook%22%3A%7B%22%40id%22%3A%22http%3A%2F%2Fexample.org%2Fcollection%2F4728-2345-2343-3455%22%2C%22identifier%22%3A%224728-2345-2343-3455%22%2C%22name%22%3A%22Sample%20textbook%22%7D%2C%22textBookUnit%22%3A%7B%22name%22%3A%22Unit%20name%22%7D%2C%22certificate%22%3A%7B%22%40id%22%3A%22http%3A%2F%2Fexample.org%2Fcert%2F1%22%2C%22name%22%3A%22Course%20completion%22%7D%2C%22course%22%3A%7B%22name%22%3A%22Course%20name%22%7D%7D&frame=%7B%7D) 

#### End-point: 
```sh
{host}/v1/dialcode/update/V1T2P8
```
> note:   
> V1T2P8 -> DIAL code used to link the content   

#### @context: 

Context infomation will be stored in the JSON file. Which can be configured/defined by the user specific to each context type. User can send the request of specific context, which will be validated against this JSON-LD context file. 
> note:  
> Here we are not using `@graph` object declaration in the context data/file.  
> Hence all the context information will be stored against single graph node of the dialcode.(if we use graph DB to store)

Examples:  
* For `Textbook`: http://example.org/dialcode-textbook.json  
* For `certificate`: http://example.org/dialcode-cert.json  

### `Textbook`: http://example.org/dialcode-textbook.json
Context file data to validation when textbook is linked to dialcode
```json
{
    "@context": {
        "schema": "http://schema.org/",
        "framework": {
            "@id": "schema:name#framework",
            "@type": "schema:name"
        },
        "board": {
            "@id": "schema:name#board",
            "@type": "schema:name"
        },
        "medium": {
            "@id": "schema:name#medium",
            "@type": "schema:name"
        },
        "gradeLevel": {
            "@id": "schema:name#grade_level",
            "@type": "@id",
            "@container": "@list"
        },
        "subject": {
            "@id": "schema:name#subject",
            "@type": "@id",
            "@container": "@list"
        },
        "textbook": "schema:Book",
        "textBookUnit": "schema:Chapter",
    }
}
```

#### Request:
```json
{
  "request": {
    "dialcode": {
      "contextInfo": {  // OPTIONAL
        "@context": "http://example.org/dialcode-textbook.json", // URL path of context file
        "@id": "http://example.org/dialcode/V1T2P8",
        "board": "AP",
        "framework": "NCF",
        "medium": "English",
        "subject": ["Maths", "Social"],
        "gradeLevel": "Grande 1",
        "textbook": {
            "@id": "http://example.org/collection/4728-2345-2343-3455",
            "identifier": "4728-2345-2343-3455",
            "name": "Sample textbook"
        },
        "textBookUnit": {
            "name": "Unit name"
        },
      }
    }
  }
}
```
#### Response: 
```json
"id": "api.dialcode.update",
  "ver": "1.0",
  "ts": "2020-12-18T07:10:28.747Z",
  "params": {
    "resmsgid": "1bd1c5b0-4100-11eb-9b0c-abcfbdf41bc3",
    "msgid": "19fe8c50-4100-11eb-9b0c-abcfbdf41bc3",
    "status": "successful",
    "err": null,
    "errmsg": null
  },
  "responseCode": "OK",
  "result": {
    "identifier": "V1T2P8",
    "@id": "http://example.org/dialcode/V1T2P8",
  }

```


### `certificate`: http://example.org/dialcode-cert.json  
Context file data to validation when textbook is linked to dialcode
```json
{
    "@context": {
        "schema": "http://schema.org/",
        "certificate": "schema:CreativeWork",
        "course": "schema:Course"
    }
}
```

#### Request:
```json
{
  "request": {
    "dialcode": {
      "contextInfo": {  // OPTIONAL
        "@context": "http://example.org/dialcode-cert.json", // URL path of context file
        "@id": "http://example.org/dialcode/V2T2P4",
        
        "certificate": {
            "@id": "http://example.org/cert/123",
            "name": "Certificate of Completion"
        },
        "course": {
            "@id": "http://example.org/course/4728-2345-2343-3455",
            "identifier": "4728-2345-2343-3455",
            "name": "Course Name"
        }
      }
    }
  }
}
```
#### Response: 
```json
"id": "api.dialcode.update",
  "ver": "1.0",
  "ts": "2020-12-18T07:10:28.747Z",
  "params": {
    "resmsgid": "1bd1c5b0-4100-11eb-9b0c-abcfbdf41bc3",
    "msgid": "19fe8c50-4100-11eb-9b0c-abcfbdf41bc3",
    "status": "successful",
    "err": null,
    "errmsg": null
  },
  "responseCode": "OK",
  "result": {
    "identifier": "V2T2P4",
    "@id": "http://example.org/dialcode/V2T2P4",
  }

```

### Pros:  
* API request format is simple
* We can validate any invalid property has sent as part of request against the JSON-LD schema.

### Cons:
* All the properties are storing against the single node object(dialcode).

## [Context with @graph](https://json-ld.org/playground/#startTab=tab-flattened&copyContext=true&json-ld=%7B%22%40context%22%3A%7B%22schema%22%3A%22https%3A%2F%2Fschema.org%2F%22%2C%22framework%22%3A%7B%22%40id%22%3A%22dial%3Aframework%22%2C%22%40type%22%3A%22schema%3Aname%22%2C%22%40container%22%3A%22%40graph%22%7D%2C%22board%22%3A%7B%22%40id%22%3A%22dial%3Aboard%22%2C%22%40type%22%3A%22schema%3Aname%22%2C%22%40container%22%3A%22%40graph%22%7D%2C%22medium%22%3A%7B%22%40id%22%3A%22dial%3Amedium%22%2C%22%40type%22%3A%22schema%3Aname%22%2C%22%40container%22%3A%22%40graph%22%7D%2C%22gradeLevel%22%3A%7B%22%40id%22%3A%22dial%3AgradeLevel%22%2C%22%40type%22%3A%22%40id%22%2C%22%40container%22%3A%22%40graph%22%7D%2C%22subject%22%3A%7B%22%40id%22%3A%22schema%3Aname%23subject%22%2C%22%40type%22%3A%22%40id%22%2C%22%40container%22%3A%22%40graph%22%7D%2C%22textbook%22%3A%7B%22%40id%22%3A%22schema%3ABook%22%2C%22%40container%22%3A%22%40graph%22%7D%2C%22textBookUnit%22%3A%7B%22%40id%22%3A%22schema%3AChapter%22%2C%22%40container%22%3A%22%40graph%22%7D%2C%22certificate%22%3A%7B%22%40id%22%3A%22schema%3ACourse%22%2C%22%40type%22%3A%22%40id%22%2C%22%40container%22%3A%22%40graph%22%7D%7D%2C%22%40id%22%3A%22http%3A%2F%2Fexample.org%2Fdialcode%2F1234%22%2C%22%40type%22%3A%22schema%3ACode%22%2C%22%40graph%22%3A%5B%7B%22%40id%22%3A%221%22%2C%22%40type%22%3A%22certificate%22%2C%22name%22%3A%22Course%20completion%22%7D%2C%7B%22%40id%22%3A%22http%3A%2F%2Fexample.org%2Fcollection%2F4728-2345-2343-3455%22%2C%22%40type%22%3A%22textbook-test%22%2C%22identifier%22%3A%224728-2345-2343-3455%22%2C%22name%22%3A%22Sample%20textbook%22%2C%22realtion%22%3A%22child%2Fparent%22%7D%2C%7B%22%40id%22%3A%22framework%2F1%22%2C%22%40type%22%3A%22framework%22%2C%22name%22%3A%22NCF%22%2C%22associatedTo%22%3A%22http%3A%2F%2Fexample.org%2Fcollection%2F4728-2345-2343-3455%22%7D%5D%7D&frame=%7B%7D)

Here we are using `@graph` declartion in the context for JSON-LD declaration. Hence user has to send the `request` also in the `graph` format so that we can validate the request against the `@context` delcaration. 

We are taking he same examples of `Textbook` & `Certificate` explanied above.

### `Textbook`: http://example.org/dialcode-textbook.json
Context file data to validation when textbook is linked to dialcode
```json
{
  "@context": {
    "schema": "http://schema.org/",
    "dial": "https://example.org/dial"
  },
  "@graph": [
    {
      "@id": "framework",
      "@type": "schema:name",
      "rdfs:comment": "Class to represent a framework.",
      "rdfs:label": "framework",
      "rdfs:subClassOf": {
        "@id": "schema:CreativeWork"
      }
    },
    {
      "@id": "DIALCode",
      "@type": "schema:Code",
      "rdfs:comment": "Class to represent a DIAL code.",
      "rdfs:label": "DIALCode",
      "rdfs:subClassOf": {
        "@id": "schema:Code"
      }
    }
  ]
}
```

#### Request:
```json
{
  "request": {
    "dialcode": {
      "contextInfo": {
        "@context": {
          "@vocab": "http://example.org/",
          "linkedTo": {
            "@type": "@id"
          }
        },
        "@graph": [
          {
            "@id": "http://example.org/framework/1",
            "@type": "framework",
            "name": "NCF",
            "linkedTo": "http://example.org/textbook/1"
          },
          {
            "@id": "http://example.org/DIALCode/V1T2P8",
            "@type": "DIALCode",
            "name": "V1T2P8",
            "linkedTo": "http://example.org/framework/1"
          },
          {
            "@id": "http://example.org/textbook",
            "@type": "textbook",
            "identifier": "4728-2345-2343-3455",
            "name": "Textbook Name"
          }
        ]
      }
    }
  }
}
```
#### Response: 
```json
"id": "api.dialcode.update",
  "ver": "1.0",
  "ts": "2020-12-18T07:10:28.747Z",
  "params": {
    "resmsgid": "1bd1c5b0-4100-11eb-9b0c-abcfbdf41bc3",
    "msgid": "19fe8c50-4100-11eb-9b0c-abcfbdf41bc3",
    "status": "successful",
    "err": null,
    "errmsg": null
  },
  "responseCode": "OK",
  "result": {
    "identifier": "V1T2P8",
    "@id": "http://example.org/dialcode/V1T2P8",
  }

```


### `certificate`: http://example.org/dialcode-cert.json  
Context file data to validation when certificate is linked to dialcode
```json
{
    "@context": {
        "schema": "http://schema.org/",
      	"dial": "https://example.org/dial/"
    },
    "@graph": [
      {
        "@id": "dial:DIALCode",
        "@type": "dial:DIALCode",
            "rdfs:comment":  "Class to represent a DIAL code.",
            "rdfs:label": "DialCode",
            "rdfs:subClassOf": {
                "@id": "schema:Code"
            }
        },
        {
          "@id": "certificate",
            "@type": "dial:certificate",
            "rdfs:comment":  "Class to represent a Certificate.",
            "rdfs:label": "framework",
            "rdfs:subClassOf": {
                "@id": "schema:CreativeWork"
            }
        },
        {
          "@id": "course",
            "@type": "dial:course",
            "rdfs:comment":  "Class to represent a Course.",
            "rdfs:label": "Course",
            "rdfs:subClassOf": {
                "@id": "schema:CreativeWork"
            }
        }
    ]
}
```

#### Request:
```json
{
  "request": {
    "dialcode": {
      "contextInfo": {
        "@context": {
          "@vocab": "http://example.org/dial",
          "linkedTo": {
            "@type": "@id"
          },
          "dial": "https://example.org/dial/"
        },
        "@graph": [
          {
            "@id": "http://example.org/DIALCode/V2T2P4",
            "@type": "dial:DIALCode",
            "name": "V2T2P4",
            "linkedTo": "http://example.org/cert/123"
          },
          {
            "@id": "http://example.org/cert/123",
            "@type": "dial:certificate",
            "name": "Certificate of Completion",
            "linkedTo": "http://example.org/course/4728-2345-2343-3455"
          },
          {
            "@id": "http://example.org/course4728-2345-2343-3455",
            "@type": "dial:course",
            "name": "Course Name",
            "linkedTo": "http://example.org/course/4728-2345-2343-3455"
          }
        ]
      }
    }
  }
}
```
#### Response: 
```json
"id": "api.dialcode.update",
  "ver": "1.0",
  "ts": "2020-12-18T07:10:28.747Z",
  "params": {
    "resmsgid": "1bd1c5b0-4100-11eb-9b0c-abcfbdf41bc3",
    "msgid": "19fe8c50-4100-11eb-9b0c-abcfbdf41bc3",
    "status": "successful",
    "err": null,
    "errmsg": null
  },
  "responseCode": "OK",
  "result": {
    "identifier": "V2T2P4",
    "@id": "http://example.org/dialcode/V2T2P4",
  }

```

### Pros:
* We can define our own named node-objects & its relations(If we are storing in graph DB)

### Cons:
* User can send the new graph node object which is not present in the JSON-LD context. It is still allowing to store. We have to explicitly validate this.
* The API request also need to change to send in the format of graph nodes

### [Context with @graph & Validation]()
* Validation of API request with JSON-LD context(Using JSON-LD Frame) 


