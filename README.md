feedback-api
============


Feedback Request API
--------------------


POST - `https://feedback.olery.com/api/v0/distribute/email`

```ruby

{ :hotel_id=>123,
  :template_id=>asdf134asdf,
  :guests => [
    {:name=>"name", :email=>"email*", :room=>"", :departure=>"", :arrival=>"", etc.},
    {:email=>"foo@example.com"}
  ]
}
```

RESPONSE - 201


GET - `https://feedback.olery.com/api/v0/distribute/email`
RESPONSE - JSON lijst van mailtjes


GET - `https://feedback.olery.com/api/v0/distribute/email/templates`
RESPONSE - JSON lijst van namen en ids
