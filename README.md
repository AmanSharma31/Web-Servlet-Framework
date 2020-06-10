# Web-Servlet-Framework

A framework to remove the effort and time consumes in writing the servlets for every request. By using this framework one does not need to write servlets, instead write classes and methods, the framework will do the rest work. The goal of this framework is to make the process of developing the back end of any web application easy, fast and efficient.


## Getting-Started
By following the instruction you can implement the framework in your webapp.  

### Installation

Create a new webapp folder in your server, assume it to be `testing` for now and create a `WEB-INF` folder in *testing*.   
Create following essentials folders in *WEB-INF* -  
* `classes` 
* `lib`
* `uploads`  
and create a `web.xml` file here.    
    
You can create your package in *classes* folder and then can write your classes here. You don't need to write servlets here instead write classes and their methods. I will describe later how to write them.   

Create a file named *ServiceConfiguration.conf* in *WEB-INF*. 

Download all the jar from the repository, and put them in
> WEB-INF\lib    

Now in *web.xml* file write the following lines     
      
      <servlet>
      <servlet-name>FrameworkInitializer</servlet-name>
      <servlet-class>com.thinking.machines.school.servlets.FrameworkInitializer</servlet-class>
      <load-on-startup>0</load-on-startup>
      </servlet>
      
      <servlet>
      <servlet-name>TMService</servlet-name>
      <servlet-class>com.thinking.machines.school.servlets.TMService</servlet-class>
      </servlet>
      <servlet-mapping>
      <servlet-name>TMService</servlet-name>
      <url-pattern>/service/*</url-pattern>
      </servlet-mapping>



### Usage

This framework is going to work only for AJAX applications. That means you can only exchange data throgh AJAX.    
There are five Annotations that will be needed for creating classes and services, which are     
* `Path`
* `Upload`
* `ResponseType`
* `Forward`    
* `Secured`

All the annotations have a member named *value* of type String except the *Upload* annotation.   
<br>
- **Path** annotation can be applied on both classes and services. You must assign a value to *value* member, which is going to be used for the searching of the service. The framework will only recognize those services on which *Path* annotation will be applied.

- **Upload** annotation can only be applied on services. This annotation tells the framework that the service will receive file\s from the client side and it will pass this file\s to that service. You must apply this annotation if you want to upload some files. The *uploads* folder that we have created above is where the uploaded files are going to be saved.

- **ResponseType** annotation is also appliable only on services. This annotation specify the response type of the service. There are three types of values considered of the member *value* in the framework, which are
     * **text/html** means the return type of service is a string
     * **json** means the return type of service is a Class or interface
     * **nothing** means the return type of service is void

- **Forward** annotation is applicable on services. The value of *value* member of *Forward* annotation is the value of *value* member of *Path* annotation of another service. Through this annotation you can forward request on another service after execution of the current service. The value that will be returned by the last service executed will be sent to the client as response. You can only forward request to services of the same class.

- **Secured** annotation is applicable on services. You should apply this on those services which need the authentication from the user first and then only that service can be executed. 



Download the jar file of the framework. 
Put it in the following folder
> ..WEB-INF\lib  


