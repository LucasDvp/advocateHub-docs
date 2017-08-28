## Microsoft Advocate Hub RESTful API Notes

This document is ***INTERNAL CONFIDENTIAL***

Basically, this system now contains two collections ( **meetings & advocators** ), each collection we treat as entity, so they can linked by each other (**n <-> n**), or by other entities may add in the future.

### Basic field of collections

We designed to use **Mongo DB**, so the **data extension & post data structure** would be simple and flexible, so this doc only show the basic info of two entities.

#### 1.  meetings

```json
{
  	"_id" : ObjectId("59817aebf19f967aab37eb30"),
  	"advocatorId" : "123456789",
  	"description" : "***",
 	"name" : "***",
  	"location" : "Redmond",
  	"date" : ISODate("2017-08-03T04:00:48.000Z"),
  	"pptLink": "https://***",
  	"videoLink": "https://***",
  	"type": "online",
  	"tags": [
      "Azure",
      "C#"
    ],
  	"popularity": 90
}
```


#### 2. advocators

 ```json
{
  	"_id" : ObjectId("598199a8f19f96165d394dd6"),
    "location" : "***",
    "bio" : "",
    "name" : "***",
    "alias" : "***",
    "id" : "***",
    "website" : null,
    "url" : "https://twitter.com/***",
    "avatar" : "https://abs.twimg.com/sticky/default_profile_images/default_profile_normal.png",
    "timezone" : null,
    "language" : "en",
  	"githubAccount" : "***"
}
 ```

### APIs now we provide

The baseUrl is `http://40.83.190.18:13888`

- #### User Login

  - ##### Introduction

    > Dealing with the advocator login

  - ##### URL

    ```http
    /advocator/login/<advocatorId> 
    ```

  - ##### Method

    > GET

  - ##### Args

    |    Name     | Required |  Type  |           Notes           |
    | :---------: | :------: | :----: | :-----------------------: |
    | advocatorId |   YES    | String | *id* in advocators entity |

  - ##### Return Sample

    ```json
    {
        "errorMessage": "",
        "status": 200,
        "data": {
            "popularity": 90,
            "homePage": "http://***",
            "githubAccount": "",
            "avatar": "https://pbs.twimg.com/profile_images/727622460934230016/BbS4_n_m_bigger.jpg",
            "id": "123456789",
            "alias": "***",
            "twitterAccount": "***",
            "name": "***",
            "tags": [
                "Azure",
                "C#"
            ]
        },
        "timestamp": 1501748008907
    }
    ```

- #### User Register

  - ##### Introduction

    > Dealing with the advocator register with twitter

  - ##### URL

    ```http
    /advocator/login
    ```

  - ##### Method

    > POST

  - ##### Args

    | Name | Required | Type |                  Notes                   |
    | :--: | :------: | :--: | :--------------------------------------: |
    |  /   |   YES    | JSON | **"id"** is required in json object, like advocators entity |

  - ##### Return Sample

    ```json
    {
        "errorMessage": "",
        "status": 200,
        "data": true,
        "timestamp": 1501748008907
    }
    ```

- #### Advocators

  - ##### Introduction

    > Get all the advocators in database

  - ##### URL

    ```http
    /advocators 
    ```

  - ##### Method

    > GET

  - ##### Args

    No

  - ##### Return Sample

    ```json
    {
        "errorMessage": "",
        "status": 200,
        "data": [
            {
                "popularity": 90,
                "homePage": "***",
                "githubAccount": "***",
                "avatar": "https://pbs.twimg.com/profile_images/727622460934230016/BbS4_n_m_bigger.jpg",
                "id": "123456789",
                "alias": "***",
                "twitterAccount": "***",
                "name": "***",
                "tags": [
                    "Azure",
                    "C#"
                ]
            },
          ...
        ],
    	"timestamp": 1501748506827
    }
    ```

- #### Meetings

  - ##### Introduction

    > Get all the meetings in database

  - ##### URL

    ```http
    /meetings
    ```

  - ##### Method

    > GET

  - ##### Args

     No

  - ##### Return Sample

    ```json
    {
        "errorMessage": "",
        "status": 200,
        "data": [
            {
                "advocator": {
                    "popularity": 90,
                    "homePage": "***",
                    "githubAccount": "***",
                    "avatar": "https://pbs.twimg.com/profile_images/727622460934230016/BbS4_n_m_bigger.jpg",
                    "id": "123456789",
                    "alias": "***",
                    "twitterAccount": "***",
                    "name": "***",
                    "tags": [
                        "Azure",
                        "C#"
                    ]
                },
                "advocatorId": "123456789",
                "location": "***",
                "name": "***",
                "_id": "59817aebf19f967aab37eb30",
                "date": 1501732848000,
                "description": "***"
            },
          ...
        ],
    	"timestamp": 1501748506827
    }
    ```
    This will return both meeting's information and advocator's

- #### Create meeting

  - ##### Introduction

    > Create or update a meeting in admin(advocator) page

  - ##### URL

    ```http
    /meeting/create
    ```

  - ##### Method

    > POST

  - ##### Args

    | Name | Required | Type |          Notes           |
    | :--: | :------: | :--: | :----------------------: |
    |  \   |   YES    | JSON | like meetings collection |

  - ##### Return Sample

    - if **_id** provided, it means meeting already exist, then update
     ```json
    {
        "errorMessage": "",
        "status": 200,
        "data": true,
        "timestamp": 1501748008907
    }
     ```
    - if not, then create a new one

    ```json
    {
        "errorMessage": "",
        "status": 200,
        "data": {qrcode_file_link},
        "timestamp": 1501748008907

    }
    ```

  - ##### Return value explain

    if create a new meeting, api will return a String value, it's a qrcode_file_relative_link

    | Name | Type   | Notes                                    |
    | ---- | ------ | ---------------------------------------- |
    | data | String | the return value is the argument of `/qrcode/<filename` |

- #### Get Qrcode

  - ##### Introduction

    > Get the qrcode of the new meeting

  - ##### URL

    ```http
    /qrcode/<filename> 
    ```

  - ##### Method

    > GET

  - ##### Args

    |   Name   | Required |  Type  |                  Notes                  |
    | :------: | :------: | :----: | :-------------------------------------: |
    | filename |   YES    | String | returned value by `/meeting/create` api |

  - ##### Return Sample

    >  svg file ( mimetype='image/svg+xml' )

- #### Get Meeting

  - ##### Introduction

    > Get a specific meeting info with advocator info

  - ##### URL

    ```http
    /meeting/<meetingId> 
    ```

  - ##### Method

    > GET

  - ##### Args

    |   Name    | Required |  Type  |          Notes           |
    | :-------: | :------: | :----: | :----------------------: |
    | meetingId |   YES    | String | *_id* in meetings entity |

  - ##### Return Sample

    ```json
    {
        "errorMessage": "",
        "status": 200,
        "data": {
            "advocator": {
                "popularity": 90,
                "homePage": "***",
                "githubAccount": "",
                "avatar": "https://pbs.twimg.com/profile_images/727622460934230016/BbS4_n_m_bigger.jpg",
                "id": "123456789",
                "alias": "***",
                "twitterAccount": "***",
                "name": "***",
                "tags": [
                    "Azure",
                    "C#"
                ]
            },
            "advocatorId": "123456789",
            "location": "***",
            "name": "***",
            "_id": "59817aebf19f967aab37eb30",
            "date": 1501732848000,
            "description": "***"
        },
        "timestamp": 1501750005940
    }
    ```

- #### Error Info

  all the error return is like

  ```json
  {
      "errorMessage": "***",
      "status": 404,
      "data": {},
      "timestamp": 1501750097494
  }
  ```

  ​

  ​