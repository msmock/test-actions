# test-actions
Repository to test the flow used in value set, concept map and code system authoring. In the process, user maintain
the value sets, concept maps and code systems using github, e.g., work with branches until ready to merge to main. At 
push events the github action publishes the value sets, concept maps and code systems to the FHIR terminology server. 

Since push requests can only be made by authenticated and authorized github users, it's guaranteed that only authorized 
users can publish the value sets, concept maps and code systems. 

The terminology server api uses an API key from the repository to secure the call from the github action. 

# Setup of Repositories

To set up a new repository as source for the terminology server:
- create a new repository
- create a root folder for data product 
- add folder named ValueSet, ConceptMap and CodeSystem to root folder
- manage users and access in github
- copy the action to the repository as .github/workflows/ci.yml
- generate a new API key from the repository in `Settings/Secrets and Variables/Actions`
- register the API key in the terminology server

Note: The github action uses the folder names ValueSet, ConceptMap and CodeSystem to identify the endpoint to be 
called on the terminology server. Files in folder with other names are ignored. 

# Test Example with Curl
To inspect data send from the action, use the following curl command:
`curl -X PUT -H 'Content-type: application/json' --data '@CHAllergyIntoleranceCondition.json' https://node-express-tracer.onrender.com/`

# Test with HAPI

## Test ValueSet $validate-code with HAPI

Validate code from HAPI:
- use POST /CodeSystem to store the code system referenced from value set
- use POST /ValueSet to store the value set
- use GET /ValueSet/{id}/$validate-code in Swagger
- set id to the numerical id as returned when saving the value set 
- set code to the code to be validated (e.g., code2)
- set system to the system as specified in the value set (e.g., http://hl7.org/fhir/test/CodeSystem/simple)

Example code: 
```  
      {
        "system": "http://hl7.org/fhir/test/CodeSystem/simple",
        "version": "0.1.0",
        "code": "code2",
        "display": "Display 2"
      }
```

Example output: 
```
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "result",
      "valueBoolean": true
    },
    {
      "name": "message",
      "valueString": "Code validation occurred using a ValueSet expansion that was pre-calculated at 2025-12-01T08:54:28.998+01:00"
    },
    {
      "name": "display",
      "valueString": "Display 2"
    }
  ]
}
```

## Test ValueSet $expand with HAPI

Expand value set from HAPI:
- use POST /CodeSystem to store the code system referenced from value set
- use POST /ValueSet/$expand to send the value set to be expanded

see: 
- input value set: test-expand.json
- expanded from HAPI: test-expand-actual.json
- expected from IG Test: test-expand-expected.json



