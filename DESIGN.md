# MILESTONE: DESIGN

### Problem Statement:  
   Code written by developers could be riddled with bugs like null pointer exceptions, vulnerabilities, duplicated code, spaghetti code and deviations from coding standards. These issues cause headaches when the application is in production and require additional effort to rectify. Early and efficient detection of these problems is a growing need of any software development project.

   In real world software projects, code-bases are very large and consists of source code in various languages. Analysing large amount of code for issues manually is tedious and inefficient. Also each programming language could require different static analysis tools which could be cumbersome to setup and configure on every developer's environment. Managers or lead developers may also want to assess the developer’s code as part of the code review process. A tool that simplifies these tasks would improve the productivity of the team and the time saved here could potentially be used to focus on requirement specifications, application design and/or customer interaction.


### Bot Description:  
   The proposed solution to the above problem is an interactive bot that takes in source code in various formats from the user and returns the result of the static code analysis performed on it. 
It is tedious for a user to check for code errors(/redundancies) that a compiler cannot detect. Static analysis tools used by the bot helps the user in detecting: null-pointer dereferences, memory leaks, logical errors, duplicated code, security vulnerabilities like SQL injections and hard-coded passwords and write error-proof clean code.

   The bot aims at providing a smooth user-experience by abstracting the computation of the analysis of code to an external server below a familiar user interface and conversational model. It does not require any extensive installation by the user and has a simple chat interface which takes in a source file(s) as an input for analysis. It can also interface with a code repository such as Git, in order to analyze remote source files. The bot then gives the user suggestions as per the best coding practice guidelines. The bot’s backend utilizes APIs exposed by various code analysis web services which can be either local or remote services. 

   The design of the bot is based on a few assumptions. The user needs to have the necessary licenses to the external web services. Also, the user has performed the necessary configuration steps to connect to these services.


### Use Cases:  
```
Use Case: Perform code analysis on a source file
1 Preconditions
   The user must have access to slack
2 Main Flow
   User will log on to Slack and access the bot chat window and request code analysis by submitting a source file to the bot [S1]. The bot will return the result of the analysis performed on the source file [S2]
3 Subflows
   [S1] User uploads a source file in the slack chat window
   [S2] The bot returns the result of the analysis back to the user on the chat window
4 Alternative Flows
   [E1] The language of the code in the input source file is not supported

```
```
Use Case: Perform code analysis on source code from a repository 
1 Preconditions
   The user must have access to slack
2 Main Flow
   The User will log on to Slack and access the bot chat window and request code analysis by submitting a link to the source file in a repository [S1] .The bot will return the result of the analysis performed on the source file [S2]
3 Subflows
   [S1] User sends a link to the source file in a repository
   [S2] The bot returns the result of the analysis back to the user on the chat window
4 Alternative Flows
   [E1]The language of the code in the input source file is not supported

```
```
Use Case: Perform code analysis on multiple files 
1 Preconditions
   The user must have access to the slack platform
2 Main Flow
   The user will access the bot via slack and request code analysis by submitting a zip archive containing several source files, possibly, in several languages, to the bot[S1]. The bot will return the result of the  analysis performed on each source file [S2]
3 Subflows
   [S1] The user sends an archive of source files for analysis
   [S2] The bot returns the result of the analysis back to the user on the chat window
4 Alternative Flows
   [E1] The zip archive is corrupt
   [E2] One or more files in the zip archive contain unsupported languages

```


### Design Sketches:  
##### Wireframe:  

![img](https://github.ncsu.edu/rshah8/Design-Milestone/raw/master/frame-git.png)

![img](https://github.ncsu.edu/rshah8/Design-Milestone/raw/master/frame-wire.png)

##### Storyboard:   

![img](https://github.ncsu.edu/rshah8/Design-Milestone/raw/master/story.png)


### Architecture Design:   

The overall architecture of the application comprises of characteristics from message-driven, client-server and layered architecture patterns. The platform, middleware and the external services are described below.

![img](https://github.ncsu.edu/rshah8/Design-Milestone/raw/master/asfdds.png)  

##### Platform 
   As per the architectural specification diagram, the bot is accessible from the Slack platform which serves as the client. All the user inputs in Slack are sent to the middleware via the  RealTime Messaging (RTM) API.

![img](https://github.ncsu.edu/rshah8/Design-Milestone/raw/master/Slack%20bot.png)  

##### External Services  
   The REST API calls to the static analysis tools serve as the computation engine of the application. These APIs then use the source files as input to their static analysis algorithm and provide the middleware with the output as a response. The application uses dedicated APIs like Codeburner, Veracode and jedi for analysing programming languages like Java, C and Python respectively.   
   Messages from the user trigger the application to perform actions accordingly.

![img](https://github.ncsu.edu/rshah8/Design-Milestone/raw/master/Middleware.png)  

##### Middleware  
   This layer consists of a NodeJS server along with the BotKit module that enables the middleware to interface with Slack via the RTM API. It listens for inputs sent by the user on the front-end. The source code gathered in the middleware is converted into a structure compatible with the external API and sent as a request to the respective API depending on the language of the source file(s).. In case of GitHub repositories provided by the user, the middleware can access the repository by sending a request to GitHub, which in turn returns the source files. The middleware also handles exceptions in case of unsupported languages.

##### Constraints and Guidelines:
   * The analysis results are limited to certain language/extension of the input files that are supported by the external analysis tool
The precision, quality and performance of the results may vary as per the different APIs/tools that are used to analyse different languages.
   * The input files are not modified by the bot as per the results so that the user has the freedom to choose the changes that he/she feels are necessary. 
   * The best time to use static analysis tool is early in the software development cycle.
   * The application can be used by each developer in the team. In case of limited licenses, a manager can use it to analyse each developer’s code.
   * It is recommended to supply well-formatted syntactically correct code for best results
   * Developers should not use this tool as a crutch. Learning to apply best practices in software development independently is a must for every developer.

