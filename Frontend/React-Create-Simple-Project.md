## React Project Setup Guide

This manual provides a step-by-step guide for setting up a React project using Vite, including essential configurations and library installations. It covers creating a new project, installing dependencies, configuring ESLint, setting up Axios for API requests, incorporating Styled Components for styling, and implementing React Router for navigation. Each section includes detailed instructions and code snippets to ensure a smooth setup process for a React application.

1. ### Create a new Vite project:
    ```shell
    npm create vite@latest
    ```

2. ### Install project dependencies:
    ```shell
    npm i
    ```
3. ### Add a lint rule to disable prop-types checks in `.eslint.cjs`:
    - Locate the `rules` section and add `"react/prop-types": "off"` as the second rule.
    The section should look like this:
        ```javascript
        rules: {
            "react/react-in-jsx-scope": "off",
            "react/prop-types": "off",
        },
        ```
      
4. ### Install Axios for API handling:
    ```shell
    npm i axios
    ```
    - Create a `common` folder inside the `src` directory.
    - Inside `common`, create `api.js` with the following content:
        ```jsx
        import axios from "axios";

        export const api = axios.create({
            baseURL: "https://devjokes.com/api",
        });
        ```

5. ### Install Styled Components for styling:
    ```shell
    npm i styled-components
    ```
    - Create a `styles` folder inside the `src` directory.
    - Inside `styles`, create `globalStyles.js`.
   

6. ### Install React Router for routing:
    ```shell
    npm i react-router-dom
    ```
    - Create a `routes` folder inside the `src` directory.
    - Inside `routes`, create `index.jsx` with the following content:
        ```jsx
        import {BrowserRouter, Routes, Route} from "react-router-dom";

        export default function Router() {
            return (
                <BrowserRouter>
                    <Routes>
                        <Route path="*" element={<h1>nope.. 404</h1>}/>
                        <Route path="/" element={<h1>Welcome Home</h1>}/>
                    </Routes>
                </BrowserRouter>
            );
        }
        ```
    - In `App.jsx`, use the `Router` component:
        ```jsx
        import Router from './routes';

        function App() {
            return (
                <>
                    <Router />
                </>
            );
        }

        export default App;
        ```