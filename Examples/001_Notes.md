# React Part I

## Installing

- Create a react solution with:

  ```console
  >> npm i -g create-react-app@1.5.2
  ```

## Vs-Code extensions

- Simple React Snippets
- Prettier
- Auto Import ES6, TS, JSX
- Wrap with abreviation Ctrl+Alt+W

## Create a new app

- Execute:

  ```console
  >> create-react-app react-app-001
  ```

- Next run

  ```console
  >> npm run start
  ```

## Virtual DOM

- React creates an specific type of object when we use jsx, that is the ``Virtual DOM``.
- The ``Virtual DOM``, will synchronize in a moment with the real DOM updating only the parts that changes.
  - Thats why we need to manage the state.
- ``Babel`` is the responsible for changing jsx to functions in react.
  
  ```js
  const element = <h1> Hello World </h1>;
  React.createElement("h1","", "Hello World");
  ```

## Rendering a list

- As ``jsx is only sugar syntaxis`` we can't do a for or other control sequence.
- We can use jsx with iterators in order to render.

  ```js
  {Formatter.formatAsCompleteList(this.state.tags, <p>No elements</p>)}

  const Formatter = {
      formatAsList: (content, counter = Number) => <li key={counter}>{content}</li>,

      formatAsCompleteList: function (arr = Array, nullMsg) {
          if (arr.isArray || arr.length === 0) return null || nullMsg;
          return <ul>{arr.map(this.formatAsList)}</ul>;
      },
  };
  ```

## Handle an event

- For handle an event we need to bind correctly ``this`` keyword.
- **First Way: Using binding**

  ```js
  this.handleIncrement = this.handleIncrement.bind(this)
  ```

- **Second way: Defining as arrow function**

  ```js
  handleIncrement= () => {
    console.log('Increment Clicked');
  }
  ```

## Updating the state

- To stablish that we make a change on a compenent and re-render, we need to update the state.

  ```js
  handleIncrement= () => {
          this.setState({count: ++this.state.count})
          console.log('Increment Clicked', this.state.count);
  }
  ```

## Passing arguments to an event

- For passing arguments to an event we use closures or functions that returns customized functions.

  ```jsx
  <button onClick={()=>this.handleIncrement(this.props.firstName) } className='btn btn-secondary btn-sm'> Increment </button>
  handleIncrement(name){
      let {count: prevCount } = this.state;
      prevCount++;
      this.setState({count: prevCount, tags: [name]})
      console.log('Increment Clicked ', this.state.count);
  }
  ```

## Installing Dependencies on React
  
- We use ``npm`` as:

  ```console
  >> npm i bootstrap@4.1.1 font-awesome@4.7.0
  ```

## Importing styles

- On ``index.jx`` add:

  ```js
    import "bootstrap/dist/css/bootstrap.css";
    import "font-awesome/css/font-awesome.css";
  ```

- React put all as a bundle and then display on the web.

## Children

- They are the content that can be passed to a component like Dialog Box or other container.
  1. On compenent:

      ```jsx
        render() {
              return ( 
              <div style={this.styles} className='m-2'>
                  {this.props.children}
                  <h1>Other component</h1>
              </div>
            );}
      ```

  2. On calling:

      ```jsx
        <Counter key={counter.id} value={counter.value}>
            <h1>Counter #{counter.id}</h1> 
        </Counter>
      ```

## Debbuging

- We can add React extension to see the props of an element.
- Calling variables:
  - ``$r``: With this we can get on console what object is selected on the virtual dom
  - ``$0``: With this we can get on console what object is selected on DOM.

## Props vs State

- **Props**
  - Data that we give to that component.
  - There are read only, we cannot change.
- **State**
  - Local data for component, we cannot access.
  - We can storage the props on the state for been changed by the component.

## Rule for component handling

---

> **The component that owns a piece of the state(passing the props), should be the one modifying it.**

- Implement the ``handleForEvent`` on the ``container component``, not on the one affected.

  ```jsx
    handleOnDeleteCounter = (id) => {
      console.log('On Delete Counter called for counter',id)}
      render() { 
          return <Counter key={counter.id}
                       value={counter.value}
                       id = {counter.id} // Pasing the id for retreiving 
                       onDelete={(id)=>this.handleOnDeleteCounter(id)}/> // Pasing the handler
      }
  ```

- On the ``component`` create the handle for receiving ``as props``.

  ```jsx
    <button 
        onClick={()=>this.props.onDelete(this.props.id)}>
         Delete
    </button>
  ```

## Single Source of truth

> A component that change his content by the his container, must eliminate his state in order to be every time synchronized.

- **Controlled component:**
  - A component where all his state and events handlers are defined outside of it.
- **Controller page:**
  - A component with all the stage.

## Stateless Functional Component

- A component without state that is defined on a basic function.

  ``` jsx
    const Navbar = (props) => {
        return ( <nav></nav> );
    }
  ```

- The shorcut for Stateless Functional component.
  
  ```snippet
  sfc
  ```

## Desestructuring props

- To have a cleaner code we can get the props.
  
  ```js
  const {totalNumberOfCounters, totalNumberOfNonZeroCounters, totalCount} = props;
  ```

## LifeCycle
  
- The LifeCycle in react can mainly divide in 3 parts:
  1. **Mount:**

      ```jsx
      constructor()
      componentDidMount()
      ```

  2. **Update:**

      ```jsx
      componentDidUpdate(prevProps,prevState)
      ```

  3. **Unmount:**

      ```jsx
      componentWillUnmount()
      ```

## Lodash

- Lodash is a library for utilities.
- Installing:

    ```console
    >> npm install lodash@4.17.10
    ```

## Verifying types in react

- Installing prop-types:

    ```console
    >> npm i prop-types@15.6.2
    ```

- Example of usage:

    ```jsx
    Pagination.propTypes = {
        itemsCount: PropTypes.number.isRequired,
        pageSize: PropTypes.number.isRequired,
        selectedPage: PropTypes.number.isRequired,
        onPageChanged: PropTypes.func.isRequired,
    };
    LikeIcon.propTypes = {
        isLikeSelected: PropTypes.bool.isRequired,
        onClickLike: PropTypes.func.isRequired,
    };
    ```

- [Documentation 📚](https://es.reactjs.org/docs/typechecking-with-proptypes.html)
- [Video example 📹](https://www.youtube.com/watch?time_continue=5&v=SKqFMYOSy4g&feature=emb_logo)

## Using the component to proccess his own data

- In the ``component`` we update the state values:

    ```js
    const raiseSort = (propertyPath) => {
        // Doing the logic that belongs to the component.
        onSort(updateSortColumn(sortColumn, propertyPath)); // Pass the new state to the caller component
    };
    ```

- On the ``render`` instead of calling the ``props function`` we call the ``closure`` to pass the data to the function:`

  ```js
  return (
  <th
    className="click-enabled"
    key={column._id}
    onClick={() => raiseSort(column.path)}> // Calling raise sort
    {column.headerContent}{" "}
    <SortIcon isAscSort={isAscSort} isSorting={isSorting} />
  </th>
  );
  ```

- On the ``caller`` we use the updated state.

  ```jsx
  // Sorting
  handleOnSort = (sortColumn) => {
      // New state calculated on the component.
      this.setState({ sortColumn });
  };
  ```

## Routing

- Installing

  ```console
  >> npm i react-router-dom@4.3.1
  ````

- **Browser**

  - Importing the Browser element
      1. Browse history objects in browsers.
      2. This element will allow the Route Component work with urls.

          ```js
          import { BrowserRouter } from "react-router-dom";
          ```

  - Using in index.js

      ```js
      ReactDOM.render(
          <BrowserRouter>
              <App />
          </BrowserRouter>,
          document.getElementById("root")
      );
      ```

- **Route**
  - The route will render the component that is passed in prop.
  - The Route matches if contains a part of the specified Route.

      ```js
      <Route path="/products" component={Products}></Route>
      ```

  - If the router don't use exact it will match will all routes.

      ```js
      <Route path="/products" component={Products} />
      <Route path="/" component={Home} />
      ```

  - With exact atributte:

      ```js
      <Route path="/products" component={Products} />
      <Route path="/" exact component={Home} />
      ```

  - With additional props:

      ```js
      <Route
        path="/products/newests"
        render={(props) => (
          <Products sortBy="newest" {...props}></Products>
        )}
      />
      ```

- **Switch**

  - The switch allow to only take one route, the first one in the list.

      ```js
      <Switch>
        <Route path="/products" render={()=><Products sortBy="newest"></Products>}
        <Route path="/posts" component={Posts} />
        <Route path="/admin" component={Dashboard} />
        <Route path="/" component={Home} />
      </Switch>
      ```

- **Link**
  - To use instead of anchors ``<a></a>``, this will prevent recharge the page.
  - The Link will only update the url to let Route show the new route.

    ```js
    <ul>
        <li>
            <Link to="/">Home</Link>
        </li>
    </ul>
    ```

- **Route parameters**
  - We can pass a parameter to the route using ``:param`` sintaxis.

    ```js
    <Route path="/products/:id" component={ProductDetails} />
    ```

  - For reading we use ``this.props.match``:

    ```json
    "match":
    {
        "path": "/products/:id",
        "url": "/products/3",
        "isExact": true,
        "params": {
            "id": "3"
        }
    }
    ```

- **Optional Parameters**
  - For pasing a paramater that can be undefinend we use ``:x?``
  - Url Patterns:
    - ``/posts/2018/06``
    - ``/posts/2018/``
    - ``/posts/``
  - Route declaration for posts url patterns with year and month optional:

    ```jsx
    <Route path="/posts/:year?/:month?" component={Posts} />
    ```

  - Then we can assign a value on the component:

    ```js
    const Posts = ({ match }) => {
    let { year = 2000, month = 1 } = match.params;
    return (
        <div>
            Year: {year}, Month:{month}
        </div>
        );
    };
    ```

- **Query parameters**
  - We can read the get parameters on from the url.
  - Url with get parameters.
    - ``posts/2018/06?sortBy=year&approved=true``
  - For reading a url with get we use ``this.props.location``.

    ```json
    "location":
        {
            "pathname": "/posts/2018/06",
            "search": "?sortBy=year&approved=true",
            "hash": ""
        }
    ```

  - As the search give us a string we can use ``query-string`` for get each paramater as a JSON:

    ```console
    >> npm i query-string@6.1.0
    ```

  - The result is an object with the properties on strings.

    ```json
    {
        "approved": "true",
        "sortBy": "year"
    }
    ```

- **Redirects**
  - For redirecting from an invalid route we can use Redirect.

    ```js
    <Route path="/admin" component={Dashboard} />
    <Route path="/not-found" exact component={Home} />
    <Redirect to="/not-found" />
    ```

  - Another useful if to have various names

    ```js
    <Route path="/products" component={Products} />
    <Redirect from="/productos" to="/products"></Redirect>
    <Redirect from="/substances" to="/products"></Redirect>
    ```

- **Programmatic Navigation**
  - Programmatic Navigation is to have links that are based on the history instead of an specific route.
  - We can find the history on the props when we use Route.

    ```js
    "history":{
        "length": 14,
        "action": "POP",
        "location": {
            "pathname": "/posts/2018/06",
            "search": "?sortBy=year&approved=false",
            "hash": ""
        }
        "block": fn()
        "go": f(n),
        "goBack": f(n),
        "goForward": f(n),
        "listen": f(n),
        "push": f(n),
        "replace": f(n)
    }
    ```

  - **History object functions:**
    - **goBack**
      - When called it goes back at the previous endpoint.

        ```js
        this.props.history.goBack()
        ```

    - **push**
      - It is like a Link that sends you to an specific page.

        ```js
        this.props.history.push("/products");
        ```

    - **replace**
      - It do the same as push but it deletes from history the last url.

        ```js
        this.props.history.replace("/products");
        ```

- **Nested routing**
  > With nested routing you can use a Component contained on an other Route.
  
  - This gives the posibility to render inside a component anoter component dependent of the url.

  - **First routing compenent**

    ```js
    class App extends Component {
        render() {
            // First routing level.
            return (
                <div className="content">
                    <Switch>
                        <Route path="/products" component={Products} />
                        <Route path="/admin" component={Dashboard} />
                    </Switch>
                </div>
            );
        }
    }
    ```

  - **Second routing component (nested)**

    ```js
    const Dashboard = ({ match }) => {
        return (
            <div>
                <h1>Admin Dashboard</h1>
                <SideBar></SideBar>
                <Route path="/admin/users" component={Users}></Route>
                <Route path="/admin/posts" component={Posts}></Route>
            </div>
        );
    };
    ```

  - **Final Component**

    ```js
    const Posts = () => {
        return (
            <Fragment>
                <h1>Posts</h1>
                <p>An example post.</p>
            </Fragment>
        );
    };
    ```

## Forms

- When sending a form we need to use the event that the form provides.

    ```js
    <form onSubmit={(e) => handleOnLogin(e)}>
        ...
    </form>
    ```

- In order to prevent the reloading we use a function provided on the event.

    ```js
    handleOnLogin = (e) => {
        e.preventDefault();
        console.log(e);
        console.log("submited");
    };
    ```

- **Controlled elements**

  > Is a recommend way for building forms using only the props on an element.

  - For notify changes they use handlers.

    ```js
    //Defining an element
    <input
      value={this.state.account.username}
      onChange={(e) => handleOnChangeUsername(e)}
    />
    ```

    ```js
    // Defining handler
    handleOnChangeUsername = (e) => {
        const { account } = this.state;
        account.username = e.target.value;
        this.setState(account);
    };
    ```

- **Refs**
  > They are elements for getting info from a tag, using the virtual DOM.

  ```js
  <input ref={this.username}/>
  ```

  - For declaring we use the constructor.

    ```js
    constructor(props) {
        super(props);
        this.username = React.createRef();
    }
    ```

  - We can read the virtual element on a method.

    ```js
      handleOnLogin = (e) => {
      e.preventDefault();
      const username = this.username.current.value;
    };
    ```

  - ==The refs must be implemented as the last option.==

## Validation with Joi

> Is a library for validation of forms.
  
- Installing

    ```console
    >> npm install joi-browser@13.4
    ```

- For validate we need a validation schema.

    ```js
    schema = {
      username: Joi.string().required(), // A string required for password..
      password: Joi.string().required(), // A string required for password.
    };
    ```

- We can pass the object with the fields to validate.
- The ``abortEarly`` property in false is to validate more than one fields.

    ```js
    const result = Joi.validate(
      {username: "lfza", password:"@Bc123"},
      this.schema, 
      {
        abortEarly: false,
      })
    ```

- **Structure of a validation error in Joi**.

    ```json
    "error": {
        "isJoi": true,
        "name": "ValidationError",
        "details": [
            {
                "message": "\"username\" is not allowed to be empty",
                "path": [
                    "username"
                ],
                "type": "any.empty",
                "context": {
                    "value": "",
                    "invalids": [
                        ""
                    ],
                    "key": "username",
                    "label": "username"
                }
            },
            {
                "message": "\"password\" is not allowed to be empty",
                "path": [
                    "password"
                ],
                "type": "any.empty",
                "context": {
                    "value": "",
                    "invalids": [
                        ""
                    ],
                    "key": "password",
                    "label": "password"
                }
            }
        ],
        "_object": {
            "username": "",
            "password": ""
        }
    },
    "value": {
        "username": "",
        "password": ""
    }
    ```

## Form Base Class

> We can define a ``Base Class`` on React for making ``Forms``, applying inheritance.

- **Base class**:

    ```js
    // Use like abstract class.
    class Form extends Component {
            constructor(props) {
                super(props);
                this.state = {
                    data: {}, // Data to be displayed on the fields of form.
                    errors: {}, // Errors on each field.
                };
                this.schema = {}; // Property for validation rules using Joi.
            }

            validateForm = () => {
                const { state, schema } = this;
                const options = { abortEarly: false }; // Check every field as the schema is defined.
                // Obtain error property from Joi.
                const { error } = Joi.validate(state.data, schema, options);
                const errors = {};
                if (!error) return errors;
                // Read all the errors from the array.
                error.details.forEach((error) => {
                    const { path: fieldName, message: errorMessage } = error;
                    errors[fieldName] = errorMessage;
                });
                return errors;
            };

            validateProperty = (input) => {
                const { name, value } = input;
                // For validate only one property not all the schema.
                const property = { [name]: value };
                const propertySchema = { [name]: this.schema[name] };
                const { error } = Joi.validate(property, propertySchema);
                return error ? error.details[0].message : null;
            };

            handleOnSubmit = (e, url) => {
                e.preventDefault();
                const errors = this.validateForm();
                this.setState({ errors });
                if (!isEmptyObject(errors)) return;

                // Call to the server.
                this.doSubmit();
                this.props.history.push(url);
            };

            handleOnReturn = (event, url) => {
                event.preventDefault();
                this.doReturn();
                this.props.history.push(url);
            };

            doReturn = () => {
                console.log("Return function");
            };

            doSubmit = () => {
                console.log("Submitted");
            };

            handleOnChangeInput = ({ target: input }) => {
                const { errors } = this.state;
                const errorMessage = this.validateProperty(input);
                // Update error according to the field
                if (errorMessage) errors[input.name] = errorMessage;
                // Assign the error.
                else delete errors[input.name]; // Delete the error
                // Update field content
                const { data } = this.state;
                data[input.name] = input.value;
                //Update the state with updated content and errors messages.
                this.setState({ data, errors });
            };

            renderInput({ name, label, type = "text" }) {
                const { data, errors } = this.state;
                return (
                    <Input
                        name={name}
                        value={data[name]
                        label={label}
                        type={type}
                        error={errors[name]}
                        OnChangeInput={(e) => this.handleOnChangeInput(e)}
                    />
                );
            }

            renderSelect({ name, label }, options = []) {
                const { data, errors } = this.state;
                return (
                    <Select
                        name={name}
                        value={data[name]}
                        label={label}
                        options={options}
                        error={errors[name]}
                        OnChangeInput={(e) => this.handleOnChangeInput(e)}
                    />
                );
            }

            renderReturnButton(url) {
                return (
                    <button
                        className="btn btn-success m-2"
                        onClick={(e) => this.handleOnReturn(e, url)}
                    >
                        Return
                    </button>
                );
            }

            renderSubmitButton(label) {
                return (
                    <SubmitButton
                        label={label}
                        disabled={!isEmptyObject(this.validateForm())}
                    ></SubmitButton>
                );
            }
    }
        export default Form;
    ```

- **Form implementation (Login Form)**:

    ```jsx
    class LoginForm extends Form {
        constructor(props) {
            super(props);
            this.componentsProps = {
                username: {
                    name: "username",
                    label: "Username",
                },
                password: {
                    name: "password",
                    label: "Password",
                    type: "password",
                },
            };
            const { username, password } = this.componentsProps;
            this.schema = {
                [username.name]: Joi.string().required().label(username.label),
                [password.name]: Joi.string().required().label(password.label),
            };
            this.state = {
                data: {
                    [username.name]: "mosh",
                    [password.name]: "myS3cretPasSw0r0d",
                },
                errors: {},
            };
        }

        doSubmit = () => {
            console.log("Login form submitted.");
        };

        render() {
            const { handleOnSubmit, componentsProps } = this;
            const { username, password } = componentsProps;
            return (
                <form onSubmit={(e) => handleOnSubmit(e)}>
                    {this.renderInput(username)}
                    {this.renderInput(password)}
                    {this.renderSubmitButton("Login")}
                </form>
            );
        }
    }

    export default LoginForm;
    ```
