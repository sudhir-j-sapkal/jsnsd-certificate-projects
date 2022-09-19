
# OpenJS Node.js Services Developer (JSNSD) certification Exercises & Quick Notes

To prepare JSNSD certification exam Linux foundation provides one Learning Course called as [Node.js Services Development (LFW212)](https://training.linuxfoundation.org/training/node-js-services-development-lfw212/). Here I have tried to cover the examples given in this course.

## Exercises & Labs

 01. [Creating a Web Server With Core Http Module](https://github.com/sudhir-j-sapkal/01_CreateWebServerWithHttp)
 02. [Creating a Web Server and Routes With Core Http Module](https://github.com/sudhir-j-sapkal/02_CreateWebServerAndRoutesWithHttp)
 03. [Create a Web Server with Express](https://github.com/sudhir-j-sapkal/03_CreateWebServerWithExpress)
 04. [Create a Web Server with Fastify](https://github.com/sudhir-j-sapkal/04_CreateWebServerWithFastify)
 05. [Labs 3.1 & 3.2](https://github.com/sudhir-j-sapkal/05_Lab3.1_3.2)
 06. [Serving Web Content Using Fastify](https://github.com/sudhir-j-sapkal/06_Serving_WebContent_Using_Fastify)
 07. [Creating RESTful JSON with Express](https://github.com/sudhir-j-sapkal/07_Creating_Restful_JSON_With_Express)
 08. [Lab 5.1](https://github.com/sudhir-j-sapkal/08_Lab_5.1)
 09. [Manipulating Data With Rest Services Using Express](https://github.com/sudhir-j-sapkal/09_ManipulatingDataWithRestServices_Using_Express)
 10. [Consuming And Agreegating Services](https://github.com/sudhir-j-sapkal/10_Consuming_And_Agreegating_Services)
 11. [Proxy Http Request With Single Route Multiple Proxy](https://github.com/sudhir-j-sapkal/11_Proxy_Http_Request)
 12. [Proxy Http Request With Single Origin Multiple Route](https://github.com/sudhir-j-sapkal/12_Proxy_Http_Request_Single_Origin_Multiroute)
 
 ## Quick Notes
 1. How Not to Install Node
  ```bash
Often Node.js can be installed with a particular Operating System's official or unofficial package manager. 
For instance apt-get on Debian/Ubuntu, Brew on macOs, Chocolatey on Windows. 
It is strongly recommended against using this approach to install Node. 
Package managers tend to lag behind the faster Node.js release cycle. 
Additionally the placement of binary and config files and folders 
isn't standardized across OS package managers and can cause compatibility issues.

Another significant issue with installing Node.js via an OS package manager 
is that installing global modules with Node's module installer (npm) tends to require the 
use of sudo (a command which grants root privileges) on non-Windows systems. 
This is not an ideal setup for a developer machine and granting root privileges to the 
install process of third-party libraries is not a good security practice.
  ```
  2. How to Use Express Generator 
  ```bash
  A tool called express-generator generates this structure with some other additions. 
  We'll use the generator in the next chapter, for now we're concentrating on the 
  bare bones to create the equivalent of our minimum viable web server that we implemented in the previous section.
 
   express my-express-service
  
  ```
