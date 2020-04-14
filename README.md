# plugged-controller

control several interfaces by plugged components


## scenário

- screens could show lists
    - it can need filters and sorters to run
    - it can use api calls
    - it can use programmatic filters and sorters after load

- screens could show input forms
    - it can use api to load data
    - it can use programmatic loaders
    - it can need validations before send
    - it can use api to send data
    - it can show success or failure messages after save

## suggested usage

- definition

        let controller = new PluggedController([
            new ApiPlugin({ url: '/clients' }), // it maps functions for data manipulation
            new ListPlugin({ page: 1 }), // it maps list and filters data
            new InputPlugin(), // it maps input data and functions for send the input data
            new RulesPlugin({
                name: { required: true },
                age: { min: 0, max: 150, required: true },
                registrationNumber: { validator: myValidatorFunction }
            }), // it maps a validation interface for the input plugin, and error messages
        ])

- usage

        // loading data
        let list = await controller.load() // disponibilized by ApiPlugin

        // access input and rules
        <input 
            type="text" 
            onChange={controller.onChangeFor('name')} 
            value={controller.valueOf('name')} 
            required={controller.rulesOf('name').required}
        >

        // access validation messages
        <div>{ controller.rulesOf('name').msg }</div>


## connecting controller with react


- the controller is injected by a connector, with provides the controller (fixed) and the data (variable) object

        function MyComponent({ controller, data }) { ... }

        export default controller.connect(MyComponent)


## references

[Create an NPM module with TS](https://levelup.gitconnected.com/create-a-npm-module-with-typescript-c99bd0686f69)

