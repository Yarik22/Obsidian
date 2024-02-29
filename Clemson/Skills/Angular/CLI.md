## Installing Angular CLI
Install the CLI using the `npm` package manager:
```node
npm install -g @angular/cli
```
## Basic workflow
```node
ng --help
ng new --help
ng new my-first-project
cd my-first-project
ng serve
```
```node
[ng (b)build] Compiles an Angular application into an output directory.
[ng (s)serve] Builds and serves your application, rebuilding on file changes.
[ng (g)generate] Generates or modifies files based on a schematic.
[ng (t)test] Runs unit tests on a given project.
[ng (e)e2e] Builds and serves an Angular application, then runs end-to-end tests.
```
You also can generate entities such as
[[Server side rendering]]
```node
ng generate app-shell 
```
Other
```node
ng generate (app)application [name]
ng generate (cl)class [name]
ng generate (c)component [name]
ng generate config [type]
ng generate (d)directive [name]
ng generate (e)enum [name]
ng generate environments
ng generate (g)guard [name]
ng generate interceptor [name]
ng generate (i)interface [name] [type]
ng generate (lib)library [name]
ng generate (m)module [name]
ng generate (p)pipe [name]
ng generate (r)resolver [name]
ng generate (s)service [name]
ng generate web-worker [name]
```
[More details for arguments etc.](https://angular.io/cli/generate)