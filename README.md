# Bug report example project

This is an example project that uses a custom instrumentation runner.
We would like to be able to run the device tests of this project in the testobject cloud.

- Version of Gradle Plugin: 2.3.2
- Version of Gradle: Gradle 3.3
- Version of Java: 1.8.0_77
- Version of testobject-gradle plugin: 0.0.43
- Version of espresso runner: 0.1-alpha

We tried to execute the tests using the following methods:
- testobject-gradle plugin
- espresso-runner
- uploading the apks manually to the testobject website

### Expected behavior:
- The tests run successfully in the cloud

### Actual behavior:
- When we tried the gradle plugin and the espresso runner, we got 500 error code.
- When we tried to upload the APKs manually to the testobject website, the website detected no tests in the uploaded test APK, therefore we could not run our tests


## Details of the example project

Please check app/build.gradle for the configuration details.

To run the tests locally:
./gradlew app:connectedCheck

To run the tests in cloud:
./gradlew app:testobjectUpload -Pusername=yourTestObjUser -Ppassword=yourTestObjPassword

We would expect this command to complete successfully, instead we get an error:
```* What went wrong:
   Execution failed for task ':app:testobjectUpload'.
   > HTTP 500 Internal Server Error
```

We also tried to run the tests using the espressorunner with the command below,but we got the same result:
```
java -jar espressorunner-0.1.jar \
    --app app/build/outputs/apk/app-debug.apk \
    --test app/build/outputs/apk/app-debug-androidTest.apk \
    --username yourTestObjUser \
    --password yourTestObjPassword \
    --team yourTeam \
    --project yourProject \
    --suite 7 \
    --runAsPackage \
    --url yourUrl
```

When we check the uploaded APKs in the testobject web UI, it says that there are no tests found in the test APK:
- Suite Tests: Your Espresso APK doesn't contain any tests.

We deliberately enabled the runAsPackage parameter in the testobject configuration, because we thought that it should solve the issue as the doc says from https://github.com/testobject/testobject-gradle-plugin:
- runAsPackage true // This is a new feature we recently brought. If you are using custom runners and doing internal filterings we recommend you to use it like this. If not this option can be deleted or set as false

---

Forked from: https://github.com/sebaslogen/CleanGUITestArchitecture