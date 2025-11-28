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



