# requirement-analysis
Requirement Analysis in Software Development.
this is front end pro 
What is Requirement Analysis?
Why is Requirement Analysis Important?.
Why is Requirement Analysis Important?
Requirement Gathering.
Requirement Elicitation.
Requirement Documentation.
Requirement Analysis and Modeling.
Requirement Validation.
Requirement Validation.
Requirement Gathering
Requirement Elicitation
Requirement Documentation
Requirement Analysis and Modeling
Requirement Validation.
Key Activities in Requirement Analysis
Types of Requirements.
Use Case Diagrams.
Missing: alx-booking-uc.png
## Use Case Diagrams

**Use Case Diagrams** are visual representations of the interactions between **actors** (users or external systems) and the system itself. They help to illustrate the functional requirements of a system, showing what the system does and who interacts with it.  

**Benefits of Use Case Diagrams:**
- Provides a clear overview of system functionality.
- Helps stakeholders and developers understand system interactions.
- Facilitates requirement analysis and validation.
- Serves as a communication tool between technical and non-technical team members.

### Booking System Use Case Diagram

Below is the use case diagram for the booking system:


![Booking System Use Case Diagram](alx-booking-uc.png)

# My Project Title

A brief description of your project.

---

## Use Case Diagrams

### What are Use Case Diagrams?

Use Case Diagrams are a type of behavioral diagram used in the Unified Modeling Language (UML). They provide a high-level, graphical representation of a system's functionality from the perspective of external actors. The primary purpose of a Use Case Diagram is to define the boundary of a system and the relationships between the actors and the different functionalities (use cases) of the system.

**Key components of a Use Case Diagram include:**

* **Actors:** External entities (people, organizations, or other systems) that interact with the system.
* **Use Cases:** Ovals that represent a specific functionality or a service provided by the system.
* **System Boundary:** A box that delineates the scope of the system.
* **Relationships:** Lines that connect actors to the use cases they perform.

### Benefits of Use Case Diagrams

* **Clarity and Simplicity:** They provide a simple, easy-to-understand view of the system's functionality, making it accessible to both technical and non-technical stakeholders.
* **Requirements Elicitation:** They are excellent for identifying and defining the functional requirements of a system at the early stages of a project.
* **Communication:** They serve as a common language for communication between clients, developers, and other project team members.
* **Scope Definition:** They clearly define what the system will and will not do, which helps in managing project scope and expectations.
* **Testing:** They can be used to derive test cases, as each use case represents a specific user scenario to be tested.

### Booking System Use Case Diagram

Here is a Use Case Diagram for a typical booking system, outlining the key actors and their interactions with the system's functionalities.

**Actors:**

* **User/Customer:** An individual who wants to make a booking.
* **Admin/Staff:** An employee who manages bookings and system data.
* **Payment Gateway:** An external system that processes payments.

**Use Cases:**

* **Search for Availability:** A user looks for available dates/times for a service.
* **View Details:** A user views detailed information about a booking option.
* **Make Booking:** A user reserves a time slot or service.
* **Cancel Booking:** A user cancels an existing booking.
* **Make Payment:** A user pays for their booking.
* **Manage Bookings:** An admin views, modifies, or cancels bookings.
* **Manage Availability:** An admin updates the system's availability schedule.
* **Process Payment:** The payment gateway handles the financial transaction.

![ALX Booking System Use Case Diagram](alx-booking-uc.png)
How do hotel booking applications like Airbnb, Booking.com and OYO work to provide such a smooth flow, from hotel listing to booking, to payments? And all without a single glitch! In this blog, you will get a detailed explanation for this.

Press enter or click to view image in full size

Photo by Edvin Johansson on Unsplash
These are so huge that they have a high amount of user traffic that needs to be processed. So to manage these we have to follow micro-service architecture. This means we have to divide the system into small chunks for each type of task.

Let’s understand the flow one by one. I have divided it into 3 parts:

Hotel Management Service
Customer Service (Search + Booking)
View Booking Service
Final Design
Hotel Management Service
This is the service that will be given to hotel managers/owners. In this managers can manage their hotel's related information. Here managers have a separate portal to access the data and update it.

Press enter or click to view image in full size

Hotel Management Service Architecture
Whenever an API is triggered from the hotel manager app the initial request is been sent to the load balancer, then the load balancer distributes the requests to the desired server to process. The hotel service cluster has multiple servers that have the container for hotel service-related API.

Now, this hotel service interacts with the Hotel DB cluster which follows the master-slave architecture to reduce the load in the database. Basically, in this approach, we create a replica of the master database which are called a slave database. Master DB is used for a write operation and slave DB is used for reading operation only. Whenever a write operation is performed on the master database it syncs the data to the slave database.

Get Purnendu Kar’s stories in your inbox
Join Medium for free to get updates from this writer.

Enter your email
Subscribe
Whenever any data is updated in the database API sends the data to the CDN(Content Distributed Network) and to a Messaging Queue System(like Kafka, RabbitMQ) for further processing. A CDN is a geographically distributed group of servers that work together to provide fast delivery of Internet content.

Customer Service (Search + Booking)
This is the service that will be given to customers. In this customers can search and book a hotel. Here customers have a separate portal to access the data and process it.

Press enter or click to view image in full size

Customer Service Architecture
The CDN app shows the content to customers like the nearby hotels, recommendations, offers etc.

As we discussed in the previous section, hotel data is sent to messaging queue system to process it. Here we have a messaging queue consumer that takes the data from the queue and stores the data in elastic search.

The customer app hit’s the API then the load balancer redirect and distribute the request to the respective service to process the request. Here we have two services one for searching for a hotel and a booking service to book the hotel and booking service also interacts with the payment service which will be a third-party service.

The search service has to get the data from Elastic Search. Elasticsearch is a NoSQL Database that is best for its search engine functionality.

The booking service communicates with Redis and the booking database cluster. Redis is caching system, that’s stores temporary data so that data need not fetched database and which could eventually reduce the load in the database also reduce the response time of API.

Any changes made in the database will be sent to the messaging queue. Then the consumer will take the data from the queue and put it to Casandra. For archival we are using Casandra because with time data size will increase in the database, increasing query time. So that’s why we may need to delete old data from the database. And Casandra is a NoSQL database that is good at handling a high volume of data.

View Booking Service
Here all current and old booking details are shown to the user. Both managers and customers use this service.

Press enter or click to view image in full size

View Booking Architecture
The Customer/Manager app sends the request to the load balancer and it distributes the request to booking management servers. Then the service request for data through Redis and Cassandra. through Redis, it requests recent data as it is a caching server. Which could reduce the loading time on the app side.

Final Design
Press enter or click to view image in full size

Hotel Booking System Design
As you can see in the above design there is a Kafka consumer for notification, notification consumers send the notification. That could be to the customer/manager, like whenever a customer books a hotel notification is sent to the manager or if a new offers come it’s notified to the customer.

Apache Streaming service takes the data from messaging queue and stores it in Hadoop which could be used for BigData analysis for multiple purposes. Like business analysis, finding potential customers, audience categorisations etc.

Missing: alx-booking-uc.png https://medium.com/nerd-for-tech/system-design-architecture-for-hotel-booking-apps-like-airbnb-oyo-6efb4f4dddd7
