# Project Folder Structure


Project folder structure:

````
├── bin
├── init
│   ├── ServerStart
│   └── WorkerStart
├── public
├── resource
│   ├── cache
│   ├── kvstore
│   ├── routing
│   ├── config
│   │   ├── qatest
│   │   │   ├── connection
│   │   │   └── ...
│   │   ├── online
│   │   │   ├── connection
│   │   │   └── ...
│   │   ├── share
│   │   │   └── table
│   │   └── test
│   │       ├── connection
│   │       └── ...
│   └── sql
│       ├── common
│       └── ...
├── src
│   ├── Controller(http server)
│   ├── Bo
│   ├── Dao
│   └── Service
├── tests
│   ├── Bo
│   ├── Dao
│   └── Service
└── vendor
    └── ...
````

### 目录说明
* bin: start service files
* init: init application files
* resource: config files folder, more information: [project config](web/config.md)
* src: logic demos folder
* tests: UT (Unit Test) folder
* vendor: composer package folder
