# Creating Guest URLs

The API allows developers to create guests with a custom survey/questionnaire
URL. This particular API provides the following endpoints:

* POST `/api/v0/companies/:company_id/distribute/guest_surveys[.json]`

Here `:company_id` is the ID of the company for which to create the guests.

## Parameters

When creating data the following POST parameters can be specified:

* `questionnaire_id`: the ID of the survey/questionnaire for which to create
  the guests.
* `guests`: an Array of guests to create with the guest fields that are also
  used when creating feedback requests.

## Example

Input (JSON is used for simplicity):

    {
        "questionnaire_id": "123123123abcdefexample",
        "guests":
        [
            {"name": "Example guests", "custom_id": "example-123"}
        ]
    }

The output in this case would look like the following:

    {
        "example-123": "http://feed.ba/ck-123123randomurl"
    }

