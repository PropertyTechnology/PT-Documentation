# Consumer Enquiries API
Read more about LeadPro at <http://propertytechnology.co.uk>

## API
The production version of the API is available at:

`https://api.propertytechnology.co.uk`

You will need to email <help@propertytechnology.co.uk> to request a live `office_authorization_token`

## Authentication
The following Authorization Bearer Token must be set on each request which requires office authentication.

```
Authorization Bearer [office_authorization_token]
```

Where `office_authorization_token` is a unique access code of an office.

## Endpoints

### Show a specific enquiry
The request requires authentication (see above).

GET `/consumer_enquiries/:enquiry_id`

Where `enquiry_id` is the ID of a specific consumer enquiry

### List enquiries
The request requires authentication (see above).

GET `/consumer_enquiries?:filters`

Where `filters` is a set of filtering parameters, for example:

* `q[created_at_gt]=2017-10-25T10:23:56`

  List all enquiries created after the `date` (YYYY-MM-DDTHH:MM:SS)

* `q[created_at_lt]=2017-10-25T10:23:56`

  List all enquiries created before the `date` (YYYY-MM-DDTHH:MM:SS)

### Updates to Enquiries
You should periodically check for updates to enquiries. Some people fill out qualification questionnaires up to 72hrs later.

### Responses
The response to a request will conform with the JSON API specification.

#### Attributes
The following attributes will returned for each consumer enquiry:

* `property_address` (string) - e.g. 40 Islington High St
* `property_reference` (string, optional) - e.g. 123456
* `from_portal` (string) - e.g. Rightmove. (Homepage for enquires from agency homepage)
* `kind` (string) - Will be either `let` or `sale`
* `name` (string) - e.g. John Smith
* `email` (string) - e.g. j.smith@example.com
* `phone` (string, optional) - e.g. +442078927246
* `message` (string, optional) - e.g. Hello, when could i view?
* `created_at` (string) - e.g. 2017-10-16 10:36:09 +0100
* `questions_answers` (array[dict], optional) - All questions and answers for this consumer

#### On Success
Success responses will follow this format:
<http://jsonapi.org/examples/#sparse-fieldsets>

```json
{
    "data": [
        {
            "type": "consumer_enquiries",
            "id": "a93882e3b5",
            "attributes": {
                "property_address": "40 Islington High St",
                "property_reference": "123456",
                "from_portal": "Rightmove",
                "kind": "let",
                "name": "John Smith",
                "email": "j.smith@example.com",
                "phone": "+442078927246",
                "message": "Hello, when could i view?",
                "created_at": "2017-11-01T15:40:27",
                "questions_answers": [
										{
                        "question": "Do you have any pets?",
                        "question_type": "yes_no",
                        "answer": "No",
                        "answer_type": "boolean"
                    },
                    {
                        "question": "When is the best time for us to contact you?",
                        "question_type": "short_text",
                        "answer": "After 6pm",
                        "answer_type": "text"
                    }
                ]
            }
        }
    ]
}
```

#### On Failure
Failure responses will follow this format:
<http://jsonapi.org/examples/#error-objects>

```json
{
    "errors": [
        {
            "status": 404,
            "source": {
                "parameter": "agency_username"
            },
            "title": "Agency Not Found",
            "detail": "Agency not found with agency_username: 'invalid-agency'"
        }
    ]
}
```

```json
{
    "errors": [
        {
            "status": 422,
            "source": {
                "pointer": "/data/attributes/kind"
            },
            "title": "Invalid Attribute",
            "detail": "Kind is not included in the list, can't be blank"
        }
    ]
}
```

