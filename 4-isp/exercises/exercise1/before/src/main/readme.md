### Build script

mvn compile

### Run tests command

mvn test

### Startup script

cd ~/workspace
mvn clean install -Dmaven.test.skip

### Tasks

In this exercise, you will see an interface that violates the ISP, and then you will refactor the code to
respect this principle.

Let's start by taking a look at the ```NotificationService``` interface. It has 4 methods that deal with various notification
scenarios. At first glance, this interface seems ok. Unit we look at the overall context, the classes that implement it.
There are 2 classes that implement this interface:
- ```KindergartenNotificationService```
- ```CompanyNotificationService```

> ✔️ Both classes do not implement the notification interface fully, just partially. They do not throw exceptions, but because the
method bodies are empty, we have a good clue that we are violating the ISP. Let's do some refactoring.


###### Identify responsibilities of interface

It is safe to assume that each method in the ```NotificationService``` interface is a reason to change.
So it is a great idea to have one interface for each notification method. This way, we can combine them to meet
our business needs.

###### Create new interface for text notifications

Create a new interface to handle text notifications.

```
public interface TextNotificationService {
    void sendTextMessage(String phoneNb, String text);
}
```

> Validation ```mvn test -Dtest=PaymentServiceTests#payUsingPayPalNotExists```

###### Create new interface for email notifications

Create a new interface to handle email notifications.

```
public interface EmailNotificationService {
    void sendEmail(String to, String text);
}
```

> Validation ```mvn test -Dtest=FileTests#emailNotificationServiceInterfaceExists```

###### Create new interface for Slack notifications

Create a new interface to handle Slack notifications

```
public interface SlackNotificationService {
    void sendSlackMessage(String slackUser, String text);
}
```

> Validation ```mvn test -Dtest=FileTests#slackNotificationServiceExists```

###### Create new interface for internal application notifications

Create the final interface to handle the internal application notifications.

```
public interface InternalApplicationNotificationService {
    void sendInInternalApplication(String text);
}
```

> Validation ```mvn test -Dtest=FileTests#internalApplicationNotificationServiceExists```

###### Refactor Kindergarten notification service

Open  ```KindergartenNotificationService.java``` class.

1. Implement only the notification interfaces that are needed: 
   1. TextNotificationService
   2. InternalApplicationNotificationService
2. Remove the unused methods (the ones with an empty body)

> Validation ```mvn test -Dtest=KindergartenNotificationServiceTests```

###### Refactor Company notification service

Open  ```CompanyNotificationService.java``` class.

1. Implement only the notification interfaces that are needed:
    1. EmailNotificationService
    2. SlackNotificationService
2. Remove the unused methods (the ones with an empty body)

> Validation ```mvn test -Dtest=CompanyNotificationServiceTests```

###### Delete original "fat" interface

At this point, you can go ahead and delete the ```NotificationService.java``` interface.

> Validation ```mvn test -Dtest=FileTests#notificationServiceInterfaceDeleted```
> Validation ```mvn test```