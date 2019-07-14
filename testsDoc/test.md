# App declaration and index
This article shows how to write your own custom tests via **Tasty**. The procedure of writing tests differs only for the type of test, but the syntax is almost the same
## Structure inside the test folder
```cmd
your_test_folder
└───test
    ├───func
    │   ├───endpoint1
    │   │       index.js
    │   │
    │   └───endpoint2
    │           index.js
    │
    │
    └───load
        ├───endpoint1
        │       index.js
        │
        └───endpoint2
                index.js
```

As mentioned earlier, 
#### Possible example of functional test:
  - get all the products and check whether the response matches required pattern
  1. request to the service
  2. check the response for correct structure
 #### Possible example of load test
  1. login 
  2. get all the products 
  3. add product to the cart 
  4. checkout
 
 So, in each type of tests there has to be its own test
 ## Functional test
### ./test/func/endpoint1/index.js
```javascript 
import tasty from 'tasty';
import app from '../app';

tasty.case('Tests for /product',        (1)
  app.login                             (2)
    .setMock({                          (3)
      token: 'some mock server token',
    })
    .post({                             (4)
      capture: {                        (5)
        json: '$.token',
        as: 't',
      },
    }),
  tasty.suite(                          (6)
    'Response structure',
    app.product.get(),                  (7)
    {
      checkStatus: 200,
      checkStatusText: 'OK',
      checkStructure: true,
      check: (res, ctx) => ctx.t === 'some mock server token',
    },
  ),
  app.logout.post(),                    (8)
);
```
#### Explanations
 ## Load Test
 

### ./test/load/endpoint1/index.js
```javascript
const tasty = require('tasty').tasty;   // 1
const app = require('../../../app');    // 2

module.exports = tasty.case(            // 3
    'basket operations',                // 4
    app.login.post({                    // 5
        body: {
            login: "vasya",
            password: "12345"
        },
        capture: [/*{
            json: "$['access-token']",
            as: "AT"
        },*/
            {
                header: "access-token",
                as: "AT"
            }]
    }),
    tasty.think(5),
    app.basketAdd.post({
        body: {
            'access-token': '${AT}'
        }
    }),
    app.basketGet.get({
        headers: {
            'access-token': '${AT}'
        }
    }),
    tasty.log("Let's get out of here!")
);
```
#### Explanations:
1. The connection of tasty framework
2. require all the application declarations, defined on previous step([App declaration and index](App.md))
3. Define the scenario of test by ```tasty.case()``` (More info here: [API Reference](../API.md))
4. The title of test scenario
5. Entity representing the tasty "action" type definition (tasty.case means we define the case where we do some actions)

- In this particular example the scenario of the test is the following:
  1. login the web resource that we are checking(```app.login```);
  2. emulate user waiting for 5 seconds (```tasty.think```);
  3. add some goods to the basket(```app.basketAdd```);
  4. get the contents of the created basket (```app.basketGet```);
  5. log some messages for the testing process(```tasty.log```);
  
```app.login.post(), app.basketAdd.post(), app.basketGet.get()```: initiate the "post" and "get" http methods of declaration with alias "login", "basketAdd", "basketGet" defined before in the [App section](App.md):
```javascript
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

### Supported methods
#### Functional tests
 - **Tasty** supports all of the types described in API reference [here](../API.md)
#### Load tests
- By default, for the load tests, **Tasty** supports all of the **App** class instance methods
- tasty.case()
- 2 additional methods: 
  - *tasty.think()* and 
  - *tasty.log()*
