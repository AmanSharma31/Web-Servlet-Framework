# Web-Servlet-Framework

A framework to remove the effort and time consumes in writing the servlets for every request. By using this framework one does not need to write servlets, instead write classes and methods, the framework will do the rest work. The goal of this framework is to make the process of developing the back end of any web application easy, fast and efficient.


## Getting-Started
By following the instruction you can implement the framework in your webapp.  

### Installation

Create a new folder in webapps folder in your server, assume it to be `testing` for now and create a `WEB-INF` folder in *testing*.   
Create following essentials folders in *WEB-INF* -  
* `classes` 
* `lib`
* `uploads`     

and create a `web.xml` file here.    
    
You can create your package in *classes* folder and then can write your classes here. You don't need to write servlets here instead write classes and their methods. I will describe later how to write them.   

Create a file named *ServiceConfiguration.conf* in *WEB-INF*. 

Download all the jar from the repository, and put them in

    WEB-INF\lib    

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

#### Beans

The framework's response is of JSON type which contains the object of *AJAXResponse* class. This class has following properties
        
         Object response;
         boolean success;
         Object exception;

*success* tells whether the service for which the request came has invoked successfully or not. If the service has been invoked successfully then the success will be true, exception will contains empty string and the response will contain whatever that service has returned. Otherwise, if service has not been invoked successfully then success will be false, response will contain empty string and exception will contain the exception or reason due to which the service couldn't invoked.    


#### Annotations   

There are five Annotations that will be needed for creating classes and services, which are present in following package 

    com\thinking\machines\school\annotations     
 
which are     
* `Path`
* `Upload`
* `ResponseType`
* `Forward`    
* `Secured`

All the annotations have a member named *value* of type String except the *Upload* annotation.   
<br>

- **Path** annotation can be applied on both classes and services. You must assign a value to *value* member, which is going to be used for the searching of the service. The framework will only recognize those services on which *Path* annotation will be applied. The value of the *value* member must be unique for all classes and methods of that classes.

- **Upload** annotation can only be applied on services. This annotation tells the framework that the service will receive file\s from the client side and it will pass this file\s to that service. You must apply this annotation if you want to upload some files. The *uploads* folder that we have created above is where the uploaded files are going to be saved.

- **ResponseType** annotation is also appliable only on services. This annotation specify the response type of the service. There are three types of values considered of the member *value* in the framework, which are
     * **text/html** means the return type of service is a string
     * **json** means the return type of service is a Class or interface
     * **nothing** means the return type of service is void

- **Forward** annotation is applicable on services. The value of *value* member of *Forward* annotation can be the value of *value* member of *Path* annotation of another service or the name any jsp or html file on which you want to forward the request. Through this annotation you can forward request on another service after execution of the current service. The value that will be returned by the last service executed will be sent to the client as response. You can only forward request to services of the same class. If you want to forward request on a page then framework will add a Header to the response with name *Forward* and value as *1*, in this case the *AJAXResponse* beans properties *response* contains the name of that jsp or html file on which you want to forward the request, *success* will be false for this case and *exception* will be an empty string. On client side you before checking for the exception you should check for the *Response* header and if it presents then forward request to that page.

- **Secured** annotation is applicable on services. You should apply this on those services which need the authentication from the user first and then only that service can be executed. Let's get into a little deep into the this.   
If you want to use this annotation, then first you need to implement the *AuthenticationInterface* interface from the package    
    
    
        com\thinking\machines\school\servlets
    
Implement this interface on your class and override its method two methods which are 

    public boolean checkIfLoggedIn(ServletContext sc,HttpServletRequest request, HttpSession session);
	public Object notLoggedIn(HttpServletRequest request, HttpServletResponse response);
	
Apply *Path* annotation on this class and these two services. The first method *checkIfLoggedIn* is to check whether the user is logged in or not, if this method returns true then the service for which request came will be invoked and its response will be sent, and if *checkIfLoggedIn* returns false then the second method *notLoggedIn* will be invoked and whatever you have written in that method will be executed and the response of second method will be sent.   
Now while applying *Secured* annotation on any service, you must have to assign the value to its member which is the value of *Path* annotation applied on the class, which has implemented the *AuthenticationInterface*.


Now in the file "ServiceConfiguration.conf" written in *WEB-INF* you have to write the packages where your classes and services are present, so that framework can create a database of services on which *Path* annotation is applied. The packages must be written in JSON format, like below 

```json
{
"packages":["package1","package2","packagen"]
}
```

You have to name the variable as *packages*.



#### PDFCreator    

There is a tool that come along with this framework which will create two pdf file in your desired location. The pdfs name are *services.pdf* and *errors.pdf*.    

* services.pdf contains the list of path annotation's value of the services along with the types of data the service consumes and produces. This pdf will contain this data only when you have written unique values to the *Path* annotation as I have mention above.

* errors.pdf contains the name of the class along with the name and path of its services, if you have assigned same value to the *Path* annotation of the services. This pdf will remain empty until you assign same value to the *Path* annotation.

This tool is present in the following package, with name **PDFCreator**

        com\thinking\machines\school\tools

To run this tool, type following command in command prompt from anywhere

    java -classpath "path to WEB-INF\lib\*";"path to WEB-INF\classes" com.thinking.machines.school.tools.PDFCreator "path where you want to save these pdfs"

after that two pdfs will be created at the path given by you. This pdfs can work as a help tool while developing the front-end part of the web. You can look into this pdfs for the path of the services you want to invoke on the server side. If you have added or updated the value of *Path* annotation, then you need to run the tool again to see the newly updated services in these pdfs.

----------------------------------
While creating classes remember to import the below package, so that you can use the annotations.

    import com.thinking.machines.annotations.*;

Now create your classes in your packages and apply *Path* annotation on those classes, write their methods and apply *Path* annotation and other annotations as your need.    

Now add your packages in the *ServiceConfiguration.conf* file.    

You can run the *PDFCreator* tool to create the pdfs, where you can see all the paths.   

Now create the front-end and in the *url* variable write url in following manner

    "url" : "/webapp Name/service/classPath/servicePath"      
    
*classPath* is the value of *value* member of Path annotation applied on the class, and *servicePath* is the value of *value* member of Path annotation applied on the service that you want to invoke on the server side.    

Remember to check for the *Forward* header in the response before going for *exception*.



