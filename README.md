# TechNotes
my tech notes


# NPM - Node Package manager
> used to install all packages stored in npm registry as dependencies

# NPX - Node Package runner
 > runs CLI tools and other executables hosted on the registry. 
 
For example, all this while packages like grunt we were installing it on global level using something like 'npm install -g grunt'. This actually downloads grunt CLI tool in global npm folder, for me it is ../AppData/Roaming/npm,  and then using the command like 'grunt build' to build applications. 'grunt build' works because the global npm path is mentioned in our environmental variables.

This process is a lot to do, mainly in cases when you want to test out a cli tool for once in your application but have to corrupt the global npm space. NPX solves this issue.  For example,
>'npx lerna bootstrap'.

It first checks if lerna is present in our local npm path(./node_modules/.bin of project) and then the global(appData/Roaming/npm) npm repo and then the registry to see if it can find lerna cli tool. It gets that tool and executes the command 'lerna bootstrap'. 
NPM is also a cli tool and hence 
>'npx npm install'

would work fine. If you want to see how one command works with different node module versions, you can do 
>'npx -p node@8 npm install'

also. This way you can see how your commands work for different node versions before bumping up the node version.


# Angular CD cycle steps for a component:

1.Update bindings
2.Run NgOnChanges, Oninit, DoCheck
3.Renders its dom
4.Change detection for children
5.AfterViewChecked and afterContentChecked
If any event or any trigger point of CD happens in any component, CD process starts from the root. 

If CDStrategy.onpush then in case of any dom event trigger in its child components or input() changes to the component, then cd is triggered in all these component from the root. Example :

                                             1

                                         2       3

                                      4    5  6   7

If an event is triggered in 4, then CD starts from 1, goes to 2 and reaches 4. This happens even if CD strategy of 2 and 4 is onPush. Here, the CD doesn't go from 1 to 3 if CD strategy of 3 is onPush because 4(where event is triggered) is not in the child tree of 3. Else, it goes. 

# Caching - HTTP Status

Scenario 1 : 304 - Not Modified - This status in the browser means that the response is not actually coming from the server but from browser cache stored on our machine. 
For example, for static files like js or any other assets, we need not get the data again and again from server as it doesnt usually change. So when the browser first sends a request to the server, the server attaches an eTag(hash of the content) in the response headers. For all the next requests the browser sends to the server requesting for information, the browser puts the eTag value in "If-None-Match" request header. When the server receives this request it checks if the eTag in the request and the eTag it had is the same. If yes, the server doesn't transfer any data and sends back 304 not modified to the browser. On seeing this status code, the browser takes the data from cache. 

Scenario 2 : 200 Cache - In cases, when server initially responds with response headers like 'expires' or 'max-age'(this is given more priority over expires) this check doesnt happen. Instead, the browser directly serves data from cache until data expired.

HTTP header: 'Cache-Control' = no-cache follows the behaviour of scenario 1.

For more detailed explanation with pictures, refer :
https://medium.com/@codebyamir/a-web-developers-guide-to-browser-caching-cc41f3b73e7c

# Schematics
https://www.youtube.com/watch?v=JAt1FSwhnWk
https://medium.com/@tomastrajan/total-guide-to-custom-angular-schematics-5c50cf90cdb4

# Yarn and npm difference
Yarn and NPM are both package managers. Package manager is basically something that deals with the hassle of installing, updating modifying the dependenices etc. Yarn and npm commands that we type on our local machines are npm clients. By this I mean they are client side programs that do all this. However npm is not just this. NPM also has an npm registry where we can get all the packages. Yarn also gets the packages from npm registry. and follows the same structure as npm like taking dependencies from package.json. Yarn has 'yarn.lock' to lock dependency versions whereas npm does 'package-lock.json'. 
Yarn was designed by facebook to make up for some of npm shortcomings.

https://www.keycdn.com/blog/npm-vs-yarn

# lerna 
Lerna gives us one command to run multiple projects present as packages under same repository. For example under package.json of each project in the repository, you have a script { "start" : "ng serve" }, then running `lerna run start` from the root folder would run ng serve command in all the projects in the repository. 

`lerna bootstrap` command :
 helps us with linking the dependencies so that we can easily include them in require() without any issues. For example, if package 1 has a dependency listed in package.json to package2 :
// packages/pkg-1/package.json
{
  name: "pkg-1",
  version: "1.0.0",
  dependencies: {
    "pkg-2": "github:example-user/pkg-2#v1.0.0"
  }
}

Organizations usually have different projects in different repositories interacting with each other. To see the interaction on local machine, you might have to start and run all these projects simultaneously. This would mean going to each repository typing start command and running it making it a tedious task.  To solve this problem, many companies follow multi-package repository where each package is its own project. Lerna helps us here in optimizing this workflow of the build.
https://lerna.js.org/
https://github.com/lerna/lerna
