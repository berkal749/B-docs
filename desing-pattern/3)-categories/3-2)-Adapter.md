# ADAPTER
ok , u can think of the adapter patttren as , a real adpater... well whe excpected that , 
for example someone came from USA and wanna charge his laptop , he cant do it in directly because the elcetecty plugs are jut too defrenete to begin with , soo what we use ? we use an adapter.

in coding , it usally used when  we deal with lgeacy systems that use xml while we use json , or when an api provide the data in miles and foots rather than meters that our code use , rather then changing the code , u  just create a class , cllaed the adapter class , that will find a way to translate between the two systems or api`s 

##  how to use the adapter genreally :

u must have at leaset two classes one is the  , one is yours the new shiny one , and the adaptee , the old incompatble one like in this example:

```java 
// Our app expect JSON notifications
public interface ModrenNotification {
    void send(String jsonMessage);
}

```

and we have an adapteeeeee:
```java
public class LegacyEmailService {
    public void sendEmailInXML(String xmlData) {
        System.out.println("Legacy Service sending XML: " + xmlData);
    }
}
 ```

 obiviously , those two cant work togther  soo we make an adapter this adapter will implements (pretends in very precsies words ) that it is a modren notification and it will have an instance of the legacy class to find logic  to make them work toghter :

 ```java
 public class EmailAdapter implements ModrenNotification {
    private LegacyEmailService legacyService;

    public EmailAdapter(LegacyEmailService service) {
        this.legacyService = service;
    }

    @Override
    public void send(String jsonMessage) {
        // 1. Convert JSON to XML 
        String xmlMessage = "<msg>" + jsonMessage + "</msg>";
        
        // 2. Delegate the work to the legacy service
        legacyService.sendEmailInXML(xmlMessage);
    }
}
 
 
  ```

  in the mian will look like somthing like this :

  ``` java

  public class Main {
    public static void main(String[] args) {
        // The old service we can't change
        LegacyEmailService oldService = new LegacyEmailService();

        // The adapter that makes it compatible and it is pretending that it is  the orgignal modren notfifcation class

        INotification mailer = new EmailAdapter(oldService);

        // Our app code stays clean and modern
        mailer.send("{ 'body': 'Hello World' }");
    }
}
  
  
  ```


  of course  some mermaid :


  ```mermaid
  classDiagram
    %% The Target (Modern Interface)
    class ModrenNotification {
        <<interface>>
        +send(jsonMessage: String)
    }

    %% The Adaptee (Legacy System)
    class LegacyEmailService {
        +sendEmailInXML(xmlData: String)
    }

    %% The Adapter
    class EmailAdapter {
        -legacyService: LegacyEmailService
        +EmailAdapter(service: LegacyEmailService)
        +send(jsonMessage: String)
    }

    %% Client / App
    class Main {
        +main(args: String[])
    }

    %% Relationships
    %% EmailAdapter IMPLEMENTS INotification
    EmailAdapter ..|> INotification : realizes

    %% EmailAdapter ASSOCIATES WITH (USES) LegacyEmailService
    EmailAdapter --> LegacyEmailService : adapts

    %% Main DEPENDS ON the Interface and creates the Adapter
    Main ..> INotification : uses
    Main ..> LegacyEmailService : instantiates 
  
  
  
  ```



