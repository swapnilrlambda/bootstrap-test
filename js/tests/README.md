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

<img height="25" alt="hyperexecute_logo" src="https://user-images.githubusercontent.com/1688653/159473714-384e60ba-d830-435e-a33f-730df3c3ebc6.png">

HyperExecute is a smart test orchestration platform to run end-to-end Selenium tests at the fastest speed possible. HyperExecute lets you achieve an accelerated time to market by providing a test infrastructure that offers optimal speed, test orchestration, and detailed execution logs.

<!---If logged in, it will be redirected to Gitpod editor in new tab where current tab will show hyperexecute dashboard.

If not logged in, it will be redirected to Login/Signup page and simultaneously redirected to Gitpod editor in a new tab where current tab will show hyperexecute dashboard.

If not signed up, you need to sign up and simultaneously redirected to Gitpod in a new tab where current tab will show hyperexecute dashboard.--->

# Detailed Description of Hyperexecute Yaml

1. runson

The runson key determines the platform (or operating system) on which the tests are executed. Here we can use win for Windows, mac for MacOS, linux for Linux.

```yaml
    runson: win   
```

2. autosplit

autosplit feature comes handy when you want to run tests at predefined concurrency and distribute the tests over the available infrastructure. Setting this boolean value to true enables autosplit.

```yaml
    autosplit: true   
```

3. retryOnFailure

In case any test fails and you want to retry it, set this boolean value to true. The maxRetries key determines how many times your tests will get retried.

```yaml
    retryOnFailure: true    
```
4. maxRetries

The maximum number of times your tests can be retried. You can allocate a numerical value between 1 and 5 for this field.

```yaml
    maxRetries: 2   
```

5. testDiscovery

This is the command used to discover (or locate) relevant tests via class names, filters, file names, and more.

```yaml
testDiscovery:
  type: raw
  mode: dynamic
  command: |
            printf 'chromeWin10\nchromeWin10Latest\nfirefoxWin10\nfirefoxWin10Latest'
```

6. testRunnerCommand

This is the command that your testing framework uses to trigger the execution of the test.

```yaml
testRunnerCommand: node_modules\.bin\karma start js\tests\karma.conf.js --browsers=$test
```

7. vault

A vault is a secure place to safe-keep data and sensitive company information while you're running tests.

```yaml
  env:
    PAT: ${{ .secrets.Token }}  //PAT: Personal Access Token
```

8. concurrency

Concurrency indicates the number of concurrent sessions on HyperExecute.

```yaml
  concurrency: 1   
```

9. preDirectives or pre

preDirectives or pre includes all the actions one needs to perform before test execution, such as, installing dependencies.

- If both pre or preDirectives flags are used at the same time, then the precedence is given to the preDirectives flag.
- maxRetries: You can retry the commands that failed in the pre-stage by using this flag. The numerical value assigned to this field determines the amount of times you can retry the failed commands.

```yaml
  preDirectives:
    commands: 
      - mkdir -p m2_cache_dir
      - mvn -Dmaven.repo.local=$CACHE_DIR -Dmaven.test.skip=true clean install  
    maxRetries: 3
```

10. postDirectives or post

postDirectives or post includes all the actions one needs to perform after test execution.

- If both post or postDirectives flags are used at the same time, then the precedence is given to the postDirectives flag.

```yaml
  postDirectives:
    - cat yaml/win/*.*hyperexecute_autosplits.yaml 
```

11. cachekey

A cache key is the unique identifier for every object in the cache of the testsuite.

```yaml
  {{ checksum "package-lock.json" }}
```

12. cacheDirectories

Cached directories are the dependency directories of your test suite such as node modules. HyperExecute caches them to help fasten your test execution time the next time you run your job. However, if you modify the dependency directories, they will be downloaded, resolved and cached again.

```yaml
  cacheDirectories:
    - CacheDir
```

13. uploadArtifacts

The uploadArtifacts key contains the path and the name of the file which you want to download.

```yaml
uploadArtefacts:
 - name: Reports
   path:
    - coverage/**/**
```

14. runtime

This flag provides the runtime for specific languages in specific versions.

```yaml
  runtime:
    language: dotnet
    version: "6.0.303"
```

15. jobLabel

The jobLabel YAML key is used to add tags or labels to jobs. This allows you to search your jobs using the labels or tags assigned to them.

- Prioritize Your Job Pipeline: To prioritize your jobs, you need to add the required priority to the jobLabel key in the YAML file e.g jobLabel: [ 'high', 'Low','medium']. With 'high' priority jobs triggered first, followed by medium priority jobs and finally low priority jobs. The values are case insensitive and the default priority is 'medium'.
- You can also use it along with your existing job labels like this:

```yaml
    jobLabel: [ '${DATE} - ${DAY}','Foo','Bar', 'low']
```

16. failFast

This feature allows you to either run your jobs faster or fail fast to provide faster feedback and save your test time.

- maxNumberOfTests: You can customize your Fail Fast feature according to your needs by providing the number of consecutive tests that can fail in this section.

```yaml
      failFast:
    maxNumberOfTests: 2
```

17. report

This field allows you to generate the test reports in the location of your choice.

- email: You can also get your job reports as an email by conifguring the email key under partialReports.

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

# Steps to integrate GitHub Actions with HyperExecute

There are three simple steps to integrate HyperExecute with GitHub Actions

### 1. Download HyperExecute CLI

HyperExecute CLI gets downloaded from https://downloads.lambdatest.com/hyperexecute/windows/hyperexecute.exe

### 2. Grant Permissions

Provide permissions to the HyperExecute CLI to start working using following command - chmod +x hyperexecute

### 3. Run the HyperExecute Command

Use the following command to run the test - ./hyperexecute --user $LT_USERNAME --key $LT_ACCESS_KEY --download-artifacts --config .hyperexecute_autosplit.yaml

Below is the GitHub Actions yaml for HyperExecute: 

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
