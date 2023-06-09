Data Modeling | Product Modeling | Dynamic Attributes |Impex |Use Case Activity

    https://www.youtube.com/watch?v=O_DNcbU17l4

Type System SAP Hybris

    https://www.youtube.com/watch?v=wwQP99Q8vEM&list=PLp1aGN8kFsXJOObAapeUG58vQnMnCgDMw&index=5

Impex Tutorial SAP Hybris

    https://www.youtube.com/watch?v=nQLtkGA2MlI&list=PLp1aGN8kFsXJOObAapeUG58vQnMnCgDMw&index=7

Flexible Search Tutorial SAP Hybris

    https://www.youtube.com/watch?v=jR57JIkx3vk&list=PLp1aGN8kFsXJOObAapeUG58vQnMnCgDMw&index=8

Data Validation Framework

    https://help.sap.com/docs/SAP_COMMERCE/d0224eca81e249cb821f2cdf45a82ace/8ba7f5a9866910148b749e7217fa45fa.html

SAP Hybris Solr / Hybris Solr Indexing / Searching

    https://www.youtube.com/watch?v=sf4cepBrlHU&t=2430s

MVC FLOW
REALTIONS AND Collection
Why order is important
Indexing (How indexing works)
Solr
How data are saved in Solr

NOTE IF item-type = XXX then during the build (ant clean all), there will be model class created = XXXModel.java

HOW TO restrict the HAC Options

    Employee are the one who use HAC

    S1 = Create the user[Employee] =
          Go to backoffice
            User -> Employee

            Set the password

    s2 -> go to particular user u created Select "General"
          In groups => u can give admin(He will have all access) group


    s3 -> Change the groups (U can give support or whatever needed)

        In spring-security-config.xml = all the different roles are defined

    We can do all this with ImpEx

In backoffice we have so many Options
Cart Order User Product Sites

Let's say i am product owner show only product realted Options

Q How to restriction types

    Cart[type] = key option[type permission] search and give permission

Wha to do after 30 days
Challenge if u do initialize u will get back but prob is whatever custom data created is DELETED

    sol2
        We can apply 90 days
        go to local.properties => licence.sap.sapsystem=DEV
        go to platform => licence.bat -temp CPS_SQL

Where we can see SAP PK numbers

    backoofice = go to record = adminstration = pk

CORNJOBS

    whatever is running on background it will show if(staged/sync)

FLEXIBLE search

    how to insert record
    how to delete record
    how to delete record
        => IMPEX


    How to get/read the data => FLEXIBLE SEARCH Query

---

In sap 1 ext can depend on 1 or more extension

q) where can we see ext dependencies

    extension.info = requires-extension
    TypeCode should be given above 10000+ coz its already reserved by hybris

The basic search and Product Cockpit get the data from Solr Doc

    To move DB to Solr Doc
    System => Facet => Facet Search Config => Solr Config for backoffice => Click Index

How do we Index in Hybris?

    Go to the Back Office and sign in.
    Click on the system in the screen's navigation tree on the left side.
    Click on "Facet search" and "Facet search config."
    Choose the site and click on an index or "hot update-index."
    From the drop-down menu, choose the Index operation type update/full/to delete and the catalog for index.
    Just below the page, click the "start" link.
    When it's done, the screen will show a message of success.

SAP Hybris Extension:

    An extension is an encapsulated piece of the hybris e-Commerce Suite that can contain business logic, type definitions, a web application, or a hybris Management Console (hMC) configuration. That way, you link up in one place all of the functionality that covers a certain field of use.

SAP Hybris AddOn:

    The purpose of an AddOn is to wrap an extension. By wrapping an extension, it provides additional features to an extension without needing to modify the code of the extension. An AddOn extends the Commerce Accelerator without touching its codebase. Technically, an AddOn is an extension but at build time its codebase is copied inside target extensions to become a part of the extension itself.

IMPEX

    text based import export functionality provided by SAP HYBRIS which is in format of a CSV
    Allows user to CRUD platform items like Product,Customer,Delivery Modes etc

    bin->platform->ext->impex

Where is Impex Used

    Importing any data
    Creating Backups

Need of impex import

    to insert data like product,promotions,prices,stories

    these data needs to inserted in hybris to display on site

    to sync data with other System such as ERP

    data can be inserted at run time/initi/update

Need of export

    to migrate data from one hybris installation to another

    to create Backups

Operations => INSERT,UPDATE,REMOVE,EXPORT

Structure of impex => HEADER,VALUE LINES,MACROS,COMMENT

    HEADER SYNTAX

    Operation itemtype;attibute(refAttr)[modifier]

    INSERT Product;code[Unique=true]
    UPDATE Product;code[Unique=true];name[lang=en]
    INSERT_UPDATE Customer;CustomerId[Unique=true];group(uid)
    REMOVE Product;code[Unique=true]

    Insert unit;code[Unique=true];conversion;unittype
    ;pieces;1.0;pieces

    Update unit;code[Unique=true];conversion;unittype
    ;grams;100;grams

    Remove unit;code[Unique=true]
    ;grams

    MACROS => Whenever we are using most common word take it as a variable

    COMMENT = #

Important syntax:

    "#% impex.exportItemsFlexibleSearch(""select {PK} from {Product} where {code}='45572'"", Collections.EMPTY_MAP, Collections.singletonList( Item.class ), true, true, -1, -1 );"

Flexible Search

    SQL like Syntax
    Return list of object(Hybris items)

Syntax

        Select<SELECTS>FROM{TYPES}(WHERE<CONDITIONS>)(ORDERBY<ORDER>)

        ASC,DESC,DISTINCT,AND,OR,LIKE,JOIN,CONCAT

    Select \* from {product}

    select {code} from {product}

    select {code} from {product!}

    select {code}, {name[en]} From {product}

    select {code}, {name[en]}, {name[de]} From {product} where {name[en]} like '%cap%'
    //cap at end
    select {code}, {name[en]} From {product} where {name[en]} like '%cap'
    select {code}, {name[en]} From {product} where lower{{name[en]}} like '%cap'

    select {code}, {name[en]} From {product} where {creationTime} > to_date('2020-12-12 03600), ("Y-M-D-S")

    select {code}, {name[en]} From {product} where {creationTime} > to_date('2020-12-12 03600), ("Y-M-D-S") order by {code} DESC

    select {code}, {name[en]} From {product} where {name[en]} NOT LIKE "%men"

    select count(\*) from {product}

    select {code}, {name[en]} From {product} where {code} LIKE "300%" AND {name[en]} like "%cap"

FACADE PATTERN

    http://javainsimpleway.com/facade-pattern/

    Facade and service layer are design patterns. They are standard ways of writing code.

    Facade layer, as the name suggests, is something beautiful which is trying to hide something ugly behind it. In spring or any other framework which uses this layer, facade serves as an interface, which is hiding some complex implementation.

    Service layer on the other hand can be said as that “complex implementation” which facade layer is hiding. It can call DAO layer which hits DB. It can call third party services. It can be used to provide features like security,
