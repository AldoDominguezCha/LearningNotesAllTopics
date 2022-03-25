# React part II

## Building a Backend
  
- We can use a false backend like JSONPlaceholder.
- Also we can use json-server to have our local server based on a json file.

## Axios
  
- For making the http request we can use Axios library.
- Axios gives us the methods to perfom different request.
- **Http Get**
  - For http get we can use axios.get:

    ```js
    const { data: posts } = await axios.get(this.apiEndpoint);
    ```

  - On data we will receive the data from the response in case of no error.
- **Http Post**
  - On Post we pass the apiEndpoint and the data obj to post.

    ``` js
    const { data: post } = await axios.post(this.apiEndpoint, obj)
    ```

  - We receive on data the response.
- **Http Put**
  - The put is used for a total update, so we can specify it.

    ```js
    handleUpdate = async (post) => {
        const posts = this.state.posts;
        const index = this.state.posts.indexOf(post);
        post.title = { "title": "updated post."};
        await axios.put(`${this.apiEndpoint}/${post.id}`, post);
        posts[index] = post;
        this.setState({ posts });
    };
    ```

  - We can pass the id of the resource on the request.
- **Http Path**
  - The put is only used for update some fields.

    ```js
      handleUpdate = async (post) => {
        post.title = "Updated post";
        await axios.patch(`${this.apiEndpoint}/${post.id}`, { title: post.title });
        const posts = this.state.posts;
        const index = this.state.posts.indexOf(post);
        posts[index] = post;
        this.setState({ posts });
      };
    ```

- **Http Delete**
  - The delete method is used for deleting a resource and typically we not pass a body.
  
    ```js  
      await axios.delete(`${this.apiEndpoint}/${post.id}`);
    ```

## Optimistic vs Pessimistic Update
  
- **Pessimistic Update**
  - We only show the real result from the calling to the backend.
- **Optimistic Update**
  - We update the UI as it is expected and then we make the changes and revert them if something bad pass.
  - **Example:**

    ```js  
    handleDelete = async (post) => {
        const orginalPosts = this.state.posts;

        const posts = this.state.posts.filter((p) => p.id !== post.id);
        this.setState({ posts });

        try {
          await axios.delete(`${this.apiEndpoint}/${post.id}`);
        } catch (ex) {
          if (ex.response && ex.response.status === 404)
            alert("This post has already been deleted.");
          this.setState({ posts: orginalPosts });
        }
      };
    ```

## Handling Unexpected Errors Globaly
  
- For handling errors we can use interceptors that is a function that will be executed before getting a response from the server.

- On axios we declare as:

  ```js
    axios.interceptors.response.use(null, (error) => {
      const isExpectedError =
        error.response &&
        error.response.status >= 400 &&
        error.response.status < 500;

      if (!isExpectedError) {
        // Log on unexpected error.
        console.log(`Loggging the error`, error);
      }

      return Promise.reject(error);
    });
  ```

## Service
  
- With a service we can call an specific library without be totally dependent of it.
- In case of axios we can make an http service and export as a library.

  ```js
    export default {
      get: axios.get,
      post: axios.post,
      put: axios.put,
      patch: axios.patch,
      delete: axios.delete,
    };
  ````
  
- Also we can define the interceptor.

  ```js
  axios.interceptors.response.use(null, (error) => {
      const isExpectedError =
        error.response &&
        error.response.status >= 400 &&
        error.response.status < 500;

      if (!isExpectedError) {
        // Log on unexpected error.
        console.log(`Loggging the error`, error);
      }

      return Promise.reject(error);
    });
  ```

## Config file
  
- We can use json files as config as constants.
- For use them we create a file fileName.json with the information:

  ```json
    {
      "apiEndpoint": "http://192.168.3.6:3001/posts"
    }
  ```

- Then we can import as config.
  
  ```js
    import config from "./config.json";
  ```

- For reading the properties we use config.propertyName  

  ```js
    const { data: posts } = await http.get(config.apiEndpoint);
  ```

## Using Toast Messages
  
- For Alerts we can use **react-toastify**.
- Installing:
  
  ```batch
    npm i react-toastify@4.1
  ```

- Importing Container
  
  ```js
    import { ToastContainer } from "react-toastify";
    import "react-toastify/dist/ReactToastify.css";
  ```

- On render we add the ``ToastContainer``
  
  ```js
    render(){
      return (
        <ToastContainer/>
      )
    }
  ```

- To display the ``alert(ToastContainer)`` we use ``toast``.

  ```js
    if (ex.response && ex.response.status === 404)
      toast.warn("This post has already been deleted.");
    else
      toast.error("Unexpected error.");
  ```

## Using a logging service
  
- We can use sentry.io
  - [Documentation 📚](https://sentry.io/onboarding/upiita/get-started/)
- Installing

  ```batch
    npm install --save @sentry/react @sentry/tracing
  ```

- Import and configure on ``index.js``

  ```js
    import * as Sentry from "@sentry/react";
    import { BrowserTracing } from "@sentry/tracing";

    Sentry.init({
      dsn: "https://f1af9aef541e4de6b9cae1280bad7a37@o1163449.ingest.sentry.io/6251560",
      integrations: [new BrowserTracing()],

      // Set tracesSampleRate to 1.0 to capture 100%
      // of transactions for performance monitoring.
      // We recommend adjusting this value in production
      tracesSampleRate: 1.0,
    });
  ```

- Sending an error

  ```js
    Sentry.captureException(error);
  ```

## Mongo DB
  
- For backend we can use a db like MongoDb
- Installing:
  1. [Download📥 Mongo DB Community server](https://www.mongodb.com/try/download/community)
  2. Search Mongo db daemon in: ``C:\Program Files\MongoDB\Server\50\bin\mongod.exe``
  3. Add the path as an enviroment variable.
  4. Run mongod on the console.

      ```batch
        mongod
      ```

  5. Click on new connection to add a Mongo db instance.
