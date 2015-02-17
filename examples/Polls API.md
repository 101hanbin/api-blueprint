FORMAT: 1A

# Polls

Polls is a simple web service that allows consumers to view polls and vote in them. You can view this documentation over at [Apiary](http://docs.pollsapi.apiary.io).

# Polls API Root [/]

This resource does not have any attributes. Instead it offers the initial API affordances in the form of the links in the JSON body.

It is recommend to follow the “url” link values, [Link](https://tools.ietf.org/html/rfc5988) or Location headers where applicable to retrieve resources. Instead of constructing your own URLs, to keep your client decoupled from implementation details.

## Retrieve the Entry Point [GET]

+ Response 200 (application/json)

        {
            "questions_url": "/questions{?page}"
        }

## Group Question

Resource related to questions in the API.

## Question [/questions/{question_id}]

A Question object has the following attributes:

- question
- published_at - An ISO8601 date when the question was published.
- url
- choices - An array of Choice objects.

+ Parameters

    + question_id (number) ... ID of the Question in form of an integer

### View a question detail [GET]

+ Response 200 (application/json)

                {
                    "question": "Favourite programming language?",
                    "published_at": "2014-11-11T08:40:51.620Z",
                    "url": "/questions/1",
                    "choices": [
                        {
                            "answer": "Swift",
                            "url": "/questions/1/choices/1",
                            "votes": 2048
                        }, {
                            "answer": "Python",
                            "url": "/questions/1/choices/2",
                            "votes": 1024
                        }, {
                            "answer": "Objective-C",
                            "url": "/questions/1/choices/3",
                            "votes": 512
                        }, {
                            "answer": "Ruby",
                            "url": "/questions/1/choices/4",
                            "votes": 256
                        }
                    ]
                }

## Choice [/questions/{question_id}/choices/{choice_id}]

+ Parameters

    + question_id (number) ... ID of the Question in form of an integer
    + choice_id (number) ... ID of the Choice in form of an integer

### Vote on a Choice [PUT]

This action allows you to vote on a question's choice.

+ Response 201

    + Headers

            Location: /questions/1

## Questions collection [/questions{?page}]

Again, instead of constructing the URLs for the next page. It is **highly** recommended that you follow the `next` link header in the response.

+ Parameters

    + page (optional, number) ... The page of questions to return

### List all questions [GET]

+ Response 200 (application/json)

    + Headers
    
            Link: </questions?page=2>; rel="next"

    + Body

            [
                {
                    "question": "Favourite programming language?",
                    "published_at": "2014-11-11T08:40:51.620Z",
                    "url": "/questions/1",
                    "choices": [
                        {
                            "answer": "Swift",
                            "url": "/questions/1/choices/1",
                            "votes": 2048
                        }, {
                            "answer": "Python",
                            "url": "/questions/1/choices/2",
                            "votes": 1024
                        }, {
                            "answer": "Objective-C",
                            "url": "/questions/1/choices/3",
                            "votes": 512
                        }, {
                            "answer": "Ruby",
                            "url": "/questions/1/choices/4",
                            "votes": 256
                        }
                    ]
                }
            ]

### Create a new question [POST]

You can create your own question using this action. It takes a JSON dictionary containing a question and a collection of answers in the form of choices.

- question (string) - The question
- choices (array[string]) - A collection of choices.

+ Request (application/json)

            {
                "question": "Favourite programming language?",
                "choices": [
                    "Swift",
                    "Python",
                    "Objective-C",
                    "Ruby"
                ]
            }

+ Response 201

    + Headers

            Location: /questions/1
    
    + Body

                {
                    "question": "Favourite programming language?",
                    "published_at": "2014-11-11T08:40:51.620Z",
                    "url": "/questions/1",
                    "choices": [
                        {
                            "answer": "Swift",
                            "vote_url": "/questions/1/choices/1",
                            "votes": 0
                        }, {
                            "answer": "Python",
                            "vote_url": "/questions/1/choices/2",
                            "votes": 0
                        }, {
                            "answer": "Objective-C",
                            "vote_url": "/questions/1/choices/3",
                            "votes": 0
                        }, {
                            "answer": "Ruby",
                            "vote_url": "/questions/1/choices/4",
                            "votes": 0
                        }
                    ]
                }
