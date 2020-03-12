# Install Express Generator

You need to install Express Generator as a global NPM package so you can run it from anywhere on your computer. This installation is pretty straightforward, and you should be familiarized with this process:
```
$ npm install express-generator -g
```
## Quick Start

The quickest way to get started with express is to utilize the executable express which will generate an application as shown below:
```
$ express --view=hbs

or

$ express --hbs
```
So let’s now start using this command and let’s create our first project.

We will name our project library-project, so we will run the following line:
```
$  express --hbs library-project
```
After navigating to the project we just created, we can run our application.
```
$ cd library-project
```
First we should install the packages using:
```
npm install
```
And install nodemon:
```
npm install nodemon
```
We need to install mongoose, serve-favicon, body-parser y dotenv:

```
npm install mongoose serve-favicon body-parser dotenv
```

We need to create the models folder:

```
mkdir models
```

We need to add this line to the scripts:
```
"dev": "nodemon ./bin/www"
```
For dev (development) mode (using nodemon), we should run the following command:
```
$ npm run dev
```
This is the command we will keep using and later on, when we start deploying our projects, we will want to run it in prod (production) mode, and then we can use this command:
```
$ npm start
```
We can navigate to http://localhost:3000 