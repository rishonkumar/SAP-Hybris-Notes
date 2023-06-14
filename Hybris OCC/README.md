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
