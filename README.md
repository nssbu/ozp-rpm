# ozp-rpm

##TODO
- Add a stage to the Jenkinsfile that edits the env.BUILD_NUMBER before the Maven build runs, so that the OZP RPM has a higher version number than the RPM that's currently deployed on the development stack. Perhaps the stage could get the current version number of the deployed RPM by checking the file name of the most recent OZP RPM artifact? That'd be a more stable solution than simply incrementing env.BUILD_NUMBER, since that assumes that the current value of that environment variable is correct.
