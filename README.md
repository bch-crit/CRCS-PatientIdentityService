**CRCS Patient Indentity Service**
----
  This describes the REST function for the Patient Indentity service through CRCS using the HL7® FHIR® standard for Patient resoucre type. If you have any problems or requests, please post to our [developer group](mailto:isdcrit-dl@childrens.harvard.edu) .

* **Schema:**
</br>
All data is sent and received as JSON.

<pre class="terminal">
{
  "resourceType": "Conformance",
  "publisher": "BCH Clinical Research Informatic Team",
  "date": "2017-01-04T09:19:12-05:00",
  "kind": "instance",
  "software": {
    "name": "CRCS FHIR Server",
    "version": "1.0.1 "
  },
  "implementation": {
    "description": "CRCS FHIR Server (DSTU2 Resources).It allows a user to search on patients, and retrieve the details of a patient based on their identifier"
  },
  "fhirVersion": "1.0.2",
  "acceptUnknown": "extensions",
  "format": [
    "application/xml+fhir",
    "application/json+fhir"
  ],
  "rest": [
    {
      "mode": "server",
      "resource": [
        {
          "extension": [
            {
              "url": "http://hl7api.sourceforge.net/hapi-fhir/res/extdefs.html#resourceCount",
              "valueDecimal": 131
            }
          ],
          "type": "Patient",
          "profile": {
            "reference": "http://hl7.org/fhir/profiles/Patient"
          },
          "interaction": [
            {
              "code": "read"
            }
          ],
          "searchParam": [
            {
              "name": "identifier",
              "type": "token",
              "documentation": "A patient identifier"
            }
          ]
        }    
  ]
}
</pre>


* **URL**

 _/Patient/_search_

* **Method:**
 
  `POST` 

*  **URL Headers**
  **Required:**
  </br> `userid=[string]`
  </br> `Accept=application/json+fhir`
  
* **Data Params**

  **Required:**
 
   `identifier=[integer]`

* **Success Response:**
  

  * **Code:** 200 <br />
    **Content:** 
    </br>
    See [this](https://www.hl7.org/FHIR/bundle.html#tab-json)
   
* **Error Response:**

There are five possible types of client errors on API calls that
receive request bodies:

1. Failing to send a required query parameter will result in a `400 Bad Request` response.

        HTTP/1.1 400 Bad Request

        no supported search parameters provided

2. Requesting without valid userid will result in a `401 Unauthorized`
   response.

        HTTP/1.1 401 Unauthorized

3. Requesting data from an unknown instance or an instance where the application is not authorized will result in a `403 Forbidden` response.

        HTTP/1.1 403 Forbidden

        Tenant not valid or accessible

4. Requesting a resource which does not exist will resule in a `404 Not Found` response.

        HTTP/1.1 404 Not Found

5. Requested a media type other than JSON will result in a `406 Not Acceptable` response.

        HTTP/1.1 406 Not Acceptable
        Content-Length: 0


  
* **Sample Call:**

  <_Sample call to get the Patient with MRN = "99999999"_> 
  <pre class="terminal">
  $ curl -i -H "Accept: application/json+fhir" -H "userid=ch0007" -X POST -d "identifier":"999999999" "https://crit-test1/FHIR/Patient/_search"

  </pre>
  
  Sample Return
  
   <pre class="terminal">
    {
  "resourceType": "Bundle",
  "meta": {
    "lastUpdated": "2017-01-04T09:24:07.119-05:00"
  },
  "type": "searchset",
  "total": 1,
  "entry": [
    {
      "fullUrl": "http://crit-test1/FHIR/Patient/1234",
      "resource": {
        "resourceType": "Patient",
        "id": "1234",
        "meta": {
          "versionId": "1",
          "lastUpdated": "2016-12-28T21:44:23.546-05:00"
        },
        "text": {
          "status": "generated",
          "div": " "
        },
        "extension": [
          {
            "url": "http://hl7.org/fhir/StructureDefinition/us-core-race",
            "valueCodeableConcept": {
              "coding": [
                {
                  "system": "http://hl7.org/fhir/v3/Race",
                  "code": "2106-3",
                  "display": "White"
                }
              ]
            }
          },
		  {
            "url": "http://hl7.org/fhir/StructureDefinition/us-core-ethnicity",
            "valueCodeableConcept": {
              "coding": [
                {
                  "system": "http://hl7.org/fhir/v3/Ethnicity",
                  "code": "2135-2",
                  "display": "Hispanic or Latino"
                }
              ]
            }
          },
		  
		  
        ],
        "identifier": [
          {
            "use": "usual",
            "type": {
              "text": "Medical Record Number"
            },
            "label": "MRN"
            "value": "999999999"
          }
        ],
			"telecom": [
          {
            "system": "phone",
            "value": "9781508508",
            "use": "home",
          }
        ],
        "address": [
          {
            "use": "home",
            "line": [
              "1295 Boylston Rd"
            ],
            "city": "Boston",
            "state": "MA",
            "postalCode": "02115",
            "country": "US"
          }
        ],
        "active": true,
        "gender": "female",
        "birthDate": "1966-04-04",
        "deceasedBoolean": false
      },
      "search": {
        "mode": "match"
      }
    }
  ]
}
</pre>
 
