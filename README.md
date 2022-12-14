# Setting-Up-Environment-for-Nodejs-application

### CREATING ENVIRONMENT VARIABLES For NODE JS
---

Two possible packages to use : 
* env-cmd
* dotenv (preferred)

In our case , we will use the dot env: 

### `case 1` :  When you have only single environment 

* create .env file in the root folder 

`Note` : There should not be any space between variable **name** , **=** , **value**
```javascript
// env file example 
PORT=3000 
```
In the main file where your application starts include the following : 

```javascript
require('dotenv').config();
/// Now you can access all the variables writte in the env file using process.env.VARIABLE_NAME
PORT = process.env.PORT;
app.listen(PORT,()=>console.log(`Listening on port ${PORT}`))
```
Next in the package.json file you need to change few things in the script

*assuming index.js is my file name to start my server*
```json
// if your index.js file exists in the root folder
scripts : {
    "start" : "node -r dotenv/config index.js" 
}

// if your index.js file exists in the src folder
scripts : {
    "start" : "node -r dotenv/config src/index.js" 
}

// -r means require
```

### `case 2` :  When you have only Multiple environments

In my case i have two environments 

* dev 
* prod

Here we will be using the benefit of NODE_ENV which is a part of process.env existing within the express server 

To use process.env.NODE_ENV we have to first set our environment using : 

```bash
set NODE_ENV=dev or set NODE_ENV=prod # in windows
export NODE_ENV=prod or export NODE_ENV=prod # in mac OS
```

Now since only one script can be run at  a time we need to take the help of sequential execution of commands using `&&` 

`&&` is used to run multiple commands sequentially 
`&` is used to run multiple commands parallely

`In package.json`

```javascript
"scripts": {
    "start": "node src/index.js",
    "build":"set NODE_ENV=prod&& node -r dotenv/config src/index.js",
    "dev": "set NODE_ENV=dev&& node -r dotenv/config src/index.js"
  },

```

Now create two files in the root folder 

1. prod.env
2. dev.env

Since we have not used the default .env format we need to tell config the path of our env files 

Earlier use only used 

```javascript
 require('dotenv').config();
```

In this case we have to provide the path to our env files in the root folder.

```javascript 
 require('dotenv').config({ path: path.resolve(__dirname, `../${process.env.NODE_ENV}.env`) });
```

**NOTE: you need to require dotenv in every file where you use the process.env variables as in second case we are providing the absolute path to env files and each file may not be in the same folder but in Case 1 : you need to only require('dotenv') in the first file only where your server starts i.e index.js in my case**
