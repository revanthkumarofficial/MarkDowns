# ANGULAR

##### ANGULAR
Angular is a typescript based client side framework to create dynamic web applications


##### ANGULAR 8 FEATURES
Differential Loading to create two different production bundles of your app.
New Dynamic Lazy Loading modules for imports.
Supports Web Workers.
Supports TypeScript version 3.4.
Availability of New Workspace APIs and Builder.
Bazel Support.
Opt-In usage sharing.
Ivy Rendering Engine: The new compiler for Angular version 8.
ngUpgrade improvements.

##### ANGULAR UPDATION PROCESS
npm uninstall -g angular-cli // For Windows Open Powershell on Administrator Mode    
npm cache verify   
npm cache clean    
npm install -g @angular/cli@latest 
ng --version 

##### IVY
Ivy in Angular version 8 is considered as the Rendering Engine. It was released in Angular 8 as Opt-in. 
It has opted as the code name for Angular’s next-generation rendering pipeline and compilation. By default, Ivy is intended to be the rendering engine in Angular 9

##### BAZEL
Bazel is one of the key features present in Angular version 8. It always allows you to build CLI applications quickly. 
Bazel is considered a built tool that is developed and mostly used by Google as it can build applications in any language. 
The entire Angular framework is built with Bazel. Moreover, Bazel allows you to break an application into different build units which are defined at the NgModule level.
It provides you the possibility to make both frontends and backends with the same tool.
Availability of incremental build and test options.
In Bazel, you have the possibility for cache and remote builds on the build farm.

##### CODELYZER
Codelyzer in Angular version 8 is the open-source tool that is present on the top of the TSLint. 
The main purpose of Codelyzer is to verify whether the Angular TypeScript 3.4 projects are following the set of linting rules or not. 
It mainly focuses on the static code in Angular TypeScript. 
In simple words, we can say that the main purpose of the Codelyzer in Angular version 8 is to check the quality and correctness of the program.


##### WILD CARD ROUTE
The main purpose of the Wildcard Router in Angular version 8 is to match every URL as an instruction to get a clear client-generated view. 
This Wildcard route always comes last as it needs to perform its task at the end only. It is mainly used to define the route of the pages in Angular 8.

##### LIMITATIONS OF WEB WORKERS
The web workers in Angular version 8 are mainly used to speed up the work of the things in your application during the working of CPU. 
These web workers mainly allow you to run the CPU computations in a background thread. This, in turn, helps the main thread to free up and then update the user interface.

##### ANGULAR UNIVERSAL
The Angular Universal is defined as the process of SSR (server-side rendering) of your particular application to HTML present on the Server. 
Basically, all the Angular applications are single-page applications so the rendering always occurs on the browser. 
The entire process of rendering single-page applications is known as the client-side rendering process (CSR).

##### OBSERVABLES
Angular makes use of observables as an interface to handle a variety of common asynchronous operations. For example:

You can define custom events that send observable output data from a child to a parent component.
The HTTP module uses observables to handle AJAX requests and responses.
The Router and Forms modules use observables to listen for and respond to user-input events.

##### COMMUNICATION BETWEEN COMPONENTS
PARENT TO CHILD :   @Input() decorator.
CHILD TO PARENT :   @Output() open = new EventEmitter<any>();
                    @Output() close = new EventEmitter<any>();
