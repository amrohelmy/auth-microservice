# auth-microservice

## Description
Authentication microservice using Spring Rest API, Spring Security, JPA, MySQL, JWT and RSA Encryption.

Full description of the code can be found [here](https://simplersoftware.io/secure-backend-apis-using-a-custom-authentication-microservice/).

## Technology
Java 13, Spring, Maven

## Pre-requisites 
- JDK-8 or above should be installed and the JAVA_HOME environment variable should be set to the JDK home directory. For example: 
`JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home`

- Maven should be installed and the bin directory is added to the PATH environment variable. More information https://maven.apache.org/install.html

## How to run the service
1. Download or clone the code
`git clone https://github.com/SimplerSoftwareOrg/branch-protection-service.git`

2. Set a system environment variable called GITHUB_TOKEN to the personal access token value of your GitHub account
run `export GITHUB_TOKEN=<personal access token>` or add the variable to the .bash_profile file under your account.
> Note: the access token is used to authenticate the updates on the repo and should have full repo access. 
To create one, go to your account page and from the right top drop down menu go to 'Settings', then go to 'Developer Settings', then click 'Personal access token', now click on 'Generate new token'. Give the token a description such as "branch-protection" and select the repo full access scope then click 'Generate Token'. Now you can copy this value and set the environment variable GITHUB_TOKEN as mentioned above.

3. At this point you can build and run the service however you can't test it yet. To test the code please follow steps 4-8 first before building and running the code.
To build code: in your command line go inside the project's top directory and run:
`mvn clean install`
The previous command will generate a jar file and prompt the location of the jar file, copy the location of the jar file.
To run code: 
`java -jar <JAR file path>`
> Note: This service will start on http port 8080 by default, however if this port is unavailable on your server then please add the following property to the src/main/resources/application.properties file
`server.port=<port number>` note that port number must be the same as used in step 5.

## How to test the service
In order to test the service, you need a Github account, Github Organisation, Organisation webhook and a smee channel. 

4. In order to receive the events generated by the webhook on your local server you need to use smee, install smee by running 
`npm install --global smee-client`

5. Generate a new smee channel by visiting https://smee.io, and copy the generated URL. Start a new smee channel by running
`smee --url <the generated smee url> --path /repo/event --port <<8080 unless you choose another port>>`

6. Create an organisation on your Github account

7. Create a new organisation webhook, go to your account ‘Settings’ and click on ‘Organisation settings’ then go to ‘Webhooks’ and click ‘Add webhook’ and enter/select the following values:
`Payload URL: <the generated smee url from step 5>;`
`Content type: application/json;`
`Which events would you like to trigger this webhook?: Let me select individual events: Repositories;`
Click ‘Add webhook’

8. Now you can start the service as described in step 3

9. Create a new repository in your organisation. You should receive a mention notification confirming that the master branch of the new repository is protected and listing the protections applied.

> You can experiment with different protections by setting the values of the following properties in the src/main/resources/application.properties file
`github.branch.default.protection.statusCheck.strict = true`
`github.branch.default.protection.statusCheck.contexts = default-context`
`github.branch.default.protection.enforceAdmin = false`
`github.branch.default.protection.pullRequestReviews.count = 1`
`github.branch.default.protection.dismissalRestriction.users =<comma separated list of users>`
`github.branch.default.protection.dismissalRestriction.teams =<comma separated list of teams>`
`github.branch.default.protection.Restriction.users =<comma separated list of users>`
`github.branch.default.protection.Restriction.teams =<comma separated list of teams>`
`github.branch.default.protection.Restriction.apps =<comma separated list of apps>`


> Note: if you are not receiving the notification emails in your mailbox then you may need to enable notifying your own updates by going to Settings->Notifications-> tick the checkbox called Include your own updates.
