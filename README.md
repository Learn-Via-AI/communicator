
# Learn Via AI
![Logo](https://github.com/Learn-Via-AI/communicator/blob/master/logo.png)


## Architecture
Below is the architecture of the project.
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
