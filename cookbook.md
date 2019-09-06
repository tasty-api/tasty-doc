

# Tasty&reg; Endpoint test system
The way to simplify and automate the way you test your endpoint functionality.

## Contents
1. [Introduction](#introduction)
    - 1.1 [Quick start](#quick-start)
    - 1.2 [Overall structure of tests in channel](#overall_schema)
    - 1.3 [Detailed structure of tests in channel](#detailed_schema)
    - 1.4 [Application declaration block](#app)
    - 1.5 [Test description](#test)
    - 1.6 [Services configuration](#services)
    - 1.7 [Additional libraries](#libs)
    - 1.8 [Other files and folders](#additional)
    - [Summary](#summary-1)
2. [Types of tests](#types-of-tests)
    - 2.1 [Functional tests](#functional-tests)
    - 2.2 [Load tests](#load-tests)
    - [Summary](#summary-2)
3. 

## 1. Introduction <a name="introduction"></a>
Welcome to Tasty framework! 

Let's start first with the description of abilities that you can use for writing various tests. We hope that the framework will be flexible enough to satisfy all your needs.

For the first time, you have only to know the basics of JavaScript and Node.js to work with your tests. You do not need to learn the semantic language of each framework(like mocha, artillery, etc.) that are adapted for different kinds of tests, the system "wraps" your tests and standardizes the output. Like in translator, you write in one language you know well, and the program makes it clear to other languages.

**Tasty&reg;** handles 2 types of tests: **functional** and **load**.
In the functional type of tests you scrupulosly write all the cases to check whether the functionality of your designed API matches all the requirements you have. However, when you write load tests, you just bother about how your API performs under high load and how it does handle requests under such pressure.
So, in load tests you just care about load scenarios, while when doing functional tests, you care about all the cases covering all the functions of API.

### 1.1 Quick start.
In come cases, it is not always possible to ask your DevOps engineers to initialize test repositories and give you all the functionality of testing system, so let us start deploying your first test repository from scratch. 

#### 1.1.1. Create and init your new test repository.
 1. Create new folder:
 ```bash
 mkdir tasty_app
 ```
 2. Create 2 folders inside your newly created folder:
 ```bash
cd tasty_app
mkdir app test
```
**Theese two folders are mandatory for the test system!**

3. As we are going to create functional and load tests, create the "func" and "load" directories for functional and load tests respectively
```bash
cd test
mkdir load func
```F
Change directory which will be the root of the project:
```bash
cd ..
``` 
the structure of your project should be the following:
```bash
tasty_app
├── app
└── test  
    ├── func
    └── load
```
4. Initialize your new test-system repo via npm and install the package named "tasty-api":
(press "Enter" by default on each line)   
```bash
npm init
npm install tasty-api
```
Result:
```bash
npm i tasty-api

> jsonpath@1.0.2 postinstall /......./node_modules/jsonpath
> node lib/aesprim.js > generated/aesprim-browser.js

npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN tasty_app@1.0.0 No description
npm WARN tasty_app@1.0.0 No repository field.

+ tasty-api@0.3.2
added 500 packages from 815 contributors in 70.157s
```
the structure of your project should be the following:
```bash
tasty_app
├── app
├── package.json
├── package-lock.json
├── node_modules
└── test
    ├── func
    └── load
```
#### 1.1.2. Creating index.js in project folder
create file index.js and paste this code:
```javascript
const path = require('path');                               (1)
const { Runner } = require('tasty-api');                    (2)
    
module.exports = new Runner(path.join(__dirname, 'test'));  (3)
```
Where: 
- 1 - include the path library for resolving paths
- 2 - import test runner function to use it
- 3 - create new test-runner instance and pass is for export so the Tasty framework could handle it. 
#### 1.1.3. Create declarations inside the /app folder
**Firstly**, create **index.js** inside the /app folder and paste this code snippet:
```javascript
const path = require('path');
const { App } = require('tasty-api');

const app = module.exports = new App('lego', {
    host: {
        develop: 'https://localhost:4000',(1)
        testing: 'http://localhost:4000', (2)
        product: 'http://localhost:4000', (3)
    },
    trace:{
        develop: 'https://localhost:4000',
        testing: 'http://localhost:4000',
        product: 'http://localhost:4000',
    }
});

app.init(path.join(__dirname));
```
Where:
 - 1 - configuration address for the "develop" environment
 - 2 - configuration address for the "testing" environment
 - 3 - configuration address for the "product" environment
 
The **trace** section is optional. Leave this section as it is. The trace configuration is for connecting and interaction with the Jaeger service (to show Jaeger-tracer hyperlinks inside of functional test results)

**Secondly**, create the "/endpoints" folder and your endpoints' subfolders inside the /app directory:
In this example we will create two subfolders for authorization:
```bash
@TODO tree ../tsaty_app
```
#### 1.1.4. Create schemas for validation
description of schemas: how are they interconnected with the tests
description of appropriate JSON and JOI schemas with references, notice that this will be described later in a cookbook
#### 1.1.5. Initialize your first test inside /test folder 
Create the following test files inside the /test folder
#### 1.1.6 Perform test
TODO
initialize the testing process by the command:  
```bash
npm run test
```
or
```bash
npx tasty
```

#### 1.1.7 Bindings
(this section will provide the basic info about how declaration block is connected with the test instance: what possibilities are mentioned in the test examples, and for the further info see below chapter #<some_number> )

#### 1.1.7
#### Appendix
Because of the use of "Mocha" for functional and "Artillery" for load testing, there are additional configuration options 
If you want to use extra features of tests configuration, you may create additional files ".mocharc.js" and ".artilleryrc.js" in root folder of the project
```bash
touch .mocharc.js .artilleryrc.js
```
For further information about configuration files for specific framework, please refer to appropriate documentations:

[Mocha configuration example](https://mochajs.org/#configuring-mocha-nodejs)

[Artillery configuration example](https://artillery.io/docs/script-reference)

The example of .mocharc.js:
```javascript
module.exports = {
  reporter: "mochawesome",        // Reporter name.
  reporterOptions: {              // Reporter settings object.
    reportDir: 'reports/func/',
    reportFilename: 'index.js',
  },
  color: true,                    // Color TTY output from reporter?
  // allowUncaught: "boolean",    // Propagate uncaught errors?
  // asyncOnly: "boolean",        // Force done callback or promise?
  // bail: "boolean",             // Bail after first test failure?
  // checkLeaks: "boolean",       // If true, check leaks.
  // delay: "boolean",            // Delay root suite execution?
  // enableTimeouts: "boolean",   // Enable timeouts?
  // fgrep: "string",             // Test filter given string.
  // forbidOnly: "boolean",       // Tests marked only fail the suite?
  // forbidPending: "boolean",    // Pending tests fail the suite?
  // fullStackTrace: "boolean",   // Full stacktrace upon failure?
  // global: "Array:string",      // Variables expected in global scope.
  // grep: "RegExp | string",     // Test filter given regular expression.
  // growl: "boolean",            // Enable desktop notifications?
  // hideDiff: "boolean",         // Suppress diffs from failures?
  // ignoreLeaks: "boolean",      // Ignore global leaks?
  // invert: "boolean",           // Invert test filter matches?
  // noHighlighting: "boolean",   // Disable syntax highlighting?
  // retries: "number",           // Number of times to retry failed tests.
  // slow: "number",              // Slow threshold value.
  // timeout: "number | string",  // Timeout threshold value.
  // ui: "string",                // Interface name.
  // useInlineDiffs: "boolean",   // Use inline diffs?
};
```
The example of .artilleryrc.js
```
module.exports = {
  config: {
    tls: {
      rejectUnauthorized: false,
    },
    http: {
      timeout: 10,
    },
    phases: [{
      duration: 10,
      arrivalRate: 1,
    }],
  },
};
```

You just have to create those configuration files and that is all! This framework automatically identifies if those 2 files exist and uses them when initializing the app.

### 1.2 Structure of directories in single channel <a name="overall-schema"></a>
The structure of your project inside the channel application should look like this:
```bash
\---tasty
    +---app                          (1)
    |   \---endpoint_test            
    +---artillery_report             (2)
    +---libs                         (3) 
    +---services                     (4)
    |   \---static_service           
    \---test                         (5)
        +---func                     
        \---load                     
       
```
It's not mandatory to call the folder "tasty", the name can be any. It is convenient to place tests project folder inside of your channel's folder. 
### Explanations:
 1. The *app* folder contains the declaration block for your tests, no matter they are functional or load;
 2. *artillery_report* is a temporary folder for load tests' output files;
 3. *libs* folder contains the necessary libraries for your test project
 4. *services* directory contains all the static config data for the outer services that you will execute during the tests(e.g. headers for yandex.weather, google.maps, etc.)
 5. *test* - main project folder containing functional and load tests

<a name="detailed_schema"></a>
## 2. Detailed View: 
```cmd
tasty
│   artilleryConfig.js
│   artilleryConfigFinal.json
│   index.js
│   package.json
├───app
│   |   index.js       
│   ├───endpoint1
│   |       index.js
│   |       
│   └───endpoint2
│           index.js
│           
├───artillery_report
│       ._load.tmp.output.json
│       ._load.tmp.output.json.html
│       
├───libs
│       log.js
│       
├───services
│   |   index.js
│   |   
│   ├───service1
│   |       index.js
│   |       
│   └───service2
│           index.js
└───test
    ├───func
    |   ├───endpoint1_func_test
    |   |   index.js
    |   |    
    |   └───endpoint2_func_test
    |       index.js
    |       
    └───load
        └───endpoint1_load_test
            index.js
                     
```
<a name="app"></a>
## 3. Application declaration block
Let's have a look at first folder of index: App 
```cmd
app
│   index.js        (1)
│
├───endpoint1       (2)
│       index.js    (3)
│
└───endpoint2
        index.js
```
1. index.js contains all the necessary settings for configuring the application environment;
2. The folder containing declarations for the endpoints, for convenience, called the same as your endpoints;
3. index.js represents the file containing the code for tests' declarations.

For more information see [App declaration and index](testsDoc/App.md)

<a name="test"></a>
## 4. Test description
Another important folder to execute is **./test** 
```cmd
test
├───func              (1)
│   ├───endoint1          (3)
│   │       index.js      (4)
│   │  
│   └───endpoint2     
│           index.js  
│   
└───load              (2)
    ├───endpoint1
    │       index.js
    │  
    └───endpoint2
            index.js
            
```
1. Functional tests folder;
2. Load tests folder;
3. Custom endpoint folder. Contains file or files that logically match one single endpoint testing procedure;
4. Single file or a bunch of files containing functionality of single endpoint test.  

The test folder contains 2 subfolders for load and functional testing, each of which contains subfolders containing scripts defining scenarios of testing.
 #### Possible example of functional test:
  - get all the products and check whether the response matches required pattern
  1. request to the service
  2. check the response for correct structure
 #### Possible example of load test
  1. login 
  2. get all the products 
  3. add product to the cart 
  4. checkout
For more information see [Test description](testsDoc/test.md)


  <a name="services"></a>
## 5. Configuration of services

```cmd
services
│   |   index.js
│   |   
│   ├───service1
│   |       index.js
│   |       
│   └───service2
│           index.js

```
## 6. Libraries
```cmd
│
└───libs
        log.js
```




```cmd
tasty
│   index.js                      (1)
│   package.json                  (2)
│   artilleryConfig.js            (3)
│   artilleryConfigFinal.json     (4)
```
1. index.js contains 
4. artilleryConfig.js is a basic configuration for load tests, that contains the target, phases of load test, network settings, etc.

```javascript
module.exports = {
    config: {
        target: "/",
        tls: {
            rejectUnauthorized: false
        },
        http: {
            timeout: 10
        },
        phases: [{
            duration: 5,
            arrivalRate: 3
        }]
    }
};
```

##Structure of load tests
1. [Declarations]()
2. [Test run scenarios]()

Each channel should have a folder with tests
The structure of tests 

###Declarations block
