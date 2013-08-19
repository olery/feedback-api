# Distributing Feedback Requests a.k.a Post-stay emails

The API allows developers to list existing feedback requests, Email templates
as well as allowing them to create new feedback requests. This endpoint
provides the following 3 requests:

* `GET /api/v0/companies/:company_id/distribute/emails[.json]`
* `GET /api/v0/companies/:company_id/distribute/emails/templates[.json]`
* `POST /api/v0/companies/:company_id/distribute/emails[.json]`

Here `:company_id` is the ID of the company.

## Parameters

When retrieving a collection of data you can set the following parameters:

* limit: the maximum amount of rows to retrieve, set to 50 by default
* offset: a custom row offset to use, useful for paginating content

## Creating Feedback Requests

When creating new feedback request you are required to specify the following
POST parameters:

* `email_template_id`: the ID of the Email template to use
* `questionnaire_id`: the ID of the survey/questionnaire for which to send the
  feedback request
* `guests`: an array of guests to send the feedback request to

Each item in the `guests` array should be a key/value pair that can contain the
following fields:

* `name`: the name of the guest
* `title`: the title of the guest
* `email` (required): the Email address of the guest
* `arrival_date`: the arrival date of the guest, preferrably in the format
  `YYYY-mm-dd`
* `departure_date`: the departure date of the guest, set to today by default.
  The format is the same as the `arrival_date` parameter
* `reason`: the reason for the guest staying at the location
* `room_number`: the number of the room the guest stayed in
* `custom_fields`: a key/value pair containing custom keys and values
* `custom_id`: a custom ID to identify a guest, this field can be set to any
  value.


