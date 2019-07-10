

# Tests
**Tasty&reg;** handles 2 types of tests: **functional** and **load**.
In the functional type of tests you scrupulosly write all the cases to check whether the functionality of your designed API matches all the requirements you have. However, when you write load tests, you just bother about how your API performs under high load and how it does handle requests under such pressure.
So, in load tests you just care about load scenarios, while when doing functional tests, you care about all the cases covering all the functions of API.
Let's have a look at load testing first. Designed as a lighter "alternative" to functional tests, it provides all the statistics for...

## Contents
1. [Overall structure of tests in channel](#overall_schema)
2. [Detailed structure of tests in channel](#detailed_schema)
3. [Application declaration block](#app)
4. [Test description](#test)
5. [Services configuration](#services)
6. [Additional libraries](#libs)
7. [Other files and folders](#additional)
8. [Quick start](#quick-start)
    - 8.1 


## 1. Structure of directories in single channel <a name="overall-schema"></a>
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
