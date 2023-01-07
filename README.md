# Documentation

## Introduction:
This project is associated with DIT365 (Distributed systems) course at university of Gothenburg and is developed by team 8. The code for this system has been split into four different repositories and the user stories are therefore traced in the individual repositories.

## Project Description
Dentistimo is a distributed system which allows users to book dentist appointments in the Gothenburg region and allows for more clinics to be added in the future.  The user can see the list of clinics and also their locations on a map and select one. The user also can see all the available time slots of each clinic and choose a timeslot and then book it. The user receives a confirmation when the booking takes place successfully.

## Objectives
The aim of Dentistimo is to give customers a website where they can book dental appointments and access information about the dental clinic, including its address, location on a map, operating hours, and available dentists. Customers can create accounts on Dentistimo, where they can view and update their user information, as well as review their booking history. The goal of Dentistimo is to provide an easy and convenient way for customers to schedule dental appointments and access information about the dental clinic.


## Scope
The scope of Dentistimo is to offer customers a website where they can book dental appointments and access information about the dental clinic, including its address, location on a map, operating hours, and available dentists. Customers can also create and manage their user accounts through the website. This service is specifically designed for customers who want to make dental appointments, and is currently offering bookings at four dentistry clinics located in Gothenburg. Dentistimo does not provide any additional services or functions beyond these core features.

## Tools & Technologies
- Visual studio code IDE
- Draw.io 
- Google document
- Vetur plugin for Vue tooling
- ESLint plugin for linting Vue, JS, and HTML code
- Debugger for Chrome plugin for debugging
- Node.js
- MongoDB
- Postman
- mosquitto broker
- Mocha for testing the mqtt

Here is a list of libraries we have used:
- Bootstrap
- JsonWebtoken Library
- Vue-full-calendar
- Axios
- SweetAlert2 
- Bcrypt
- Mongoose
- NodeMailer
- Leaflet
- Opossum


## Other Repositories
- [Client](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/client-server)
- [Backend](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/data-manager)
- [Middleware](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/t8-project)
- [Security](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/user-manager)



## Milestones and user stories: 
User stories connected to each issue can be found on the individual milestone pages. There is also a complete collection of all milestones and user stories on the [wiki](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/documentation/-/wikis/Milestones).

- [Milestone 1: Visualization](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/documentation/-/milestones/1#tab-issues)
- [Milestone 2: Map](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/documentation/-/milestones/2#tab-issues)
- [Milestone 3: Logic](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/documentation/-/milestones/3#tab-issues)
- [Milestone 4: Testing](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/documentation/-/milestones/4#tab-issues)
- [Milestone 5: Security](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/documentation/-/milestones/5#tab-issues)

All issues could be found [here](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/documentation/-/issues/?sort=created_date&state=closed&first_page_size=20)

## Diagrams & charts:
- [Functional decomposition diagram](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/documentation/-/wikis/functional-decomposition-diagram)
- [Sequence diagram](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/documentation/-/wikis/Sequence-Diagram)
- [Time planning](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/documentation/-/wikis/Time-Planning)
- [Component diagram](https://git.chalmers.se/courses/dit355/dit356-2022/t-8/documentation/-/wikis/Component-diagram)

## Architectural sytles & Design desicions
We have used service-oriented architectural style to increase the availability and fault tolerance of the system and to have the possbility to add more components to the system in the future. Having decoupled distributed components, this style is more cost effective and less complicated than other styels like microservises, which was the reason we chose it. Dentistimo is a distribtued system contianing multiple components (security, backend and frontend) sharing a single database. Furthermore, we have benefited from publish and subscribe architectural style for the communication among components. The communication takes place via a broker (mqtt). This way the components can not communicate with each other directly and all the requests and responses are sent through the broker. This style helps with the scalability, low copuling and extendability of the components. The last architectural style to be mentioned is pipe and filter which is used in the broker to filter all the messages it receives and send them to the correct components.

To ensure the distributed nature of the system, as stipulated by the project brief, the system was to be separated into a number of independent components, the final version of which is illustrated by this component diagram. The final components settled upon to be used were the Frontend, dealing with the client side display and handling of direct input, the Backend, dealing with processing received data, and communicating with the database, and finally, the Security, dealing with authorization and validation of requests and communication. All aforementioned components have been created such that they operate independently of each other, in keeping with the principles of distributed systems, and their only interlinking point of connection is the central MQTT broker. To facilitate such arrangement, each of the components includes an MQTT manager, which translates incoming and outgoing messages into the appropriate format, such that the data can be used where needed. In order to achieve mutual decoupling of the components, service-based architectural style was employed, which was later combined with the ‘pipeline and filter’ architectural style to ensure security and fault tolerance. 

A number of development tactics have been used to bring the end product to the light of day, most notably conglomeration of Vue.js components on the client side of things, and usage of timeouts to ensure the proper execution order of asynchronous operations. To elaborate on the former, when creating the “Vues” for the frontend visualisation, a large number of relatively separate functionalities have all been included in the Home.vue file, among others the map, the individual clinic data, and the generation of available time slots for each clinic and dentist. This decision was reached upon the realisation that the relevant functionalities would need to exchange significant quantities of data amongst each other, thus making it more efficient to include all of them in the same file, rather than separating them into individual vue components. While this increases the interdependence of the program as a whole, it was deemed to be the preferable option, especially since the functionalities have not been necessary to call upon anywhere else in the codebase. Further details regarding the structure of the code can be gleaned from the class diagram. Another majorly notable design tactic was the usage of timeouts to prevent the program from moving on too rapidly, thus operating without the necessary data (for example, trying to display available time slots before the broker has had sufficient time to retrieve them from the database). This would lead to bugs where the data would be improperly displayed. While the solution of using timeouts has negatively impacted the performance in a slight manner, this was recognised as being minor enough of a setback when employed at the scale of the existing project to warrant its continued usage as opposed to other possible solutions.

Besides those mentioned above, further design decision tactics have been employed within the program. In all component parts concerning themselves with MQTT communication, Quality of Service of 1 has been utilised, thus ensuring that at least one – but possibly more – messages are received. This has been chosen due to its increased speed of communication, as opposed to QoS-2, helping to ensure smooth and stable connection. While in certain sections, namely booking an appointment, receiving multiple messages of the same type could lead to issues (i.e. doubly booking a slot), a decision has been made to handle multiple incoming requests directly in the backend endpoint responsible for the creation of bookings, thus not only dealing with multiple messages received from the same source, but also the potential of multiple users attempting to book the same slot simultaneously.

Furthermore, it has been decided to use HTTP rather than MQTT for internal communication within particular components. While HTTP would result in marginally slower response rate from the program, this has been determined to be negligible on the present scale. Additionally, due to a large number of endpoints present, HTTP has been chosen due to the simplicity of implementation, as well as increased readability of code.

On the topic of database storage and retrieval, it has been decided that the database should store and concern itself with the slots that have been booked, as opposed to ones still available, and that this information, combined with opening hours, will later be used to generate available time slots separately. This has been implemented as described due to the predicted small scale of the system, which is expected to result in fewer booked than empty slots at most times, thus making retrieving booked slots, rather than empty ones, the lighter load on the system, decreasing response time to user requests.


## Project Management Practices
We used scrum practice as an agile method for this project. As a scrum team we had a scrum master and product owner and the rest of the team were developers. The TA was another stakeholder with whom we constantly provided our progress in the weekly meetings. In the project planning session we came up with milestones and user stories for all the features that were going to be implemented in the project. This was divided into four sprints. At the beginning of each sprint, we held planning sessions to decide on the user stories that we would work on. Every week, we had a weekly meeting where team members updated the group on their individual progress. At the end of each sprint, we held sprint review meetings with our customer, the TA, to discuss our work, and sprint retrospective meetings to discuss any issues that arose during the sprint and how we could improve our team's workflow. We used a Trello board to manage our product and sprint backlogs, and GitLab for continuous delivery, linking our commits to related issues to increase traceability.



## Contributors:
- [Yasamin Fazelidehkordi](https://git.chalmers.se/yasaminf) (Developer)
- [Amin Mahmoudifard](https://git.chalmers.se/aminmah) (Product owner, Developer)
- [Emrik Dunvald](https://git.chalmers.se/dunvald) (Scrum master, Developer)
- [Daniel J Coetzer](https://git.chalmers.se/coetzer) (Developer)
- [Julia Ayvazian](https://git.chalmers.se/ayvazian) (Developer)
- [Patrik Samcenko](https://git.chalmers.se/samcenko) (Developer)
- [Vladyslav Shatskyi](https://git.chalmers.se/shatskyi) (Developer)

The tasks or user stroies assigned to each person could be found in trello and also could be traced in commits related to different issues in other repositories.
## Other Links:
[Trello](https://trello.com/b/NFWMRtVd/dentistimo)
