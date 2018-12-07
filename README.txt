Dependencies: 
•	Jenkins
•	Gradle 5.0
•	Git
•	Java JDK 1.8 

Setup:
	Jenkins:
    -   Ensure that Jenkins is installed on target PC.
    -   The following item paths need to be defined in the global config path:
    -	Java JDK location
    -   Gradle install location
    -	Git location
    -   Modify Jenkins port to 8282 for localhost in global config.
    -   Setup Jenkins SMTP server to allow for email notifications on Jenkins Jobs.
    -   To populate the Case Study jobs for Jenkins, take the ‘Jenkins config’ folder from GitHub.
    -   The following jobs will be added:
            Complete_Build [Manual option to build application]
            Poll_Complete_Build [Automatic polling of SCM to build new application changes]
            Push_To_Webpage [Takes generated build file and runs it on a local server]

	Gradle: 
        Install Gradle version 5 to directory on C Drive(C:\Gradle).
        Ensure Gradle bin path is added to environment variables to allow for non-dependent path use of Gradle in command prompt.
	
	Java JDK:
        Ensure Java JDK 1.8 is installed on target PC.
        Ensure JDK lib path is added to environment variables to allow for non-dependent path use in command prompt.
	
	Git:
        Ensure Git is installed on target PC.
        Ensure Git application is added to environment variables to allow for non-dependent path use in command prompt.

Objective:
    Develop a CI/CD process to build and test the development team’s code. 
    After commit is made to the master branch of the repository, code should be build and tests run. 
    If the build is successful, push the artefact to an online repository with a unique name and deploy to a dev environment.
    If the build is unsuccessful, an email should be sent to the dev team to indicate that it has failed and the reason why.
    Dev team should be able to trigger a build manually to deploy an artefact and upload to whichever environment they wish to deploy it to.
    
My Approach:
    For my approach to this Case Study I have decided to create a CI pipeline using Git, Jenkins and a web application.
    Web App source code shall be managed on Git to allow for continuous development on a single master branch.
    Jenkins will be used to configure jobs to allow for the handling of the CI pipeline by building latest changes on master branch and push the build output to run on a server.
    My setup is all based on a single PC which means both development, Jenkins hosting and Web hosting is done locally.


Source Code Management: 
	- Github:
        Repo location: 
        https://github.com/Sundas94/CaseStudy
        
        Sample project where base code was retrieved from:
        https://github.com/spring-guides/gs-spring-boot
	
	- Building Application: 
        Gradle Environment is used to compile and build output of web application file.
        To run the build, navigate to the ‘complete’ folder in repo checkout on the target PC.
        Enter command 'gradlew build --info' to run the build script.
        Errors will be reported on the command line.
        Build output will be generated in the location: ‘complete/build/lib’
	
	- Committing:
        Developers can commit and push changes to GitHub.
        GitHub is a public source, so no access rights are present. 
	
	-Pushing: 
        No git hook or git push is used due to GitHub repo being hosted on remote server while Jenkins is hosted on local server.
        GitHub hook will be not have access to local Jenkins to perform hook commands so the Jenkins job will Poll SCM server every 30 minutes for new changes.
	
CI Pipeline:
	-Local setup:
        Each developer will have their own local environment where they can modify, build and test source code. 
        Each developer will commit and push to a single master branch for continuous development.
	
	-Jenkins: 
        Jenkins will monitor GitHub repo for pushed commits. 
        The 'Poll_Complete_Build' job will Poll SCM every 30 minutes to check for any new developer changes.
        On detection of a new commit, 'Poll_Complete_Build' will checkout latest repo revision and compile.
        On a successful build, output file will be copied to artefacts folder in Jenkins workspace and job will be marked as successful.
        On success of a 'Poll_Complete_Build' job , Push_To_Webpage job will be called, which will run generated output on dev environment.
        On failure of a build, an email will be sent to technicaltest@deloitte.co.uk.
        and build will be marked as failed.
	
	-Dev Environment: 
        Push_To_Webpage job will be used to update dev environment webpage.
        Current running jar will be closed and updated output will be ran from successful build.
	
Target objectives not met:

    The sample code contains test cases for the sample class included in GreetingsController.
        This job has not been added to Jenkins as new test cases would need to be updated due to changes I have made to source code.
    
    Artefact Storage:
        Artefact Storage such as Nexus was not implemented as generated outputs are stored locally in Jenkins workspace and pushed to dev environment from there.
        With a hosted server, setting up Nexus Storage can be added for storing build outputs.