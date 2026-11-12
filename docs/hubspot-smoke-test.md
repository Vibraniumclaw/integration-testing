# HubSpot action smoke test

The smoke test uses two new standalone records. Each action is verified by reading its result back from HubSpot.

```mermaid
flowchart LR
    start(["Run local smoke test"])

    subgraph calls["Create Call"]
        direction TB
        call["Run create_call with a test title and duration"]
        readCall["Read the new call and verify title and duration"]
        call --> readCall
    end

    subgraph meetings["Update Meeting"]
        direction TB
        seed["Create a standalone test meeting"]
        update["Run update_meeting with the new meeting ID"]
        readMeeting["Read the meeting and verify notes and outcome"]
        seed --> update --> readMeeting
    end

    start --> call
    start --> seed
    readCall --> done(["Both actions verified"])
    readMeeting --> done

    classDef entry fill:#172554,color:#fff,stroke:#60a5fa,stroke-width:2px;
    classDef action fill:#155e75,color:#fff,stroke:#67e8f9;
    classDef verify fill:#166534,color:#fff,stroke:#86efac;
    class start entry;
    class call,seed,update action;
    class readCall,readMeeting,done verify;
```

CI uses the main HubSpot account, where these write tests would create test records. The dev account lacks the features required to run both tests.
