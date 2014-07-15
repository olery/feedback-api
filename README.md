# Feedback API

The Feedback API allows third-party developers to use data provided by
Feedback using a JSON only API. The base URL for all API endpoints is
https://feedback.olery.com/api/v0 and is SSL only.

## Sending Requests

Because the API is JSON only you should make sure that your HTTP requests are
set up correctly. Requesting JSON data can be done by either setting the
`Accept` header to `application/json` or by adding the `.json` extension to the
URL you are requesting.

## Authentication

Authentication is done using a so called authentication token and should be
specified in the `auth_token` GET parameter. Currently these tokens have to be
requested manually so send us an Email if you need one.


## What can you do with the API?

* [Send out post stay e-mails](guest_urls.md)
* [Route custom URLs with Guest info to specific
  questionnaires](custom_routes.md)
