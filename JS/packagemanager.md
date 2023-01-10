# Package Manager

## npm vs npx

npm - `Manages packages but doesn't execute` any.   
npx - A tool for `executing Node packages`.

### npm
NPM by itself does not simply run any package. If you want to run a package using NPM, you must specify that package in your package.json file. When executables are installed via NPM packages, NPM simply links to them.

### npx
npx will check whether `<command>` exists in $PATH, or in the local project binaries, and execute it. So, for the above example, if you wish to execute the locally-installed package some-package all you need to do is type: `npx some-package`

Another major advantage of npx is the ability to execute a package which wasn't previously installed: `$ npx create-react-app my-app`
The above example will generate a react app boilerplate within the path the command had run in.

### Example
Let’s compare the process for creating a React app without npx and with npx.

Without npx -   
```console
npm install create-react-app
node ./node_modules/create-react-app/index.js myreactapp
```

With npx -
```console
npx create-react-app myreactapp
```

Without an additional tool for invoking these executables, developers would need to dig through their project’s node_modules directory to find the right file.


### SUMMARY

NPM is a package manager, you can install node.js packages using it. 

NPX is a tool to execute node.js packages.
It doesn't matter whether you installed that package globally or locally. NPX will temporarily install it and run it.

NPM also can run packages if you configure a package.json file and include it in the script section.

