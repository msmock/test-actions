# test-actions
Repository to test the flow used in value set, concept map and code system authoring. In the process, user maintain
the value sets, concept maps and code systems using github, e.g., work with branches until ready to merge to main. At 
push events the github action publishes the value sets, concept maps and code systems to the FHIR terminology server. 

Since push requests can only be made by authenticated and authorized github users, it's guaranteed that only authorized 
users can publish the value sets, concept maps and code systems. 

The terminology server api uses an API key from the repository to secure the call from the github action. 

To setup a new repository as source for the terminology server:
- create a new repository
- manage users and access in github
- copy the action to the repository as .github/workflows/ci.yml
- generate a new API key from the repository in `Settings/Secrets and Variables/Actions`
- register the API key in the terminology server


# Open Issues

## large value sets

### Problem 
Value sets may be too large for the curl command to handle. For example

```curl -X POST -H "Content-Type: application/json" --data @CHAllergyIntoleranceCondition.json https://node-express-tracer.onrender.com/```

fails, since the json file has 393 kB which is too large.

### Solution
Send in chunks or zip the file. 

