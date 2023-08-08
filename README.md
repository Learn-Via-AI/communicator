
# Learn Via AI
![Logo](https://github.com/Learn-Via-AI/communicator/blob/main/logo.png)

## Architecture
### * Change required in below diagram *

```mermaid
flowchart LR

    user>user]

    userDb[(User \n Demography\n Data)]
    courseDb[(Course \n Meta course)]
    progressDb[(Progress \n History \n data)]
    personalCouseDb[(Personal \n courses)]

    signup([signup])
    login([login])

    courses{{course suggestion\nfilter n suggest}}

    myCourses{{my Cources}}

    generate((Generate\ncourse))

    consume((Consume\ncourse))

    feedback{ask\nQuestion}

    openAPI[/Open\nAPI/]

    user==>signup
    signup==>userDb

    user==>login

    login== continue \n with \n courses==>myCourses
    progressDb==>myCourses
    myCourses===>consume

    login== check \n availbale\n courses==>courses
    courses<==>courseDb
    courses==>generate
    generate<==>userDb
    generate<==>progressDb
    generate<===>personalCouseDb

    generate==>consume
    consume<===>progressDb

    consume== break \n post \n lesson ===>feedback
    feedback==>|continue|consume
    feedback==>|ask\nquestion|openAPI

    
```

## Login / signup  Flow

```mermaid
sequenceDiagram
    actor U as User
    participant US as user service
    participant db as CosmosDB
    
    U->>+US: Signup
    US->>+db: Save email and password in (user Entity)
    db->>-US: saved
    US->>-U: ok
    
    U->>+US: update details
    US->>+db: update demography in (user Entity)
    db->>-US: saved
    US->>-U: ok
    
    U->>+US: Login
    US->>+db: Check creds
    db->>-US: ok
    US->>-U: ok

```

## Course generation Flow

```mermaid
sequenceDiagram
    actor U as User
    participant com as course service
    participant db as CosmosDB
    participant gpt as ChatGPT

    U->>+com: submit prompt for course generation 
    com->>+db: get user data (user Entity)
    db->>-com: user data
    com-->>com: Engineer prompt

    com->>+gpt: call gpt with prompt
    gpt->>-com: get response

    com-->>com: convert to course entity
    com->>+db: save (course Entity)
    db->>-com: saved
    com->>-U: done, move to course

```

## Course management Flow

```mermaid
sequenceDiagram
    actor U as User
    participant com as course service
    participant db as CosmosDB

    U->>+com: get all courses
    com->>+db: get all courses for user
    db->>-com: list of cources
    com->>-U: list of courses

    U->>+com: Click on any course
    com->>+db: Get course by id
    db->>-com: course entity
    com-->>com: convert to course DTO
    com->>-U: course DTO

    U->>+com: mark section complete
    com->>+db: update (course Entity)
    db->>-com: saved
    com->>-U: done

```

## Consume course flow

```mermaid
sequenceDiagram
    actor U as User
    participant transcript as transcript service
    participant db as CosmosDB
    participant gpt as Chat GPT

    U->>+transcript: start a section
    transcript->>+gpt: make initial section prompt
    gpt->>-transcript: section details from GPT
    transcript->>+db: save the transcript with time n id
    db->>-transcript: saved
    transcript-->>transcript: synthesize speech
    transcript->>-U: deliver speech

    loop every question asked
        U->>+transcript: ask more questions 
        transcript-->>transcript: engineer prompt 
        transcript->>+gpt: call gpt with prompt
        gpt->>-transcript: prompt response
        transcript->>+db: save the transcript with time n id
        db->>-transcript: saved
        transcript-->>transcript: synthesize speech
        transcript->>-U: deliver speech
    end

```

## Get Notes Flow

```mermaid
sequenceDiagram
    actor U as User
    participant transcript as transcript service
    participant db as CosmosDB

    U->>+transcript: Get notes for transciption id
    transcript->>+db: Get all transcriptions 
    db->>-transcript: list of transcription
    transcript-->>transcript: sort/format to DTO
    transcript->>-U: deliver notes

```
