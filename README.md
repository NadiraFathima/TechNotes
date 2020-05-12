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
