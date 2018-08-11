Sample use of mocker.

Mocker consists of:
* A UI for management
* A mock server which responds to requests based on rules.
* A learning mode reverse proxy server. In learning mode requests to the real server that the mock server should mock are captured. From the UI these captured requests can be transformed in rules for the mock server. 

Initialize a new project
```shell
npm init
```

Install [@kroonprins/mocker](https://github.com/kroonprins/mocker) as a dependency
```shell
npm install @kroonprins/mocker
```

Add shortcuts in package.json:
```json
  "scripts": {
    "start": "mocker-ui",
    "mock-server": "mocker-mock-server",
    "learning-mode": "mocker-learning-mode"
  }
```
note: on Windows the shortcuts need to be defined as follows instead:
```json
  "scripts": {
    "start": "node --experimental-modules .\node_modules\@kroonprins\mocker\mocker-ui.mjs",
    "mock-server": "node --experimental-modules .\node_modules\@kroonprins\mocker\mocker-mock-server.mjs",
    "learning-mode": "node --experimental-modules .\node_modules\@kroonprins\mocker\mocker-learning-mode.mjs"
  }
```

Add a .env file in the root of the project and add following variables:
* MOCKER_PROJECTS_FILE: location of the yaml file that will keep the list of projects.
* MOCKER_LEARNING_MODE_DB_LOCATION (optional): if you want to use the learning mode of the server, then add here the name of the file in which the captured requests should be stored.
* MOCKER_PROJECT (optional): if you want to start the mock server or the learning mode from the command line, then this variable should specify the name of the project for which the mock server should run.
* TEMPLATING_HELPERS_NUNJUCKS (optional): if you want to define additional helper functions/filters for the nunjucks templating engine, then this variable should contain the path to the file containing them. See template-helpers.nunjucks.mjs for an example on how to define additional helper functions/filters for the nunjucks templating engine.
* MOCKER_RULES_DEFAULT_LOCATION (optional): set the default location in which the yaml files of the rules will be stored.
* MOCKER_ADMINISTRATION_SERVER_PORT (optional): override the default port (3001) used by the administration server.
* MOCKER_ADMINISTRATION_SERVER_BIND_ADDRESS (optional): bind the administration server to a given address instead of localhost.
* MOCKER_API_SERVER_PORT (optional): override the default port (3004) used by the api server.
* MOCKER_API_SERVER_BIND_ADDRESS (optional): bind the api server to a given address instead of localhost.
* MOCKER_UI_SERVER_PORT (optional): override the default port (3005) used by the UI server.
* MOCKER_UI_SERVER_BIND_ADDRESS (optional): bind the UI server to a given address instead of localhost.
* MOCKER_UI_SERVER_STATICS_LOCATION (optional): override the location where the UI server statics are located.
* MOCKER_MOCK_SERVER_PORT (optional): override the default port (3000) used by the mock server when run from the command line.
* MOCKER_MOCK_SERVER_BIND_ADDRESS (optional): bind the mock server to a given address instead of localhost when run from the command line.
* MOCKER_MOCK_SERVER_WATCH_FOR_FILE_CHANGES (optional): if set to true, a running mock server will watch the filesystem for changes to project configuration, and automatically restart itself when this happen to load the latest configuration. 
* MOCKER_LEARNING_MODE_REVERSE_PROXY_SERVER_PORT (optional): override the default port (3002) for the learning mode reverse proxy server when run from the command line.
* MOCKER_LEARNING_MODE_REVERSE_PROXY_SERVER_BIND_ADDRESS (optional): bind the learning mode reverse proxy server to a given address instead of localhost when run from the command line.
* MOCKER_LEARNING_MODE_REVERSE_PROXY_SERVER_TARGET_HOST (optional): when the learning mode reverse proxy server is started from the command line then this variable should be set to indicate the downstream server to which the reverse proxy should proxy the requests.
* MOCKER_LOG_LEVEL: set the log level (one of 'error', 'warn', 'info', 'debug', or 'trace')

To run the UI:
```shell
npm start
```
From the UI projects can be defined, rules added to the projects, mock servers and learning mode can be started, the captured requests from learning mode can be handled.

To start the mock server for a project from the command line instead of the UI: 
```shell
npm run mock-server
```
In this case the variable MOCKER_PROJECT should be defined in the .env file specifying the name of the project for which the mock server should be started.

To start the learning mode reverse proxy server for a project from the command line instead of the UI:
```shell
npm run learning-mode
```
In this case the variable MOCKER_PROJECT should be defined in the .env file specifying the name of the project for which the learning mode reverse proxy server should be started. In addition the variable MOCKER_LEARNING_MODE_REVERSE_PROXY_SERVER_TARGET_HOST should be defined indicating the server to which the requests should be proxied.