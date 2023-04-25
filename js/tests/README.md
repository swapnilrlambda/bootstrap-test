<img height="100" alt="hyperexecute_logo" src="https://user-images.githubusercontent.com/1688653/159473714-384e60ba-d830-435e-a33f-730df3c3ebc6.png">

HyperExecute is a smart test orchestration platform to run end-to-end Selenium tests at the fastest speed possible. HyperExecute lets you achieve an accelerated time to market by providing a test infrastructure that offers optimal speed, test orchestration, and detailed execution logs.

The overall experience helps teams test code and fix issues at a much faster pace. HyperExecute is configured using a YAML file. Instead of moving the Hub close to you, HyperExecute brings the test scripts close to the Hub!

* <b>HyperExecute HomePage</b>: https://www.lambdatest.com/hyperexecute
* <b>Lambdatest HomePage</b>: https://www.lambdatest.com
* <b>LambdaTest Support</b>: [support@lambdatest.com](mailto:support@lambdatest.com)

To know more about how HyperExecute does intelligent Test Orchestration, do check out [HyperExecute Getting Started Guide](https://www.lambdatest.com/support/docs/getting-started-with-hyperexecute/)

<!---If logged in, it will be redirected to Gitpod editor in new tab where current tab will show hyperexecute dashboard.

If not logged in, it will be redirected to Login/Signup page and simultaneously redirected to Gitpod editor in a new tab where current tab will show hyperexecute dashboard.

If not signed up, you need to sign up and simultaneously redirected to Gitpod in a new tab where current tab will show hyperexecute dashboard.--->

# HyperExecute v/s Traditional Test Grids

Traditional automation testing solutions are slow since the triggered test scenarios are first sent to the Hub, which in turn are scheduled to run on the best-suited Node. This results in unnecessary lag as a number of components are involved in the entire process. Over & above, multiple network hops with the network separated components results in increased test flakiness, a factor that can be a huge hindrance to time-to-market.

HyperExecute merges all the components into a single execution environment, thereby ensuring all the components talk to each other just like they are in a local network. This eventually helps in achieving native-like performance (on the cloud)! Over & above, you also have the flexibility of enabling all the features that are available in a cloud grid.

![image](https://user-images.githubusercontent.com/128702372/234190064-5aeb99d7-2d6d-4ab1-bf39-456966e20ed4.png)

On the other hand, HyperExecute accelerates test execution at a massive scale with proprietary features mentioned below:

- Smart Auto Test Splitting for parallel test execution through custom filters
- Matrix-based build multiplexing allows users to run the single command over multiple scenarios/conditions/environments by creating a matrix of all the combinations. This is best-suited for regression testing, better coverage and achieving a more stable codebase.
- Auto purging of environments after build completion
- Smart Caching for speeding up the regression tests

Security is at the core of all the tests run using HyperExecute. The test execution logs are encrypted and backed by the Azure cloud storage. This is applicable irrespective of whether the data is in motion or at rest. There is a continuous auditing to secure your test execution environment(s).

Hyperscale of HyperExecute allows users to have fine grain control over test execution concurrency using YAML files.

Here is the diagrammatic representation of how tests are run on the HyperExecute infrastructure:

![image](https://user-images.githubusercontent.com/128702372/234190239-e9a490ae-10ba-4170-8143-ea05bdc44e7c.png)

As seen above, tests are securely run on dedicated machines on the Azure cloud. The cloud (for execution) is chosen based on the location from where concierge (i.e. HyperExecute CLI) is triggered.

# Detailed Description of Hyperexecute Yaml

1. globalTimeout

Total time in minutes after which your test execution should stop. You can allocate the globalTimeout value from 1 minute to 150 minutes, and it is set to 90 minutes by default.

```yaml
    globalTimeout: 90   
```
2. testSuiteTimeout

Timeout in minutes after which your test suite will end. You can allocate the testSuiteTimeout value from 1 minute to 150 minutes, and it is set to 90 minutes by default.

```yaml
    testSuiteTimeout: 90  
```

3. testSuiteStep

Timeout in minutes after which your test suite will end. You can allocate the testSuiteStep value from 1 minute to 150 minutes, and it is set to 90 minutes by default.

```yaml
    testSuiteStep: 90  
```

4. runson

OS on which you will run your Test. You can run your tests on Linux(linux), MacOS(macos) or Windows(win).

```yaml
    runson: win   
```

5. autosplit

If your Auto Split test has to be enabled, set this boolean value to true. For more information on the Auto split feature, go to [this page](https://www.lambdatest.com/support/docs/he-smart-auto/).

```yaml
    autosplit: true   
```

6. retryOnFailure

If your testSuite fails and you want to retry it, set this boolean value to true. The maxRetries key determines how many times your tests will get retried.

```yaml
    retryOnFailure: true    
```
7. maxRetries

The maximum number of times your tests can be retried. You can allocate a numerical value between 1 and 5 for this field.

```yaml
    maxRetries: 2   
```

8. testDiscovery

A command used to discover (or locate) relevant tests via class names, filters, file names, and more.

```yaml
testDiscovery:
  type: raw
  mode: dynamic
  command: |
            printf 'chromeWin10\nchromeWin10Latest\nfirefoxWin10\nfirefoxWin10Latest'
```

9. testRunnerCommand

This is the command that your testing framework uses to trigger the execution of the test.

```yaml
testRunnerCommand: node_modules\.bin\karma start js\tests\karma.conf.js --browsers=$test
```

10. vault

A vault is a secure place to safe-keep data and sensitive company information while you're running tests.

```yaml
  env:
    PAT: ${{ .secrets.Token }}  //PAT: Personal Access Token
```

11. concurrency

Indicates the number of concurrent sessions on HyperExecute. For more information on concurrency, go to [this page](https://www.lambdatest.com/support/docs/he-smart-auto/).

```yaml
  concurrency: 1   
```

12. preDirectives or pre

All actions you need to perform before test execution, such as, installing dependencies.

- If both pre or preDirectives flags are used at the same time, then the precedence is given to the preDirectives flag.
- maxRetries: You can retry the commands that failed in the pre-stage by using this flag. The numerical value assigned to this field determines the amount of times you can retry the failed commands.

```yaml
  preDirectives:
    commands: 
      - mkdir -p m2_cache_dir
      - mvn -Dmaven.repo.local=$CACHE_DIR -Dmaven.test.skip=true clean install  
    maxRetries: 3
```

13. postDirectives or post

All the actions you need to perform after test execution.

- If both post or postDirectives flags are used at the same time, then the precedence is given to the postDirectives flag.

```yaml
  postDirectives:
    - cat yaml/win/*.*hyperexecute_autosplits.yaml 
```

14. cachekey

A cache key is the unique identifier for every object in the cache of the testsuite.

```yaml
  {{ checksum "package-lock.json" }}
```

15. cacheDirectories

Cached directories are the dependency directories of your test suite such as node modules. HyperExecute caches them to help fasten your test execution time the next time you run your job. However, if you modify the dependency directories, they will be downloaded, resolved and cached again.

```yaml
  cacheDirectories:
    - CacheDir
```

16. uploadArtifacts

The uploadArtifacts key contains the path and the name of the file which you want to download.

```yaml
uploadArtefacts:
 - name: Reports
   path:
    - coverage/**/**
```

17. version

The version of HyperExecute YAML being used to run the tests.

```yaml
  version: 0.1
```

18. tunnelOpts

You can opt to use a tunnel for the entire testing process. If you want to use a tunnel selectively (either in pre steps or post steps), you can change the global field to preOnly or postOnly and set it to true.

```yaml
  tunnelOpts:
    global: true
```

19. runtime

|This provides the runtime for specific languages in specific versions. You can visit this page to go through all of the languages and frameworks that HyperExecute supports.

```yaml
  runtime:
    language: dotnet
    version: "6.0.303"
```

20. jobLabel

The jobLabel YAML key is used to add tags or labels to jobs. This allows you to search your jobs using the labels or tags assigned to them.

- Prioritize Your Job Pipeline: To prioritize your jobs, you need to add the required priority to the jobLabel key in the YAML file e.g jobLabel: [ 'high', 'Low','medium']. With 'high' priority jobs triggered first, followed by medium priority jobs and finally low priority jobs. The values are case insensitive and the default priority is 'medium'.
- You can also use it along with your existing job labels like this:

```yaml
    jobLabel: [ '${DATE} - ${DAY}','Foo','Bar', 'low']
```

21. failFast

This feature allows you to either run your jobs faster or fail fast to provide faster feedback and save your test time.

- maxNumberOfTests: You can customize your Fail Fast feature according to your needs by providing the number of consecutive tests that can fail in this section.

```yaml
      failFast:
    maxNumberOfTests: 2
```

22. report

This field allows you to generate the test reports in the location of your choice.

email: You can also get your job reports as an email by conifguring the email key under partialReports. To learn more about this feature, go through [this page](https://www.lambdatest.com/support/docs/he-email-reports).

```yaml
  report: true
  partialReports:
    location: target/cucumber-reports/
    frameworkName: cucumber
    type: json
    email:
      to:
        - johndoe@example.com 
 ```

# How to run Selenium automation tests on HyperExecute

* [Pre-requisites](#pre-requisites)
   - [Download HyperExecute CLI](#download-hyperexecute-cli)
   - [Configure Environment Variables](#configure-environment-variables)

* [Auto-Split Execution with NightWatch](#auto-split-execution-with-nightwatch)
   - [Core](#core)
   - [Pre Steps and Dependency Caching](#pre-steps-and-dependency-caching)
   - [Artifacts Management](#artifacts-management)
   - [Test Execution](#test-execution)

* [Secrets Management](#secrets-management)
* [Navigation in Automation Dashboard](#navigation-in-automation-dashboard)
* [GitHub Actions Instructions](#github-actions-instructions)

# Pre-requisites

Before using HyperExecute, you have to download HyperExecute CLI corresponding to the host OS. Along with it, you also need to export the environment variables *LT_USERNAME* and *LT_ACCESS_KEY* that are available in the [LambdaTest Profile](https://accounts.lambdatest.com/detail/profile) page.

## Download HyperExecute CLI

HyperExecute CLI is the CLI for interacting and running the tests on the HyperExecute Grid. The CLI provides a host of other useful features that accelerate test execution. In order to trigger tests using the CLI, you need to download the HyperExecute CLI binary corresponding to the platform (or OS) from where the tests are triggered:

Also, it is recommended to download the binary in the project's parent directory. Shown below is the location from where you can download the HyperExecute CLI binary:

* Mac: https://downloads.lambdatest.com/hyperexecute/darwin/hyperexecute
* Linux: https://downloads.lambdatest.com/hyperexecute/linux/hyperexecute
* Windows: https://downloads.lambdatest.com/hyperexecute/windows/hyperexecute.exe

## Configure Environment Variables

Before the tests are run, please set the environment variables LT_USERNAME & LT_ACCESS_KEY from the terminal. The account details are available on your [LambdaTest Profile](https://accounts.lambdatest.com/detail/profile) page.

For macOS:

```bash
export LT_USERNAME=LT_USERNAME
export LT_ACCESS_KEY=LT_ACCESS_KEY
```

For Linux:

```bash
export LT_USERNAME=LT_USERNAME
export LT_ACCESS_KEY=LT_ACCESS_KEY
```

For Windows:

```bash
set LT_USERNAME=LT_USERNAME
set LT_ACCESS_KEY=LT_ACCESS_KEY
```

## Auto-Split Execution with HyperExecute

Auto-split execution mechanism lets you run tests at predefined concurrency and distribute the tests over the available infrastructure. Concurrency can be achieved at different levels - file, module, test suite, test, scenario, etc.

For more information about auto-split execution, check out the [Auto-Split Getting Started Guide](https://www.lambdatest.com/support/docs/getting-started-with-hyperexecute/#smart-auto-test-splitting)

### Core

Auto-split YAML file (*hyperexecuteStatic.yaml*) in the repo contains the following configuration:

```yaml
globalTimeout: 90
testSuiteTimeout: 90
testSuiteStep: 90
```

Global timeout, testSuite timeout, and testSuite timeout are set to 90 minutes.
 
The *runson* key determines the platform (or operating system) on which the tests are executed. Here we have set the target OS as Windows.

```yaml
runson: win
```

Auto-split is set to true in the YAML file.

```yaml
 autosplit: true
```

*retryOnFailure* is set to true, instructing HyperExecute to retry failed command(s). The retry operation is carried out till the number of retries mentioned in *maxRetries* are exhausted or the command execution results in a *Pass*. In addition, the concurrency (i.e. number of parallel sessions) is set to 2.

```yaml
retryOnFailure: true
runson: win
maxRetries: 2
```

## Pre Steps and Dependency Caching

To leverage the advantage offered by *Dependency Caching* in HyperExecute, the integrity of *package-lock.json* is checked using the checksum functionality.

```yaml
cacheKey: '{{ checksum "package-lock.json" }}'
```

The caching advantage offered by *NPM* can be leveraged in HyperExecute, whereby the downloaded packages can be stored (or cached) in a secure server for future executions. The packages available in the cache will only be used if the checksum stage results in a Pass.

```yaml
cacheDirectories:
  - node_modules
```

The *testDiscovery* directive contains the command that gives details of the mode of execution, along with detailing the command that is used for test execution. Here, we are fetching the list of Feature file scenario that would be further executed using the *value* passed in the *testRunnerCommand*

```yaml
testDiscovery:
  type: raw
  mode: dynamic
  command: |
            printf 'chromeWin10\nchromeWin10Latest\nfirefoxWin10\nfirefoxWin10Latest'
```

Running the above command on the terminal will give a list of browser that are located in the Project folder:

Test Discovery Output:
- chromeWin10
- chromeWin10Latest
- firefoxWin10
- firefoxWin10Latest

The *testRunnerCommand* contains the command that is used for triggering the test. The output fetched from the *testDiscoverer* command acts as an input to the *testRunner* command.

```yaml
testRunnerCommand: node_modules\.bin\karma start js\tests\karma.conf.js --browsers=$test
```


### Artifacts Management

The *mergeArtifacts* directive (which is by default *false*) is set to *true* for merging the artifacts and combing artifacts generated under each task.

The *uploadArtefacts* directive informs HyperExecute to upload artifacts [files, reports, etc.] generated after task completion.  In the example, *path* consists of a regex for parsing the directory (i.e. *reports* that contains the test reports).

```yaml
mergeArtifacts: true

uploadArtefacts:
 - name: Reports
   path:
    - coverage/**/**

```

HyperExecute also facilitates the provision to download the artifacts on your local machine. To download the artifacts, click on *Artifacts* button corresponding to the associated TestID.

### Test Execution

The CLI option *--config* is used for providing the custom HyperExecute YAML file (i.e. *HyperExecute-Yaml/.hyperexecuteStatic.yaml*). Run the following command on the terminal to trigger the tests in Python files on the HyperExecute grid. The *--download-artifacts* option is used to inform HyperExecute to download the artifacts for the job.

```bash
hyperexecute.exe --user <username> --key <accessToken> --config .hyperexecute_autosplit.yaml --download-artifacts
```
Visit [HyperExecute Profile Page](https://accounts.lambdatest.com/detail/profile) to get the username and accessToken

Visit [HyperExecute Automation Dashboard](https://automation.lambdatest.com/hyperexecute) to check the status of execution

## Run Hyperexecute tests on Mac and Linux platforms

The CLI option *--config* is used for providing the custom HyperExecute YAML file (i.e. *yaml/.hyperexecute_simple_win.yaml* for Windows and *yaml/.hyperexecute_simple_linux.yaml* for Linux).

Run the following command on the terminal to trigger tests on Mac platform:

```bash
./hyperexecute --user <username> --key <accessToken> --config .hyperexecute_autosplit.yaml --download-artifacts
```

Run the following command on the terminal to trigger tests on Linux platform:

```bash
./hyperexecute --user <username> --key <accessToken> --config .hyperexecute_autosplit.yaml --download-artifacts
```

## Secrets Management

In case you want to use any secret keys in the YAML file, the same can be set by clicking on the *Secrets* button the dashboard.


All you need to do is create an environment variable that uses the secret key:

```yaml
env:
  AccessKey: ${{.secrets.AccessKey}}
```

## Navigation in Automation Dashboard

HyperExecute lets you navigate from/to *Test Logs* in Automation Dashboard from/to *HyperExecute Logs*. You also get relevant get relevant Selenium test details like video, network log, commands, Exceptions & more in the Dashboard. Effortlessly navigate from the automation dashboard to HyperExecute logs (and vice-versa) to get more details of the test execution.

# GitHub Actions Instructions

### 1. Log into your GitHub account

- Navigate to the main page of the repository.
- Under your repository name, click Actions.

### 2. Create a New Workflow

In the left sidebar, click the New workflow button.

![image](https://user-images.githubusercontent.com/128702372/234122303-3519a62a-e661-4af3-a7d6-cfb60bec82c8.png)

### 3. Create the GitHub Actions work-flow YAML file

To create the GitHub Actions pipeline YAML file, follow the sample command below:

```yaml
name: HyperExecute
on:
  workflow_dispatch:
    inputs:
      username:  # This will ask for the LT username which you can access from https://accounts.lambdatest.com/detail/profile
        required: true
        description: LT Username 
      accessKey: # This will ask for the LT accessToken which you can access from https://accounts.lambdatest.com/detail/profile
        description: LT Access Key
        required: true
      sampleRepoLink: # This will show the default bootstrap repo to be used for testing, you may change it with other bootstrap repos
        description: Link to the HyperExecute repo for bootstrap
        default: 
        required: true
jobs:
  bootstrap-hye-lambda:
    runs-on: windows-latest  # os on which test will be performed
    timeout-minutes: 90  # test time out minutes
    strategy:
      fail-fast: false  
    steps:
      - name: Checkout sources   
        uses: actions/checkout@v2
        with:
          persist-credentials: false
      - name: Starting CLI testing  # this starts the hyperexecute process
        shell: bash
        run: |
          # git clone ${{ github.event.inputs.sampleRepoLink }} 
          curl https://downloads.lambdatest.com/hyperexecute/windows/hyperexecute.exe -o hyperexecute.exe 
          chmod +x hyperexecute
          export LT_USERNAME=${{ github.event.inputs.username }} 
          export LT_ACCESS_KEY=${{ github.event.inputs.accessKey }} 
          ./hyperexecute --user $LT_USERNAME --key $LT_ACCESS_KEY --download-artifacts --config .hyperexecute_autosplit.yaml   
          echo "Test completion"
      - name: Report Generation
        run: cat coverage\Chrome 109.0.0.0 (Windows 10)\reports.txt  
```
- On:
	- Workflow_dispatch: In the workflow_dispatch section, you should declare the pre-defined variables that will be used before running the GitHub Actions PipeLine as an input.

- Jobs: 
	- In the Jobs Section, declare the workflow of the pipeline execution.
	- runs-on: Runs-on contains the value of the OS flavor you would like to execute the GitHub Actions Pipeline on.

- Steps: In the steps section, you should declare the execution commands.
	- The first step in the above sample YAML changes the path of the root directory.
	- In the second step it downloads the HyperExecute CLI binary.
	- The third step is the execution command which executes The Hyperexecute CLI binary. This contains LambdaTest username, access key, and path of the Yaml created for Hyperexecute. You can find more information on this here.

![image](https://user-images.githubusercontent.com/128702372/234122457-83f7faf8-cd41-4095-a0b9-7ca9a8758fc8.png)


### 4. Run the Workflow

To run the new pipeline that you just created, click the Run workflow button on the workflow page.

![image](https://user-images.githubusercontent.com/128702372/234122584-b45d3421-399d-47ce-8c44-bc5781aa0f8a.png)

### Below is an example of how a test that is run on the GitHub Actions pipeline gets executed:

![image](https://user-images.githubusercontent.com/128702372/234123008-421d6b7b-a149-4a15-8334-cbfaaff85509.png)

### Below is an example of a Hyperexecute job that was triggered through the above pipeline:

![image](https://user-images.githubusercontent.com/128702372/234123083-38aca641-0248-491d-ba6a-1ce710540789.png)


## We are here to help you :)
* LambdaTest Support: [support@lambdatest.com](mailto:support@lambdatest.com)
* HyperExecute HomePage: https://www.lambdatest.com/support/docs/getting-started-with-hyperexecute/
* Lambdatest HomePage: https://www.lambdatest.com



## How does Bootstrap's test suite work?

Bootstrap uses [Jasmine](https://jasmine.github.io/). Each plugin has a file dedicated to its tests in `tests/unit/<plugin-name>.spec.js`.

- `visual/` contains "visual" tests which are run interactively in real browsers and require manual verification by humans.

To run the unit test suite via [Karma](https://karma-runner.github.io/), run `npm run js-test`.
To run the unit test suite via [Karma](https://karma-runner.github.io/) and debug, run `npm run js-debug`.

## How do I add a new unit test?

1. Locate and open the file dedicated to the plugin which you need to add tests to (`tests/unit/<plugin-name>.spec.js`).
2. Review the [Jasmine API Documentation](https://jasmine.github.io/pages/docs_home.html) and use the existing tests as references for how to structure your new tests.
3. Write the necessary unit test(s) for the new or revised functionality.
4. Run `npm run js-test` to see the results of your newly-added test(s).

**Note:** Your new unit tests should fail before your changes are applied to the plugin, and should pass after your changes are applied to the plugin.

## What should a unit test look like?

- Each test should have a unique name clearly stating what unit is being tested.
- Each test should be in the corresponding `describe`.
- Each test should test only one unit per test, although one test can include several assertions. Create multiple tests for multiple units of functionality.
- Each test should use [`expect`](https://jasmine.github.io/api/edge/matchers.html) to ensure something is expected.
- Each test should follow the project's [JavaScript Code Guidelines](https://github.com/twbs/bootstrap/blob/main/.github/CONTRIBUTING.md#js)

## Code coverage

Currently we're aiming for at least 90% test coverage for our code. To ensure your changes meet or exceed this limit, run `npm run js-test-karma` and open the file in `js/coverage/lcov-report/index.html` to see the code coverage for each plugin. See more details when you select a plugin and ensure your change is fully covered by unit tests.

### Example tests

```js
// Synchronous test
describe('getInstance', () => {
  it('should return null if there is no instance', () => {
    // Make assertion
    expect(Tab.getInstance(fixtureEl)).toBeNull()
  })

  it('should return this instance', () => {
    fixtureEl.innerHTML = '<div></div>'

    const divEl = fixtureEl.querySelector('div')
    const tab = new Tab(divEl)

    // Make assertion
    expect(Tab.getInstance(divEl)).toEqual(tab)
  })
})

// Asynchronous test
it('should show a tooltip without the animation', () => {
  return new Promise(resolve => {
    fixtureEl.innerHTML = '<a href="#" rel="tooltip" title="Another tooltip"></a>'

    const tooltipEl = fixtureEl.querySelector('a')
    const tooltip = new Tooltip(tooltipEl, {
      animation: false
    })

    tooltipEl.addEventListener('shown.bs.tooltip', () => {
      const tip = document.querySelector('.tooltip')

      expect(tip).not.toBeNull()
      expect(tip.classList.contains('fade')).toEqual(false)
      resolve()
    })

    tooltip.show()
  })
})
```
