# Τεχνικό documentation

## Authentication

Για να επιτευχθεί σωστή επικοινωνία με τα REST Services του e-table.gr, θα πρέπει σε κάθε request να υπάρχουν τα παρακάτω HTTP Headers: 

- `X-Access-Token <token>`
- `Accept application/json; version=public/v2`

Η version 2 του API υποστηρίζει τα παρακάτω response types, τα οποία θα πρέπει να υπάρχουν στον Accept Header:

- `application/json`
  
Σε περίπτωση που δεν έχει γίνει σωστά η παραπάνω διαδικασία, ισχύουν τα παρακάτω responses:

- Status: `406 Not Acceptable`
- Status: `401 Unauthorized`

    Response: `{"error": true, "type": "Authentication", "message": "Your access token is either missing or incorrect. Please check the X-Access-Token header and try again." }`

Υπόλοιπα Statuses ανα περίπτωση:

- Status: `200: OK`
- Status: `201: Resource Created`
- Status: `400: Bad Request`
- Status: `404: Not Found`
- Status: `409: Conflict`
- Status: `503: Server Unable To Handle Request`

## Update X-Access-Token

`PUT https://www.e-table.gr/public/token`

### Response

- Status: `200 OK`
- Body: `{"public/token": true, "message": "Το Access Token αποθηκεύτηκε", "token": s7WBUR1wUjljDPKgxD0duyFOqEQLnDalc7ysJ2ZlTmwjck1o3CyMVV9N39wcUntG1}`


## Localization

Γλώσσες: για responses σε επιλεγμένη γλώσσα, πρέπει να προστεθεί στο request url, το αντίστοιχο language code

- Ελληνικά: δεν απαιτείται
- English: `en` 

    π.χ `https://www.e-table.gr/en/public/start` ή `https://www.e-table.gr/en/public/store`   

## Get starting variables

 `GET https://www.e-table.gr/public/start`  

### Parameters

- city (default: `attiki`)

### Optional parameters

- limit: override default limit of 100 records per request (eg `limit=250`)
- page: based on prev/next records of pagination response (eg `page=4`)

### Response fields

- pagination
- max_pages
- max_records
- prev
- next
- langs
- top_locations
- sublocations
- filters->zones
- filters->cuisines
- filters->styles
- filters->suggestions
- filters->services
- filters->ambiences
- filters->paymethods
- filters->costs
- filters->persons
- stores

### Example

	curl
		'https://www.e-table.gr/en/public/start/'
		-H 'X-Access-Token: <token>'
		-H 'Accept: application/json; version=public/v2'


## Get store object

 `GET https://www.e-table.gr/public/store`  

### Parameters

 - store_id OR slug

### Response fields

- store
- store_id
- slug
- title
- description
- menu
- working_hours
- loyalty
- discount
- cover
    - small
    - medium
    - big
- location
    - path
    - title
    - address
    - coordinates
- urls
    - website
    - youtube
    - facebook
    - virtual_tour
- cost
    - id
    - slug
    - title
    - symbol
- style
    - id
    - slug
    - title
- cuisines
    - id
    - slug
    - title
    - ...
- suggestions
    - suggest_id
    - suggest
    - ...
- services
    - service_id
    - service
    - ...
- ambiences
    - ambience_id
    - ambience
    - ...
- paymethods
    - paymethod_id
    - paymethod
    - ...
- rating
    - total
    - reviews
    - food
    - service
    - env
    - price
- images
    - image
    - thumb
    - ...
- updated (ημ/νια τελευταίας τροποποίησης)

### Example

	curl
		'https://www.e-table.gr/en/public/store/?store_id=52'
		-H 'X-Access-Token: <token>'
		-H 'Accept: application/json; version=public/v2'

	curl
		'https://www.e-table.gr/en/public/store/?slug=pommery-ice'
		-H 'X-Access-Token: <token>'
		-H 'Accept: application/json; version=public/v2'

## Get a list of stores by searchable criteria

`GET https://www.e-table.gr/public/store/search`

### Optional parameters

- city - default: `attiki`
- location - eg `attiki.kentrikanotiaproastia.alimos`
- attributes (+ separated) - eg `elliniki+eos-16+me-thea`
- availability parameters
    - date - eg `2018-08-28`
    - zone
    - persons
- proximity parameters
    - dlat - client's latitude
    - dlng - client's longitude
    - distance - in Km
- pagination parameters
    - page - default: `1`
    - limit - default: `100`

### Response fields

- pagination
    - max_pages
    - max_records
    - prev
    - next
- stores

### Example

	curl
		'https://www.e-table.gr/en/public/store/?store_id=52'
		-H 'X-Access-Token: <token>'
		-H 'Accept: application/json; version=public/v2'


## Get store booking schedule

`GET https://www.e-table.gr/public/store/schedule`

### Parameters

- `store_id` OR `slug`
  
### Response fields

- schedule
    - booking_date
    - zone
    - persons
    - option_id
    - option_title
    - option_subtitle
    - hours (διαθέσιμες ώρες προσέλευσης)
        - 20:00
        - 20:30
        - ...
    - ...

### Example

	curl
		'https://www.e-table.gr/en/public/store/schedule/?store_id=52'
		-H 'X-Access-Token: <token>'
		-H 'Accept: application/json; version=public/v2'

## Get a user object (aka login)

`GET https://www.e-table.gr/public/user`

### Parameters

- email
- pass
 
### Response fields

- user
    - user_id
    - email
    - firstname
    - lastname
    - gender
    - created

## Create a user object (aka register)

`POST https://www.e-table.gr/public/user`

### Parameters

- email
- pass
- firstname
- lastname
- location_id (απο top_locations)

### Optional parameters:

- gender
- birthdate (eg `2018-08-28`)
- city
- area
- phone
- test_mode (`test_mode=true` applies to `public/activity` endpoint)

### Response fields

- user
    - user_id
    - email
    - firstname
    - lastname
    - gender
    - created

## Add new reservation

`POST https://www.e-table.gr/public/reservation`

### Parameters

- store_id
- User Parameters
    - user_id
    - email (if user_id is unknown)
- sms_code (if user phone is not validated)
- date (eg `2018-08-28`)
- time (eg selection by `public/store/schedule->hours`)
- option_id (selection by `public/store/schedule->option_id`)
- persons (selection by `public/store/schedule->persons`)
- name
- phone_code (default: `30`)
- phone

### Optional parameters

- comment
- test_mode (`test_mode=true` applies to `public/activity` endpoint)

### Response

#### Validation required response

- Status: `200 OK`
- Response: `{"public/reservation": false, "message": "Ο 4ψήφιος κωδικός αριθμός, στάλθηκε στο κινητό τηλέφωνο", "sms_code": 3239}`

#### Reservation created response

- Status: `201 Created`
- Response: `{"public/reservation": true, "message": "Η κράτηση αποθηκεύτηκε", "user_id": 1111, "code": "GHVQD"}`

#### Λόγοι μη επιτυχημένης καταχώρησης κράτησης

Η καταχώρηση κράτησης μπορεί να αποτύχει στις εξής ειδικές περιπτώσεις κατά τις οποίες θα πρέπει να εμφανίζεται σχετικό μήνυμα στον χρήστη:

- Όταν καταχωρηθεί λάθος SMS ή έχει γίνει session timed out. Στην περίπτωση αυτή θα πρέπει να εμφανίζεται το παρακάτω μήνυμα:

    `Λανθασμένος κωδικός SMS`

- Όταν ο χρήστης έχει ήδη καταχωρήσει 3 ενεργές μελλοντικές κρατήσεις - αυτό συμβαίνει για προστασία από κακόβουλους χρήστες ή ρομπότ. Στην περίπτωση αυτή θα πρέπει να εμφανίζεται το παρακάτω μήνυμα:
 
    `Δεν μπορείτε να έχετε περισσότερες από 3 ενεργές κρατήσεις εστιατορίων`

- Όταν ο χρήστης έχει ήδη ενεργή μελλοντική κράτηση στην ίδια ή σε προηγούμενη ή μεταγενέστερη ζώνη ώρας κατά την ίδια μέρα - αυτό συμβαίνει για να αποτρέψουμε το σενάριο να κάνει ένας χρήστης πολλαπλές κρατήσεις την ίδια ώρα με σκοπό να πάει στο εστιατόριο που θα αποφασίσει τελευταία στιγμή Στην περίπτωση αυτή θα πρέπει να εμφανίζεται το παρακάτω μήνυμα:

    `Δεν μπορείτε να έχετε περισσότερες από 1 ενεργές κρατήσεις στην ίδια ή σε διαδοχικές ζώνες ώρας`

- Για οποιαδήποτε άλλο λόγο (π.χ. γιατι εξαντλήθηκε το τελευταίο τραπέζι μέχρι να καταχωρησει τον κωδικό sms). Στην περίπτωση αυτή θα πρέπει να εμφανίζεται το παρακάτω γενικό μήνυμα:

    `Ουυπς... κάτι περίεργο συνέβη. Μας συγχωρείτε. Παρακαλώ κάνετε refresh και δοκιμάστε ξανά`

### Example

	curl
		'https://www.e-table.gr/en/public/reservation/'
		-d 'store_id=52&email=email%40example.com&date=2018-08-31&time=20%3A15&persons=2&name=Firstname+Lastname&phone_code=30&phone=6900000000&option_id=1&test_mode=true'
		-X POST
		-H 'X-Access-Token: <token>'
		-H 'Accept: application/json; version=public/v2'

## Get completed reservations status

`GET https://www.e-table.gr/public/reservations/report`

### Parameters

- page (eg `page=4`)
- limit (eg `limit=250`, default `limit=100`)

### Optional parameters

- start_date (format `yyyy-mm-dd`, απο ημερ/νια καταχώρησης κράτησης)
- stop_date  (format `yyyy-mm-dd`, εως ημερ/νια καταχώρησης κράτησης)

### Response fields

- range
- max_pages
- max_records
- prev
- next
- reservations
    - store_id
    - store_title
    - code
    - datetime
    - reward
    - fullname
    - status

## Remove reservations and users created for testing purposes

`DELETE https://www.e-table.gr/public/activity`

Calls to either POST public/reservation or POST public/user must be followed by test_mode=true param 

### Response

- Status: `200 OK`
 - Body: `{"public/activity": true, "affiliate" => affiliate_name, "message": "Public API v2 records removed", "purged": {"users": 3, "reservations": 15}}`