# App declaration and index
## Structure inside the test folder
```cmd
your_test_folder
└───app
    │   |   index.js      
    │   ├───endpoint1      
    │   |       index.js   
    │   |       
    │   └───endpoint2
    │           index.js
```

[index.js](#App)<br/>
The code inside the file 
### **./app/index.js** 
should look like this:
```javascript
const {App} = require('tasty');
const app = module.exports = new App('appName', {
    host: {
        develop: 'http://localhost:3000',
        testing: 'http://localhost:7777/api/v1',
        product: 'http://localhost:7777/api/v1',
    },
    //config for artillery
    load: {
        phases: [{
            duration: 10,
            arrivalRate: 1
        }]
    }
});

app.init();
```

First, you require the tasty framework, then import the App class from it, configure necessary addresses and set default configuration for load tests. After that initiate the *app.init* to start tests properly.


[{endpoint_folder}/index.js](#folders)<br/>
The /app directory contains some folders having all the declaration related to each endpoint of the API that has been designed. For example, contents of the file:<br/>
### **./app/endpoint/index.js**
```javascript
const app = require('../');

app.declare({
    url: '/advices/getAdvices', //(1)
    alias: 'getAdvices',        //(2)
    headers: {},
    params: {},
    methods: ['get'],           //(3)
});
app.declare({
    url: '/advices/findAdvice',
    alias: 'findAdvice',
    headers: {},
    params: {},
    methods: ['get'],
});
app.declare({
    url: '/auth/headerCapture',
    alias: 'login',
    headers: {},
    params: {},
    methods: ['post'],
});
app.declare({
    url: '/basket/get',
    alias: 'basketGet',
    headers: {},
    params: {},
    methods: ['get'],
});
app.declare({
    url: '/basket/add',
    alias: 'basketAdd',
    headers: {},
    params: {},
    methods: ['post'],
});

```
The example shows that the declaration of two endpoint tests has been made: (for the development environment) *http://localhost:3000/advices/getAdvices* and *http://localhost:3000/advices/findAdvice*
#### Explanations:
1. The address is relative, because it is defined upper in ./app/index.js;
2. Alias is the pattern, by witch it can be executed further in the test, e.g. *app.getAdvices.get()*
3. methods of HTTP requests: GET,POST,PUT,... select it **in lowercase**

You can also define here some headers, body, params as you want. The framework is quite flexible to provide the configuration change of request parameters at every step of test execution. You have to initiate tests by declarations further in next steps.

[Back to the CookBoock](../CookBook.md#app)

