## Architectural Pattern Analysis

The selected Spring PetClinic files demonstrate two architectural patterns from the given list: **MVC (Model-View-Controller)** and **Layered Architecture**.

### 1. MVC (Model-View-Controller)

Spring PetClinic strongly demonstrates the **MVC architectural pattern** through its Spring MVC controllers and domain model classes.

- **Model:** Classes such as `Owner.java`, `Pet.java`, `PetType.java`, and `Visit.java` represent the application's domain data and relationships.
- **Controller:** `OwnerController.java`, `PetController.java`, `VetController.java`, and `VisitController.java` handle HTTP requests and coordinate application operations.
- **View:** The controllers return view names and populate the Spring MVC `Model`, allowing the presentation layer to display the required information.

For example, `OwnerController`, `PetController`, `VetController`, and `VisitController` are responsible for handling web requests, while domain entities such as `Owner`, `Pet`, and `Visit` represent the model. This separation of request handling and domain data is evidence of the MVC pattern.

### 2. Layered Architecture

The selected files also demonstrate a **Layered Architectural Pattern** because responsibilities are separated into different logical layers.

The structure visible from the selected files can be summarized as:

**Presentation/Web Layer**
- `OwnerController.java`
- `PetController.java`
- `VetController.java`
- `VisitController.java`

**Domain/Model Layer**
- `Owner.java`
- `Pet.java`
- `PetType.java`
- `Visit.java`

**Data Access Layer**
- `OwnerRepository.java`

The controllers handle web requests, the domain classes represent application entities and relationships, and the repository provides the persistence abstraction. This separation of responsibilities is consistent with Layered Architecture.

### Architectural Pattern Conclusion

Therefore, based on the selected files, Spring PetClinic can be described as a **Layered Architecture that uses MVC for its web/presentation structure**.

### Patterns Not Clearly Demonstrated

The selected files do not provide sufficient evidence to classify the project as **Pipe and Filter** or **Event-Driven** architecture. Although a web application can operate in a client-server environment, the selected files do not provide enough evidence to use **Client-Server** as a primary architectural pattern.



## 1. Owner.java

### Overview

`Owner.java` defines the Owner domain entity used by the PetClinic application. It stores owner information such as address, city, and telephone number and maintains the relationship between an owner and their pets. It also provides operations for finding pets and adding visits.

### SOLID Analysis

The class generally follows the **Single Responsibility Principle (SRP)** because it represents an owner and manages behavior directly related to the owner-pet relationship. Its inheritance from `Person` and its use as a domain entity are also consistent with the application's domain model.


## 2. OwnerController.java

### Overview

`OwnerController.java` handles web requests related to owners. It manages owner creation, searching, updating, displaying owner information, and pagination. Persistence operations are delegated to `OwnerRepository`.

### SOLID Analysis

The class generally follows **SRP** because it is responsible for handling owner-related web requests. It also supports **Dependency Inversion Principle (DIP)** by depending on the `OwnerRepository` abstraction instead of directly implementing database operations.

### Code Smell Analysis



**Comments:** The comments in `processFindForm()` explain the different search cases and therefore provide useful documentation rather than unnecessary comments.

## 3. OwnerRepository.java

### Overview

`OwnerRepository.java` provides the persistence abstraction for Owner objects. It extends Spring Data JPA's `JpaRepository` and defines a custom query method for finding owners by the beginning of their last name.

### SOLID Analysis

The interface supports **DIP** by providing an abstraction between the application and the persistence implementation. It also follows **SRP** because its responsibility is focused on Owner data access.

## 4. Pet.java

### Overview

`Pet.java` defines the Pet domain entity. It stores the pet's birth date and type and maintains the relationship between a pet and its visits.

### SOLID Analysis

The class follows **SRP** by representing a pet and maintaining behavior directly associated with the pet, such as adding visits.

Its relationship with `NamedEntity` is part of the domain model and provides common entity functionality.

## 5. PetClinicApplication.java

### Overview

`PetClinicApplication.java` is the main entry point of the Spring Boot application. Its `main()` method starts the PetClinic application using `SpringApplication.run()`.

### SOLID Analysis

This class is a strong example of **SRP** because its responsibility is limited to application startup.


## 6. PetController.java

### Overview

`PetController.java` handles web requests related to pets. It loads owners and pets, prepares pet types, handles pet creation and updating, performs validation, and saves changes through `OwnerRepository`.

### SOLID Analysis

The controller generally follows **SRP** at the architectural level because it handles pet-related web requests. It also supports **DIP** by using repository abstractions for persistence.

### Code Smell Analysis

**Duplicated Code:** The owner lookup and owner-not-found exception handling in `findOwner()` and `findPet()` repeat the same logic used in `OwnerController` and `VisitController`.

There is also repeated pet validation logic between `processCreationForm()` and `processUpdateForm()`. Both methods check whether the pet's birth date is in the future and both contain duplicate-name validation logic.

**Long Method:** `processCreationForm()` and `processUpdateForm()` combine validation, error handling, persistence, exception handling, and redirection. They are moderate examples of methods that could be simplified through extraction.


## 7. PetType.java

### Overview

`PetType.java` represents the type or category of a pet, such as a cat, dog, or hamster. It is a persistent entity that inherits common functionality from `NamedEntity`.

### SOLID Analysis

The class follows **SRP** because its responsibility is to represent the PetType domain entity.


## 8. VetController.java

### Overview

`VetController.java` handles requests for displaying veterinarian information. It provides paginated veterinarian results and a JSON-compatible veterinarian list.

### SOLID Analysis

The class follows **SRP** because its responsibility is focused on veterinarian-related web requests.

It also supports **DIP** because persistence is delegated to `VetRepository` rather than implemented directly in the controller.

The pagination operations are already separated into `findPaginated()` and `addPaginationModel()`, which keeps the controller methods relatively focused.


## 9. Visit.java

### Overview

`Visit.java` defines the Visit domain entity. It stores a visit date and description and initializes a new visit with a default date of tomorrow.

### SOLID Analysis

The class follows **SRP** because it represents the state and behavior of a clinic visit.

## 10. VisitController.java

### Overview

`VisitController.java` handles the creation of new visits for pets. It loads the owner and pet, creates a visit, validates the visit date, and saves the owner.

### SOLID Analysis

The controller generally follows **SRP** by handling web requests related to visits and supports **DIP** by using `OwnerRepository` for persistence.

However, `loadPetWithVisit()` performs several coordination tasks: loading the owner, finding the pet, placing objects into the model, creating the Visit, and adding the Visit to the Pet. This creates a mild SRP concern because some domain coordination could potentially be moved to a service.

### Code Smell Analysis

**Duplicated Code:** The owner lookup and owner-not-found exception handling is very similar to the implementation in `OwnerController` and `PetController`.


**Comments:** The comments explaining Spring MVC's method invocation order are useful framework documentation and should not be classified as a Comments smell.
