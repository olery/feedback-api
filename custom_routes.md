# Custom Feedback Routing

It's possible to request "custom routes" for your hotel or hotel group. This
feature comes in handy when connecting an external marketing automation system
and /or PMS mailing system to Olery Feedback.

In order to use the Custom Feedback Routing, please contact Sales and/or Support
to enable it for you. 

Use Case
--------

You have 25 hotels and 3 questionnaires for each hotel. The most flexible option
would be to use the [Personalized Feedback URLs api](guest_urls.md). Which you
can use to create guest specific feedback urls on the fly.

However, some e-mail software doesn't play well with urls that are generated on
the fly. That's why we offer a second solution: **Custom Routing**.

Custom routing uses, like the name implies, custom questionnaire tags and
company ids. It takes some manual setup on our side, so please let us know what
your use case is well in advance.

How it works
------------

You setup 1 or more questionnaires for each company in your group or group of
hotels. 

Olery will:

* tag each of the questionnaires with a special "label"
* tag each company / hotel in the group with a "custom identifier" field. 
* create a custom url for you
* create a shared secret to protect the url information

For example, if you have 3 hotels, each with 2 questionnaires: 1 about the
overall experience of the guest, and 1 about the bar.

In your PMS system the hotels are known as "Hotel A", "Hotel B" and "Hotel C".

We will then tag Hotel A in Olery feedback with the ```hotela``` tag, Hotel B with
the ```hotelb``` tag and Hotel C with the ```hotelc``` tag. And each of the 6
questionnaires (3 x 2) with the appropriate values ```stay``` or ```bar```.

An example of a custom URL will then look like:

```
http://feed.ba/ck-thegroup?custom_id=hotela&tag=stay
```

This url would send the guest to a survey about "Hotel A" asking for his overall
experience during his or her stay.

Adding Guest Information
-----------------------

It is possible to add extra guest information to the custom url by sending it
extra arguments. The parameters you can add are:

* `guest-name`: the name of the guest
* `guest-reservation`: the reservation number of the guest
* `guest-title`: the title of the guest
* `guest-email` (required): the Email address of the guest
* `guest-arrival_date`: the arrival date of the guest, preferrably in the format
  `YYYY-mm-dd`
* `guest-departure_date`: the departure date of the guest, set to today by default.
  The format is the same as the `arrival_date` parameter
* `guest-reason`: the reason for the guest staying at the location
* `guest-room_number`: the number of the room the guest stayed in
* `guest-custom_id`: a custom ID to identify a guest, this field can be set to any
  value.


An example url would be:

```
http://feed.ba/ck-thegroup?custom_id=hotela&tag=stay&guest-name=Giannis&guest-arrival_date=2013-08-16
```

Securing the URL
----------------

When all information is "out in the open" in the URL it's easy for the guest to
"tweak" the information, which is probably not what you want. So we provide you
with a way to encrypt the parameter information in the ```i``` parameter
using a shared secret.

In order to do this you create a valid JSON object string and encrypt is using
standard AES encryption algorithm. In Ruby it would look something like this:

```ruby
require 'openssl'
require 'cgi'
require 'base64'
require 'json'

information = { custom_id: 'hotela',
                tag: 'stay',
                guest: {
                  name: 'Giannis',
                  arrival_date: '2013-08-16',
                  custom_fields: {
                    dining_preference: 'salmon'
                  }
                }
              }.to_json
              
cipher = OpenSSL::Cipher::AES.new(256, :CBC)
cipher.encrypt

## 256-bit key.
cipher.key = "the shared secret you got from Olery"
iv = cipher.random_iv

encoded_iv = URI.encode(Base64.encode64(iv))

encrypted_json = cipher.update(information) + cipher.final

url_friendly = URI.encode(Base64.encode64(encrypted_json))

url = URI.encode("http://feed.ba/ck-thegroup/?i=#{url_friendly}&iv=#{encoded_iv}")
```

In PHP it would look something like this:

```php
<?php


//should be a 256-bit size key.
$key = "the shared secret you got from Olery";

function addpadding($string, $blocksize = 16){
    $len = strlen($string);
    $pad = $blocksize - ($len % $blocksize);
    $string .= str_repeat(chr($pad), $pad);
    return $string;
}

$iv_size = mcrypt_get_iv_size(MCRYPT_CAST_256, MCRYPT_MODE_CBC);
$iv = mcrypt_create_iv($iv_size, MCRYPT_DEV_RANDOM);

$information = array('custom_id' => 'hotela', 
                     'tag' => 'stay',
                     'guest' => array('name' => 'Giannis',
                                      'arrival_date' => '2013-08-16',
                                      'custom_fields' => array('dining_preference' => 'salmon')));
$information_json = json_encode($information);
                                      
//Issue with PHP you have to set MCRYPT_RIJNDAEL_128 for 256 encryption.
$encrypted_json = base64_encode(mcrypt_encrypt(MCRYPT_RIJNDAEL_128, $key, addpadding($information_json), MCRYPT_MODE_CBC, $iv));

$url_friendly = urlencode($encrypted_json);

$encoded_iv = urlencode(base64_encode($iv));

$url = "http://feed.ba/ck-thegroup/?i=$url_friendly&iv=$encoded_iv";

echo $url;                                     

?>
```

Bonus
-----

As you might've noticed in the code above: when using a json hash as an argument
you can also add custom fields to your guest information. Something which is not
possible when using plain URL parameters.
