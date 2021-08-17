 
## Pull Requests

*   Keep the Pull requests independent, and split the PRs if necessary

*   Steps to follow for a Pull Request
    *   Create a Pull Request and add reviewers
    *   After review, while merging follow the below rules
        *   if the PR is merged from feature branch to Develop branch, then always Squash and Merge
        *   if the PR is merged from Develop to Master, then always Only Merge


### Common GIT commands

#### Rebase
```
// rebase on top of Develop branch
git rebase origin/develop

// run in interactive mode
git rebase -i origin/develop

// add resolved commit
git add <file-name>

// continue with rebase
git rebase --continue

// abort a rebase, Save any changes before running the above command
git rebase --abort

// To remove a latest commit, we can rebase
git rebase -i HEAD~1

```

## Style Guide

- General
    
    * Use `kebab-case` for file names
 
   ```
   fetch-reservations.ts
   reservation-list.vue
   ```
 
    * File names should be `lowercase`
 
    * directory names should be `lowercase`
 
    * function or method names are `camelCased`
 
        ```
        getReservations()
        findProperty()
        ```
 
    * class names are `PascalCased`
 
    * Alignment
 
        We are following below code alignment styles;
 
        ```
        // BAD
        function a() {
        
            return {
            x: 10, y: 20,
            z: 30,
            }
        }
        
        // GOOD
        function a() {
        
            return {
            x: 10,
            y: 20,
            z: 30,
            }
        }
        ```
 
    * Line endings for file
 
        ```
        function fetch() {
        
        }
        
        ```
 
        Note:  
            code generated files like graphql types, client code can be excepted  
            infratructure files like .tf(terraform) should have line endings
 
        * Line length should not be greater than 80 characters
 
        * Do not add semicolon after a import, statement or function or class
 
        ```
        // BAD
        import lodash from 'lodash';
        // GOOD
        import lodash from 'lodash'
        ```
 
    * (2) indent spaces for blocks
    
        ```
        // BAD
        function fetchData() {
        
            return {
                count: 100,
            }
        }
        
        // GOOD
        function fetchData() {
        
            return {
            count: 100,
            }
        }
        ```
        Note: Can be configured for linting in tsconfig.json as in [here](https://palantir.github.io/tslint/rules/indent/)
 
    * Give a line spacing after start and end of a function
    
        ```
        function getReservations() {
        
            const reservations = awaitReservation.findAll()
        
            return reservations
        
        }
        ```
 
    * Give a line spacing after start and end of test or suite
 
        ```
        describe('reservation', () => {
        
            it('should return a reservation', () => {
        
            })
            
        })
        ```
    * Empty blocks
 
        ```
        // OK
        function notImplemented() {}
        
        // BAD
        if (some-condition) {
        
        }
        else {
            doWork()
        }
        
        ```
 
    * No line continuations
 
        ```
        // NOT OK
        const longString = `Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi in elit eget est placerat maximus. Donec rhoncus, elit ut fermentum \ porttitor, mauris eros semper arcu, vestibulum porttitor mi orci sit amet lacus. Pellentesque dictum, sapien ut dignissim feugiat, ligula'
        
        // OK
        const longString = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi in elit eget est placerat maximus. Donec rhoncus, elit ut fermentum ' +
        'porttitor, mauris eros semper arcu, vestibulum porttitor mi orci sit amet lacus. Pellentesque dictum, sapien ut dignissim feugiat, ligula'
        ```
 
- **Project**
 
     * Use third party libraries  
        The benefits are they are tried and tested.  
    example.  
    Lets say we have a requirement to create and format a csv, rather than build the functionality and write extensive tests, look for a good third party library
 
    * use `snake-case` for enum names
 
        ```
        enum Status {
            confirmed
            incomplete_payment
        }
        ```
    * `constants` for primitives should be `UPPERCASED`
 
    * for prometheus metrics, prefer to use `total` as suffix
 
        ```
        // NOT OK
        jobRunCount
        // OK
        jobRunTotal
        ```

    * Logger

        * Add a trace token and any reference like property id in the logger message to trace in Graylog

            ```
            logger.log({ traceToken, propertyId }, 'message')
            ```

    * Add CHANGELOG to api client, if any changes to specifications

            ```
            # Changelog
            All notable changes to this project will be documented in this file.

            The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
            and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

            ## [3.0.0]
            ### BREAKING CHANGE
            - add platformHelpUrl to partner links
            - update API response for partners, properties, partner-product and property-product
            - suffixed the link properties with Url to match the implementation and domain model
            ```
    
    * Generate api client and bump version

        ```
        npm version minor
        npm version patch
        ```
    * All API should contain the trace token `x-sm-trace-token` and mandatory

- **Typescript**
 
    * Prefer `arrow` function
 
        ```
        // NOT OK
        function x () {
        
        }
        
        // OK
        const x = () => {
        
        }
        ```
    * use template string literals to append variables
        
        ```
        // BAD
        const text = 'the currency is ' + currency
        
        // GOOD
        const text = `the currency is ${currency}`
        ```
    * use `single quote` for string literals
 
        ```
        // BAD
        const currency = "AUD"
        
        // GOOD
        const currency = 'AUD'
        ```
    * prefer defensive or guard style checks
 
        ```
        const convert = (
            checkedOutDate: Date,
            amount: number
        ): number => {
        
            if (amount < 0) return
            if (checkedOutDate > now()) return
        
            // do coversion and return
        
        }
        ```
 
    * use `const` as default, unless needs to be redefined , then use `let`
 
        Note: Do not use `var` (global)
        
        ```
        const property = await await Property.findOne()
        ```
    * Always declare `types`
 
        * type safety
        * readability and clarity
 
        ```
        const print = (): void => { logger.info('some log') }
        
        const convert = (
            checkedOutDate: Date,
            amount: number
        ): number => {
        
        }
        
        ```
    * Do not use `any` type
 
        * we are not getting type safety
        * potential for run time error
 
        Can be used if type is not known at all during development time.
 
    * Do not use one variable declaration
 
        ```
        // BAD
        const a = 1, b = 2
        
        // GOOD
        const a = 1
        const b = 2
        ```
 
    * Add trailing commas for `object` and `array` literals
 
        Include a trailing comma whenever there is a line break between the final property and the closing brace.
 
        ```
        const points = {
            x,
            y,
            z,
        }
        
        const currencies = [
            'USD',
            'AUD',
        ]
        ```
 
    * use objects as argument if there are multiple 
 
         Benefits:
 
        * more readability while calling the function
        * avoid errors like misplaced arguments
        * do not have to pass all arguments
 
        ```
        // BAD
        function convert(
            checkedOutDate: Date,
            amount: Float,
            localCurrency: String,
            billingCurrency: String
        ) {
            // convert
        }
        
        // GOOD
        function convert(
            args: {
            checkedOutDate: Date,
            amount: Float,
            localCurrency: String,
            billingCurrency: String
            }
        ) {
            // convert
        }
        
        convert({
            foreignExchangeRate: 1.1,
        })
        
        ```
 
        Note: We have agreed to use this pattern if arguments are more than 3.
 
    * Advanced for-loop syntax
 
        ```
        // NOT OK
        for (let i = 0; i < properties.length; i = i + 1) {
        
        }
        
        // GOOD - arrays
        for (let property of properties) {
        
        }
        
        // looping for objects
        Object.keys()
        
        //
        ```
 
        Note: lint rules might complain, and can be silenced with a ignore.
  
    * usage of `this`
 
        * only use in classes and methods
        * only use in arrow functions declared inside classes and methods
    
    * Destructuring
 
        * Array
 
            ```
            const [property1, ...restOfProperties] = properties
            ```
 
            Note: for concat of array, the below syntax can be used
            ```
            const newArray = [...array1, ...array2]
            ```
        * Onject
 
            ```
            const { spid, name } = property
            ```
 
    * Empty catch blocks should be avoided
        
        ```
        try {
            shouldFail();
            fail('expected an error');
        } catch (expected) {
        
        }
        ```
 
    * Do not `mutate` objects
 
        ```
        // BAD
        currenctArray[0] = 'abc'
        
        // GOOD
        
        const newArray = [... currentArray]
        newArray[0] = 'abc'
        ```
 
    * Prefer `generics` for declaring function and object signatures
 
    * Use `utility types` for creating custom types
 
        ```
        type Property = {
            spid: string
            name: string
        }
        
        type Reservation = {
            bookingReferenceId: string
        }
        
        type PropertyBill = Pick<Property. 'spid>> & Reservation
        
        ```
 
        Some times we want only a subset of atributes of a type
 
        ```
        type Reservation = {
            id: number;
            bookingReferenceId: string;
        }
        
        type ReservationParams =
        Partial<Omit<Reservation, "id">>
        
        ```
 
    - Graphql
 
        * type names are PascalCase (first letter capitalized)
 
        * Do not add commas for graphql types
 
            ```
            // BAD
            type Reservation {
                uuid: String,
                commissionAmount: Float,
            }
            ```
 
        * Use List as naming convention for arrays
 
            ```
            // BAD
            type ReservationCollection {
            
            }
            
            // GOOD
            type ReservationList {
            
            }
            
            query property {
            reservations: ReservationList!
            }
            ```
 
    * Database
 
         * factory
 
            * prefer `createMany` to create multiple instances
            
            ```
            // NOT OK
            const property1 = factory.create(Property.name)
            const property2 = factory.create(Property.name)
            const property2 = factory.create(Property.name)
            
            // OK
            const properties = factory.create(Property.name, 3)
            ```
        
        * Use `snake_case` for database column name

            ```
            In the Sequelize model, always specify the column name (field)

            uuid: {
                field: 'uuid',
                type: Sequelize.UUID,
                allowNull: false,
                },
            localCurrency: {
                field: 'local_currency',
                type: Sequelize.CHAR,
                allowNull: false,
                },
            ```
 
* Terraform
 
 
