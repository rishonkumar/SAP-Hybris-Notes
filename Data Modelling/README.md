Data modelling {PIM} Type System

SAp commerce by default provides lot of tables

Example

    Cart Order User Product Category

To see those tables

    \*.items.xnl (core-items => Product User,cms2-items => CMS related tables/types)

How to create table/type , create cols, create relations, manage DB

    ANS IS DATA MODELLING

Model Classes / Object

    Are generated during the build[ant clean all]

Q = based on what
Type defined in -items.xml

    	If itemtype name = Abc then generated model class name = AbcModel.java

    	User UserModel, Cart CartModel

Q = What Model class contains

    	Attriburtes are columns,
    	Qualifiers are columns name

    	for every attribute will be 1 variable 1 set method 1 get mothod

Q = What happens if attirbute is String and localized String

    		In DB => localized u can add any language english,french
    		In only String u can enter only 1 value

    		IF attribue is locailzed 1 var 2 set and get methods

type = "boolean" vs "java.lang.boolean"

    		boolean
    			variable, set(), isRequired()

    			IN DB => True, False

    		java.lang.boolean
    			  Unique, setUnique, getUnique,

    			  In DB => True, False, N/A

SO USING MODAL CLASSES WE CAN SET , GET THE DATA

PURPOSE OF MODAL CLASS

     WE can perform DB operations (create,get,update,delete)

REQUIRMENT => I want to create custom USING CODE (Model file)

    			set 1
    				CustomModel cm = new CustomerModel();
    							or

    				CustomerModel cm = modelService.create(CustomerModel.class); (Recommnded)


    			step 2
    				Enter the data
    				cm.setId("Rishon")
    				cm.setName("ABC")
    				cm.setDesc("Testing")

    			step 3 Perform DB Operations

    					modelService.save(cm);


    		TEMPLATE

    			S1 => Create Obj
    				YYYModel ob = modelService.create(YYYModel.class)

    			S2 => Set the Data
                    ob.setCol1("abc")
                    ob.setCol2("RRS)

                S3 => Perform Db Operations
                    modelService.save(ob)

Scenarios We want to create Emp[type/table] with 3 cols

        S1 => *=items.xml
            Write the code / design

              <itemtype code = "Emps">
                <deployment table = "Emp" typecode="4 ....>
                <attibute qualifiers = "EmpID..>
                <attibute qualifiers = "EmpName..>
                <attibute qualifiers = "EmpSal..>
              </itemtype>

        S2 => do the build
                Model file will be generated "XXX then during the build (ant clean all), there will be model class created = XXXModel.java"

        S3 => go to hac => perform the update
            It will be converted into table

        deployment table => DB purpose

        if u want to create cols/attribute in own table then go with the deployment tag

        If u dont use deployment tag then col or attribute are created current itemtype parent deployment

        in attribute tag
            if persistence type = "Property" -- creted in DB
            if persistence type = "Dynamic" -- not creted in DB => this nly for Sap commerce Cals


        typecode
         <deployment table = "Emp" typecode="4 ....>
            Range is 0 - 32767
            Unqiue number given to tables

            < 10K number SAP will use
            > 10K you can use
                Whenever u r creating a table
                    Use the typecode after 10k

IMPEX => FSQ

    Insert , delete, Update the record => IMPEX

    TO get the record => Flexible search Query

    Flexible => SAP commerce

    SQL => DB Query


    SAP E-commerce is DB Independent

Scenarios
IN sap System
Material Plant [Table]

    Req => Asked to create same in SAP commerce
    & complete with code .. name .. desc
    === client started entering the data
    after 20 days Client -> Asked to add 2 more cols => valueCenter,  profitCenter


    ANS
        We code in items.xml
        Then build
        After update
        It got created

        Default is 255 char now client asked only 10 char max so u coded it

        <columtype database = "orcale"> <value>VARCHAR(10) </value>
        <columtype database = "mysql"> <value>VARCHAR(20) </value>

        Once again build

        But there is no expected in the table WHY ??
            Decreasing the size is not possible

        Solutions
            1) INIT [but prob is all the data will lost]
            2) Export data. Drop those cols "hac- update"
            3) Limitations
                When client enter Put the Validations

    IN SAP WE DO DATABASE DESIGN IN *.items.xml

        If u want to change dp from orcale to mysql then just confiugure the DB
        config/local.properties
        if mysql specify driver then copy it
        start the server and perform INIT/Update

How to move the data

        SOL 1 : export import in DB
        SOL 2 : Impex in SAP COMM

MODIFIERS

        Whenever we create attibute/col => then we can specify the modifier
        <modifier read = "true" write = "false" search = "true" optional = "true" initial = "true" />

            read = get, write = set, initial = val, optional = mandatory or not , Unique = no duplicate

        Select * from order where code = 123

WHERE TO DO DATA MODELLING

        Data modelling we do inside trainingcore => resources

Syntax for data modelling

         We need 6 tags

         ACE MRI [TYPE System] Order is important

         Atomic
         Collection
         Enum
         Map
         Relation
         itemtype

ATOMIC TYPES

        20 -20 int long
        float double
        String
        boolean

Collection Type

        How to find how many order are placed by each User
            Select User.UserID, Count(Order.OrderID) from User,Order where User.UserId = Order.UserID group by Order.UserID

        Performance Issue

        Soln -> Use collection

        List , Set => Collection

Collection => 1- M Relation

        If u Know the approx data size == Collection
            Retrive of data is fast

        If u dont know data == Relation
            We can have more data

ENUM

        There is possiblilty that u can have small capital combination
            If u have fixed value for your DB - Col then go with ENUM

Order

    OrderNo CardType[ENUM] Status[ENUM]

Map

        Use to store K-V pairs
        Keys are Unique . vals can be duplicate

Relation

        1-1 1-M M-N

        User order 1-M
        User Cart 1-M

        to define the realtions
            Source[Left-type] Target[Right-Type] == cardinality


            1-1 Relation HHow many min tables req => 1
            1-M Relation HHow many min tables req => 2
            M-M Relation HHow many min tables req => 3 , coz we need one more table to store
                    So we will add deployment tag

ITEM TYPE => HEART OF TYPE SYSTEM

        <itemtype code = "Product"
                  extends = "GenericItem"
                  autocreate = "true"
                  generate = "true"

                  >

            autocreate true => will be created in DB
                        false => u telling sap commerce that type already exisiting & contiune to update

            generate => for model files (You are saying SAP to create model files)


        even in <attribue> tag if you dont mention autocreate, generate then it will take from parent(itemtype)

Unique and PK

    Unique = will not allow duplicate and will allow NULL value
    PK = will not allow duplicate and will not allow NULL value + internally it creates index

    <indexes> = we create for unique columns
    IMPROVE the performance while retriving the data

    <indexes>

    PK1(c1) PK2(C8) == yes
    PK1(c1,c4,C9) == Yes. composite PK


    Important
        if itemtype extends genericItem then use deployment tag
        if itemtype doesnt extends genericItem then dont use deployment tag
        deployment typecode must be >10k
        deployment table  must be defined for all M_M relations

DATA MODELLING CODING

    REQ = Create small DB to provided the recorded session in website

        U have the website [youtube.com]

        S1 Lets create all new types

        Where we can do the data modelling = *core ext
            *core-items.xml

    trainingcore.xml

    We can do Coding = Top-Bottom, Bottom-Top (Best)

        s1 -> create 2 enums

            <enumtype code = "PostStatus" >
                <value code = "DRAFT" />
                <value code = "PUBLISHED" />
                <value code = "DELETED" />
            </enumtype>
            <enumtype code = "CommentStatus" >
                <value code = "PENDING" />
                <value code = "APPROVED" />
                <value code = "BLOCKED" />
            </enumtype>

        S2 -> CMSSITE [No need to create. It's already given ]

        S3 -> Create blog type/table

                <itemtype code  = "blog" extends = "genericItem" autocreate = "true" generate = "true"

                    <deployment table = "blog" typecode = "11001">

                    <attribute qualifiers= "name" type = java.lang.String">
                        <modifier initial = "true" read = "true" write ="false" optional="false" unique = "true" />
                        <persistence type = "Property"
                    </attribute>

                    <attribute qualifiers= "active" type = "boolean">
                        <persistence type = "Property"
                    </attribute>

                </itemtype>


                GO to backoffice -> Types and verfiy (blog)

                for post itemtype
                    since blog and this is connected
                    add inside attribute tag type = "blog"

                    <attribute qualifiers="blog" type = "blog"

                    />

        s4      create the relations

        ant clean all => *Model.java is created

        start the server
                HAC - Update
