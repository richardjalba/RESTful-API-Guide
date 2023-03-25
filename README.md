**API** is an entryway for clients to reach the data in a server.

Designing APIs increases productivity, structure, professionalism, and makes future changes easier to manage.

Types of APIs

**Public API**: Available on the internet.

**Private API**: Used internally in company.

**Partner API**: Used between companies.

TIP: Following the existing design is better than changing it. Gradually apply new design going forward.

How Does HTTP Work?

A Client will send an HTTP Request to a Server, then receive an HTTP Response in return.

A successful Request will return a 200 OK Response, along with the requested data.

A failed Request will return a 500 Error Response, or another error status code.

Parts of a HTTP Request

**Method**: GET, POST, PUT, DELETE

**URL**: Identifies unique resources (/example)

**Headers**: Metadata for requested data

**Content**: Body and query parameters

Parts of a HTTP Response

**Status Code**: Success 2XX, Error 4XX 5XX

**Headers**: Metadata of response data

**Content**: Response body

What is REST?

**RE**presentational **S**tate **T**ransfer

Transfer from one state to another state.

**RESTful** is a methodology for designing APIs optimally.

REST principles optimize approach to web services, builds around resources, is platform independent, not limited to any language, standard interface, stateless for scalability.

TIP: Postman is a tool to manage and test your API by working as a mock environment where you can send Requests as a Client and see the Server Responses it returns, and more.

URL vs URI

**URI**: Uniform Resource Identifier

Identifies resource by location or name.

*URI = URL + URN*

**URL**: Uniform Resource Locator

Identifies resource by location.

*Ex. www.example.com/sample/demo*

**URN**: Uniform Resource Name

Identifies resource by name.

*Ex. sam:isbn:4567880395*

Resources of an API

Revolves Around Business Entities

Doesn’t Need To Be Based On Physical Data

Resource Names Should Be Nouns, Not Verbs

*Good: customers, orders*

*************~~Bad: GetAllCustomers, GetOrderByName~~*************

Resources Should Be Collections

Resources are organized in Collections, which contain datasets that can each individually be reached with unique URLs.

*Examples:*

*/collection/unique-identifier*

*/customers/10  (You are targeting customer with unique identifier of 10 in the customers Collection)*

*/orders/34   (You are targeting order with unique identifier of 34 in the orders Collection)*

The unique identifiers tend to be id’s with matching URLs.

Non-Resource Data Is Used For Query Parameters

Filter /colleges?type=engineering

Sort /colleges?sortBy=collegeName

Pagination /colleges?page=5&size10

Defining Relationships Between Resources

When you find relationships between resources you can use URL navigation for sub-objects.

*/api/colleges/15/students*

*Students of the college whose ID is 15*

There should not be another model for any Resources overlap. There is no need for additional complexity by adding unnecessary Models.

Instead you should be returning the Model for the specifically requested Resource.

*In this case, any student with ID 15 in students Collection that happens to be in colleges Collection.*

You will return the requested data as a student Model, not a any other Model. 

Use Simple URLs

The most complicated you should allow your structure is: *api/collection/item/collection*

Don’t allow your URL to get longer and more complex.

Combine Related Resources

Make APIs simpler by combining related resources.

Overview of HTTP Operations

The following are operations, or also known as HTTP Methods:

**GET**: Retrieves data from Resource

**POST**: Creates new Resource in Server

**PUT**: Updates data in existing Resource

**DELETE**: Deletes chosen Resource

**PATCH**: Partial update for existing Resource

Selecting Operations Based on Item or Collection

Collection */courses*

Item */courses/10*

GET  Retrieve all data from item or collection

POST  Create new item in a collection

PUT  Update all data in item or collection

DELETE  Remove item or collection

Idempotent Operations

**Idempotent** means making multiple identical requests would have the same effect as making a single request; GET, PUT, DELETE.

You can’t POST the same Resource multiple times, it only works once. This is why POST and PATCH are not idempotent methods.

Differences Between POST, PUT, and PATCH

|  | POST | PUT | PATCH |
| --- | --- | --- | --- |
| Resource URL | Server assigns new URL | Client mentions URL | Client mentions URL |
| Idempotent | Maybe | Yes | Maybe |
| Ideally Used On… | Collections to add new Item | Single Item to update data | Single Item to update partial data |

Designing Request Parameters

**********************************Query Parameters**********************************: Request targets a Resource while passing in additional data.

**********************************Path Parameters**********************************: Request is passed within part of the URL.

**Headers**: Typically has default values, unless passed in otherwise.

**Cookies**: Stores data in browser.

Designing Request Content

**************************Content Type**************************:
application/json

********************Request Body********************: 
GET (none), POST (data), PUT (data), DELETE (ID)

HTTP Status Codes

******2XX****** Success

******3XX****** Redirection

******4XX****** Client-side Error

******5XX****** Server-side Error

Choosing Response Formats

Servers can support more than one format.

******************application/json (most popular)
application/xml
application/octet-stream
text/plain
text/csv
text/html
image/png
image/jpeg******************

Designing Response Body

Follow your naming convention.

Don’t expose database details.

Array of items for Collections.

Consider supporting pagination for Collections.

Designing Error Handling

**********************************Client-side Errors:**********************************
Invalid credentials, parameters, or ID’s

****************Server-side Errors:****************
Exceptions

Support For Filtering Data

Consider filtering data if:

- Collection may contain large amounts of data.
- Pass filter as query string in the URL.

*******Returns a collection:
GET /api/colleges*******

************************Returns a filtered collection:
GET /api/colleges?zipcode=95183************************

Support For Paginating Data

Consider paginating data if:

- GET requests may return large number of items
- Paging can return subset of data at a time
- Page number and page size to be used

*******Returns large amounts of items:
GET /api/colleges*******

************************Returns a subset of items:
GET /api/colleges?page=3&pageSize=10************************

Additional considerations:

- Include total items count in response subset
- May include navigation between pages (prev/next in response subset)

Support For Sorting Data

Consider sortingdata if:

- May include field name to sort the results on
- Provide a default value for sorting

*******Returns target items:
GET /api/colleges*******

************************Returns target items sorted by:
GET /api/colleges?sort=collegeName************************

Why Version APIs?

**********************Public APIs********************** must be versioned.

************Partner APIs************ must be versioned.

**********Private APIs********** optional versioning due to internal use.

Versioning should be planned and designed from beginning; best for enhancing existing APIs to support new features.

Ways to Version APIs

**************************No Versioning**************************
Change API directly.
Beware for breaking changes.
No additional development.
Good for small teams.
Won’t be tracking changes.
Ideal for Private APIs.

****************************URL Versioning****************************
Version is added as part of URL.
*******************/api/v1/colleges/12*******************
*******************/api/v3/colleges/12*******************
Need to support existing versions per update.
Lack of docs can lead to breaks in prev versions.
Simple to use; URL.
Very clear in usage.
Need to change URL every time.
Difficult to handle URL linking in responses.
Very popular in production.

********Query String Versioning********

Add the version as query string to API request.

The version being used is added to query string.
*******************/api/colleges/12?version=1*******************
*******************/api/colleges/12version=2*******************

Existing integrations are not affected.
URL structure remains the same.
Client might forget to pass correct version query string.
Difficult to handle URL linking in responses.

****Header Versioning****
Custom header added to indicate version.
No Versioning:*******************/api/colleges/12*******************
Example of Header Versioning:
*Header: X-API-Version=2
Request: /api/colleges/12*

Existing integrations are not affected.
URL structure remains the same.
Versioning logic is separate from API.

Client may forget to pass the header
Difficult to handle linking in responses.

**********Media Type Versioning**********
Include the version along with media type header.
*****************************************Content-Type: application/vnd.cms.v1+json
Content-Type: application/vnd.cms.v2+json*****************************************
Ideal if response consists of URL linking

Ideal form of API versioning.
Easier to handle URL linking in responses.

Complex compared to other approaches.
Not cache-friendly.

 ************************************CREATING A NEW API************************************

**STEP 1: Capture the Metadata**

*This includes the title of the API, description, etc.*

**STEP 2: Determine the Type of API**

**STEP 3: Identify Server Base URL**

*http://{hostname}:{port-optional}/{directory}*

**STEP 4: Identify Resources**

*course, student*

**STEP 5: Name Resources Collections as Plural**

*courses (/api/courses)*

*student (/api/students)*

**STEP 6: Build Resource Models**

*Course Model*

   *Course Id*

   *Course Name*

   *Course Duration*

   *Course Type*

**STEP 7: Select Identifier for Each Resource**

*Course Model*

   *Course Id (Unique Identifier)*

   *Course…*

*Student Model*

   *Student Id (Unique Identifier)*

   *Student…*

**STEP 8: Identify Associations Between Resources**

*Courses*

   */api/courses/{courseId}/students*

   */api/courses/{courseId}/course-subjects*

*Students*

   *None (No Associations; bottom-level Resource)*

**STEP 9: Check URL Complexity**

*Courses*

   */api/courses*

   */api/courses/{courseId}*

   */api/courses/{courseId}/students*

**********************************STEP 10: Identify Operations For Each Resource**********************************

*Courses*

   */api/courses*

*GET
POST*

   */api/courses/{courseId}*

*GET
PUT
DELETE*

   */api/courses/{courseId}/students*

*GET
POST*

******************STEP 11: Identify Parameters Required for Operation******************

*~~Query Parameters: None~~*

*Path Parameters:
   None - Collections
   courseId - Unique Id for Course Model (individual Item)*

*~~Headers: None~~*

*~~Cookies: None~~*

****STEP 12: Identify Content Type of Operation Request****

************application/json************

****STEP 13: Identify Request Body of Operation****

*POST /api/courses
Content-Type: application/json
Request Body Format:
****   {
   "courseName": Name of the course
   "courseDuration": Duration of the course in years
   "courseType": Type of the course
   }
Example:
   {
   "courseId": 1,
   "courseName": "Computer Science",
   "courseDuration": 4,
   "courseType": "Engineering"
   }*

******************STEP 14: Identify HTTP Status Codes per Operation******************

*****/api/courses
   GET
      HTTP 200 OK
   POST
      HTTP 201 CREATED
      HTTP 400 BAD REQUEST*****

*****/api/courses/{courseId}
   GET
      HTTP 200 OK
      HTTP 404 NOT FOUND
   PUT
      HTTP 201 CREATED
      HTTP 404 NOT FOUND
   DELETE
      HTTP 204 NO CONTENT
      HTTP 404 NOT FOUND*****

******************STEP 15: Identify Response Content Type per Operation******************

****************application/json****************

******************STEP 16: Identify Response Body per Operation******************

*GET /api/v1/courses
Responses:
HTTP 200 OK
Response Body Format:
[{
     "courseId": Unique ID of a course in the system
     "courseName": Name of the course
     "courseDuration": Duration of the course in years
     "courseType": Type of the course
}]
Example:
[{
     "courseId": 1,
     "courseName": "Computer Science",
     "courseDuration": 4,
     "courseType": "Engineering"
},
{
     "courseId": 2,
     "courseName": "Computer Science",
     "courseDuration": 4,
     "courseType": "Engineering"
}]
…*

******************STEP 17: Handle Errors for Operations******************

*HTTP 400 BAD REQUEST
Response Body Format:
{
     "error":
  {
     "code": Unique error code for your project
     "message": Useful message for developers to identify the issue
  }
}*

****************STEP 18: Add Filtering If Needed****************

****************STEP 19: Add Pagination If Needed****************

****************STEP 20: Add Sorting If Needed****************

*GET /api/courses
Request:
Query parameters (optional):*

*courseType:*

- *Support for Filtering.*
- *Filter the results by course type.*
- *Example: api/courses?courseType=Engineering*

*page*

- *Support for Pagination.*
- *Page number of the result to fetch.*
- *Example: api/courses?page=1&size=4*

*size*

- *Support for Pagination.*
- *Page size of each result.*
- *Example: api/courses?page=1&size=4*

*sortBy*

- *Support for Sorting.*
- *The field name to sort the results on.*
- *Example: api/courses?sortBy=courseName*

****************STEP 21: Identify API Versioning Approach & Set Version****************

**********************************Versioning Schema: URL Versioning
Version: 1.0**********************************
*/api/v1/courses
/api/v1/colleges*
