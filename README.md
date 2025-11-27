# test-actions
Repository to test the flow used in value set, concept map and code system authoring. In the process, user maintain
the value sets, concept maps and code systems using github, e.g., work with branches until ready to merge to main. At 
push events the github action publishes the value sets, concept maps and code systems to the FHIR terminology server. 

Since push requests can only be made by authenticated and authorized github users, it's guaranteed that only authorized 
users can publish the value sets, concept maps and code systems. 

The terminology server api uses an API key from the repository to secure the call from the github action. 

To setup a new repository:
- create a new repository
- manage users and access
- create a new file called .github/workflows/ci.yml
- generate a new API key from the repository Settings/Secrets and Variables/Actions
- register the API key in the terminology server