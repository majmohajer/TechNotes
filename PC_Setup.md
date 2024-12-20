## PC Setup Instructions

- Install Git by downloading windows installer  
- Install VS Code  
- Install Visual Studio
- Install Node
- Install Angular CLI
- Add Angular DevTools Chrome Extension
- Install Red gate Sql Search   

  
### Install Node & Angular CLI
https://angular.dev/tools/cli/setup-local

Download Node.js installer https://nodejs.org/en/download/prebuilt-installer

>   npm install -g @angular/cli
> or for a specific version  
>   npm install -g @angular/cli@wished.version.here

If already installed, then:
npm uninstall -g @angular/cli     
npm cache verify   

On Windows client computers, the execution of PowerShell scripts is disabled by default, so the above command may fail with an error. To allow the execution of PowerShell scripts, which is needed for npm global binaries, you must set the following execution policy.    
-Open Powershell as administrator and run:  
    Set-ExecutionPolicy -ExecutionPolicy Unrestricted  

### Markdown 
- Configure your machine to open md files in Chrome
- Add Markdown Viewer Chrome Extension  
- Go to extension settings and enable Allow access to file URLs 

### Install VS Code

-Install the TypeScript compiler:      npm install -g typescript and check version tsc --version
- Add a tsconfig.json file to define the TypeScript project settings

.NET Core runtimes or SDKs:
dotnet --info   cmd tells you what you have on your machine. Install latest version of .net core sdk


### Configuration Items

Windows 10 Mouse Settings > Advance >  Pointer Options > Remove check for hid pointer while typing

Sql Managment Studio has a glitch refreshing windows
https://superuser.com/questions/1319376/sql-server-management-studio-graphical-glitches
