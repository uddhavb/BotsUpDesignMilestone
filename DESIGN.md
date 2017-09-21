# MILESTONE: DESIGN

#### Problem Statement:  
   Code written by developers could have bugs like null pointer exceptions, vulnerabilities, duplicated code, complex code and deviations from coding standards etc. These issues could cause major problems when the application is in production. Early and efficient detection of these problems is a growing need of any software development project.

  In real life, code bases are very large and consists of source code in various languages. Analysing these code bases one by one is tedious. Also each programming language could require different static analysis tools which could be cumbersome to setup and configure on every developer's environment. Managers or lead developers might also want to analyze the developers code as part of the code review process. A tool that simplifies these tasks would improve the productivity of the team.


#### Bot Description:  
  The proposed solution to the above problem is an interactive bot that takes in source code in various formats from the user and returns the result of the static analysis performed on it. The bot’s backend utilizes APIs exposed by various code analysis tools. 

  The bot is housed in the slack platform and that has a simple chat interface. Here, the bot takes in a source file or files (or an archive) of "any environment" or a link to the repository as an input, analyses the file/project and returns the results of the analysis in the file/project in the same chat window. 

  The bot would require a one-time setup to connect external tools to it.

  The bot analyses these errors and presents it to the user one by one in the chat window. The bot then gives the user suggestions as per the best coding practice guidelines.


#### Use Case:  
```
Use Case 1 : A developer wants to verify his code and find potential deviation from coding standards, memory issues, security vulnerabilities
1 Preconditions
   The developer must have access to slack
2 Main Flow
   [S1] User will log on to slack and access the bot chat window
   [S2] User will provide code to the bot
   [S3] The bot will return a result of the static analysis performed
3 Alternate Flows
   [E1] The language in the input file is not supported by the Bot or its APIs
```
```
Use Case 2 : A manager or a technical lead developer wants to review code of other (possibly cross functional) developers in the team.
1 Preconditions
   The developer must have access to slack
2 Main Flow
   [S1] The  developer will submit the link to the code in the git repo
   [S2] The bot will return the result of the static analysis performed on that code
3 Alternate Flows
   [E1] The language in the input file is not supported by the Bot or its APIs
```
```
Use Case 3 : A recruitment firm needs to evaluate code submitted by several potential candidates
1 Preconditions
   The user must have access to the slack platform
2 Main Flow
   [S1] The user will submit a zip archive containing several source files, possibly, in several languages
   [S2] The bot will return the analysis and the suggestions for each source file
3 Alternate Flows
   [E1] The zip archive is corrupt
   [E2] One or more files in the zip archive contain unsupported languages

```

#### Design Sketches:  
Wireframe:  

![img](https://github.ncsu.edu/rshah8/Design-Milestone/raw/master/frame-git.png)

![img](https://github.ncsu.edu/rshah8/Design-Milestone/raw/master/frame-wire.png)

Storyboard: 

#### Architecture Design:   
![img](https://github.ncsu.edu/rshah8/Design-Milestone/raw/master/asfdds.png)

As we can see from our architectural specification diagram, the user will interact with our bot in three ways. The user can either upload a file which contains the source code, a zip file which contains a collection of source code files and a link to a GitHub repository which contains the source code files. Our bot acts as an interface between the user and the middleware. The platform we have chosen for our bot is Slack.

![img](https://github.ncsu.edu/rshah8/Design-Milestone/raw/master/Slack%20bot.png)

The APIs form the backbone of our bot. Depending on the language of the source file(s), the middleware calls the respective APIs. These APIs then use the source files as input to their static analysis software and provides the middleware with the output. Our project provides a single API for each programming language including Hopper, clang-tidy and jedi for Java, C and Python respectively. Thus, the APIs act as an interface between the middleware and underlying software.

![img](https://github.ncsu.edu/rshah8/Design-Milestone/raw/master/Middleware.png)

The middleware forms the central processing unit of our bot. It is this unit where the logic behind our bot lies. In case of GitHub repositories provided by the user, the middleware can access the repository by sending a request to GitHub. This request is processed by GitHub to provide the files. The middleware interprets the files that have been provided by the user and calls the respective API. The middleware also performs exception handling tasks including providing an ‘Unsupported Language’ error. It also renders the output as provided by the API, back to the user.


