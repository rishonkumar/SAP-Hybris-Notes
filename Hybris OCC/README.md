## How to build new OCC

    1) ant modulgen
    2) ycommerce webservices
    3) rrrstraininigwebservices
    4)

    S1) Identify input/output of the contract and what type of operation(CRUD)

        Get the course details so the REQUESTMETHOD is "GET"

    S2) create item type "courses" and perform ant clean all

    S3) Model type is created

    s4) write a flexible search query based on input criteria in "Core extension" and define in springxml

    s5) Wrtie a service class

    s6) definde courseData object in facedes-beans  / ant all check(-beans.xml)

    s7) write populator and converter to convert course model object to course data object

    s8) write faced class in faced extension to call the service class and convert the course model to course data objext

    s9) define facade class, converter and populator in face-spring.xml


    s10) define WSDTO in traniningcommmerecewebservice-bean.xml / ant all

            CourseDataWsDTO.java is created

    s11) Write a controller based on i/p critiera with GET method

    s12) define the filed mapping from coursedata -> coursedataWSDTO in dto-mapping-v2-spring.xml

    s13) define the scope of courseDATAWSDTO into  dto-mapping-v2-spring.xml / ant clean all

    backoffice -> base store

    Dao -> CourseModel

    Faceds -> COurseModel -> CourseData (till s9)

    COntroller -> COurseData -> CourseDataWsDTO

### OOC =>

    Meant for Web Services in RESTful

    Hybris provides 2 types of OCC V1 and V2  => Stateless

### Customer Registration using OCC API

    We need to some config

    1) Using Ycommercewebservices template
    2) Add local extension
    3) Change the web root path => into extension-info.xml / rest

    4) Need to run impex coz when u r exposing as service so we need to secure
    5) ClientId, and secret / Access token used for authorize the request

POSTMAN => JSON
Customer ID, Name, Last, Pass, TitleCode need to pass JSON through POSTMAN to Hybris

    When API makes a call
        First call go to controller
        THen faced layer in this all type of validation happens
        Then go to service layer we write all business logic ATQ requirement
        Then go DAO layer we write all oour Query like flexbile and also responsible to fetch data from DB
        Now again go back to service layer and manuplicate the data
        Back to faced layer and then controller awe get back the response

        Faced code always in facades extension

        Services and DAO layer in core extension

### V1 and V2

    OCC v1: This is the original version of OCC that was introduced in earlier versions of SAP Hybris Commerce. It provides a RESTful API for building custom storefronts and integrating external systems with the commerce platform. OCC v1 follows a resource-based approach where each resource (e.g., products, carts, orders) is represented by a specific URL endpoint. The API supports CRUD (Create, Read, Update, Delete) operations and provides basic functionality for managing the commerce-related entities.

    OCC v2: OCC v2 is an enhanced version of the API introduced in later versions of SAP Hybris Commerce (starting from version 1811). It builds upon the foundation of OCC v1 but introduces several improvements and additional features. OCC v2 provides a more flexible and powerful API for building modern, headless commerce experiences. It introduces a GraphQL-based API, allowing clients to request precisely the data they need, reducing over-fetching and under-fetching of data. It provides more advanced querying capabilities, supports data batching, and enables real-time updates using subscriptions. OCC v2 offers more extensive customization options and a more efficient way to retrieve and manipulate data.

### What does OCC resutful API is stateless mean

    When we say that the OCC (Omni Commerce Connect) RESTful API is stateless, it means that the API does not maintain any information or state about the client's previous requests or interactions. Each request made to the API is treated as an independent, self-contained operation.

    No Session State: In a stateless API, the server does not store any session-specific information or context related to a client's interactions. Each request from the client must contain all the necessary information to complete the request.

    Stateless Operations: Each API operation is performed based solely on the data and parameters provided in the request. The server does not rely on any previous requests or maintain any state between requests. This ensures that each request can be processed independently.

    Authentication and Authorization: Stateless APIs typically use authentication mechanisms such as API keys, tokens, or OAuth to validate the client's identity and ensure authorization for accessing specific resources. The authentication information is included in each request, allowing the server to verify the client's credentials for each individual operation.

    Scalability: Stateless APIs are inherently scalable because they do not require the server to maintain session state or store client-specific data. This allows the server to handle a large number of concurrent requests efficiently, as it does not need to manage and synchronize state information across multiple requests.

### Explain session in terms of OCC?

    In version 2 of the application, a session is created for each individual request. Think of a session as a temporary storage area that keeps track of information related to the current user's interaction with the application. It acts like a container for storing data specific to that user's session.

    For example, let's say a user adds items to their cart and logs in with their account. In this scenario, the cart and user information are stored in the session. The session keeps this data associated with the user throughout their interaction with the application, allowing the application to access and update it as needed.

    With the per-request scope of the session in version 2, a new session is created for each incoming request. This ensures that each user's session data remains separate and isolated from other users. So, if multiple users are accessing the application simultaneously, each user will have their own unique session to store their specific information.

    This per-request session scope helps maintain the integrity of user data and prevents information from being mixed up between different users. It allows the application to accurately retrieve and update the user-specific data for each request, ensuring a smooth and personalized experience for each user.

### V1 vs V2

    1. Session Per Request: In version 2, a new session is created for each individual request. This means that the session is short-lived and scoped to the specific request being processed. Once the request is completed, the session is discarded. This approach ensures that each user's session data remains separate and isolated from other users.

    2. Stateless Requests: Unlike version 1, where requests are stateful and rely on session data, version 2 follows a stateless request model. This means that each request contains all the necessary information to process it, without relying on previous session data. Instead of storing data in the session, relevant information is typically sent within the request payload or retrieved from a data store or database.

    3. No JSESSIONID: In version 2, the concept of a JSESSIONID is not used because there is no need to track and identify long-lived sessions. Since each request has its own session, there is no need for a session identifier to be stored as a cookie on the client-side.

    4. Enhanced Scalability and Performance: The session-per-request model in version 2 provides better scalability and performance compared to version 1. By eliminating the need to maintain long-lived sessions, the application can handle a larger number of concurrent users without the risk of session conflicts or bottlenecks.


    Overall, version 2 moves towards a stateless architecture where session data is minimized, and requests are self-contained. This approach simplifies the handling of requests, improves scalability, and reduces dependencies on session management.

### What are the filters a new V2 request passes through.?

    1. Session Filter: This filter is responsible for managing the session for each request. It creates a new session for each request and handles session-related operations such as session creation, retrieval, and expiration. It ensures that each request has its own isolated session.

    2. Site Matching Filter: This filter matches the incoming request with a specific site or domain within the application. It determines which site the request belongs to based on the request URL, hostname, or other criteria. This filter helps route the request to the appropriate site-specific configuration or logic.

    3. Spring Security Filter Chain: The Spring Security filter chain is a series of filters provided by the Spring Security framework. These filters handle various security-related tasks, such as authentication, authorization, and protection against common security vulnerabilities like cross-site scripting (XSS) and cross-site request forgery (CSRF). The specific filters and their order in the chain depend on the configuration and requirements of the application.

    4. User Matching Filter: This filter is responsible for identifying and matching the user associated with the incoming request. It may extract user information from the request, such as a user ID or authentication token, and use it to determine the corresponding user in the system. This filter ensures that the request is processed in the context of the correct user.

    5. Cart Matching Filter: This filter deals with the shopping cart functionality in the application. It matches the request with a specific cart or shopping session based on user information, session ID, or other criteria. This filter ensures that the request is related to the correct cart or shopping session, allowing the application to handle cart-specific operations and modifications.
