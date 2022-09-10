Let's create your first Typescript mod in NanosWorld.

# Prerequisites
You need to have [NodeJS](https://nodejs.org/en/) >= 16 installed on your computer

# Installing NanosTS
First, you will need to install nanosts globally, install via NPM with this command:
```sh
npm install -g nanosts
```

NanosTS is a CLI that will help create your project but also automatically update your projects types when NanosWorld API updates.

Verify your install with this command:
```sh
nanosts -v
```

It should output something similar to this (depending on your OS and latest version)
```sh
nanosts/1.1.0 linux-x64 node-v16.17.0
```

# Creating your first project
Navigate to the `Packages` folder of your NanosWorld server and lets create the package !

Type this command:
```sh
nanosts project my-first-script
```

Next, enter your name:
```sh
➜  tests nanosts project my-first-script
? enter name of author DKFN
```

Then the version (or Enter if you want to skip)

Select "script" when asked for a type

When asked for script folder, press "a" to select all folders then press Enter

Finally, type Y to enable Lazy Loading, this will make it easier to test the changes we make to the sources files (learn more about Lazy Loading here)

Wait some minutes, NanosTS will generate the type declarations for the latest NanosWorld API version, creating base files for your project and installing mandatory dependencies

Finally, if you go into the folder you should have a simillar file structure:
```sh
```