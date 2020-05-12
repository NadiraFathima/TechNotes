# TechNotes
my tech notes

###### Angular
NPM - Package manager

NPX - Package runner


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
