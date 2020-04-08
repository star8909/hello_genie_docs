## Usage

API URL - http://35.229.88.94/likely/api/v1 (POST)
Request body in JSON

200 response code if success. 

422 - failed with errors description will be return if request params is wrong

Another codes - failed

Auth key hardcoded:

``
m2g2jPhCeRBxfETTyJM38zmXGDsz8Mhz866qjE4uSdsf9ywShecf4FsgVUUfLG65
``
### Required params for all API methods
* key - auth key
* action - API action (add, status, service)
* service - Service ID (string)

### Add order method
Request params:
* action: "add"
* link - link to post or instagram nickname (followers service)
* quantity - Needed quantity (string)
* comment - comment if (comments only service = 3)

200 response code if success. Another codes - failed

#### Add order method request body examples

##### Likes (service = 1):
```json 
{
  "key": "%auth key%",
  "action": "add",
  "service": "1",
  "link": "https://www.instagram.com/p/B21bnK2Cw9U/",
  "quantity": "50"
}
```
##### Comments (service = 3):
```json 
{
  "key": "%auth key%",
  "action": "add",
  "service": "3",
  "link": "https://www.instagram.com/p/B21bnK2Cw9U/",
  "quantity": "50",
  "comment": "Sample comment"
}
```

##### Followers (service = 2):
```
{
  "key": "%auth key%",
  "action": "add",
  "service": "2",
  "link": "pespespes2020",
  "quantity": "50
}
```
#### Add order method response
Response fields:
* order - Order ID

Response body example:
```json 
{
  "order": "69dOAAYw8e0XHbZUBMpw"
}
```
### Get order status by id method
Request params:
* action: "status"
* order - Order ID (string)  

Request body example:
```json
{
  "key": "%auth key%",
  "action": "status",
  "order": "CPvCc8dyCFdsMekwYFgu"
}
```

Response fields:
* status - Order status (Pending, In Progress, Completed)

Response body example:
```json
{
  "status": "Pending"
}
```

If order not found 404 will be returned

### Get order status by multi id method
Request params:
* action: "status"
* orders - Order IDs coma separated 

Request body example:
```json
{
  "key": "%auth key%",
  "action": "status",
  "orders": "dsddwewewe,00Vvbrnp56fmILhL45pG,00Yzopp2219R3ehmsN8s"
}
```

Response fields:
* status - Order status (Pending, In Progress, Completed)

Response body example:
```json
{
  "dsddwewewe":{
    "error": "Not found"
  },
  "00Vvbrnp56fmILhL45pG":{
    "status": "Completed"
  },
  "00Yzopp2219R3ehmsN8s":{
    "status": "Completed"
  }
}
```

### Show services method
Request params:
* action: "services"

Request body example:
```json
{
  "key": "%auth key%",
  "action": "services"
}
```

Response body example:
```json
[
  {
    "service": 1,
    "name": "인스타그램 리얼 한국인 좋아요",
    "type": "Default",
    "category": "INSTAGRAM",
    "rate": "2",
    "min": "1",
    "max": "2000"
  },
  {
    "service": 2,
    "name": "인스타그램 리얼 한국인 팔로워",
    "type": "Default",
    "category": "INSTAGRAM",
    "rate": "10",
    "min": "1",
    "max": "50000"
  },
  {
    "service": 3,
    "name": "인스타그램 리얼 한국인 댓글",
    "type": "Custom Comments",
    "category": "INSTAGRAM",
    "rate": "15",
    "min": "1",
    "max": "100"
  }
]

```

## Installation

```bash
$ npm install
```

## Running the app

```bash
# developmentя могу приехать за тобой куда скажеш

# watch mode
$ npm run start:dev

# production mode
# instafree (port 8080)
$ npm run start:prod:instafree
# instafree (port 8081)
$ npm run start:prod:hello-genie

# pm2 one ny one
$ sudo pm2 start --name perfectpanel-api-instafree npm -- run start:prod:instafree
$ sudo pm2 start --name perfectpanel-api-hello-genie npm -- run start:prod:hello-genie

# pm2 run all once
$ sudo pm2 start ecosystem.config.js 
```
# Deploy
```bash
cd /var/www/perfect-panel-api
sudo pm2 deploy ecosystem.config.js production
```
# How to add new project


```
1. add firebase config key file to configs name %project-name$-firebase-key.json
2. add to scripts section in package.json:
3. "start:prod:%project-name%": "FIREBASE_KEY_FILE=%project-name%-firebase-key PORT=%new api port% node dist/main.js",
4. add new app to ecosystem.config.js    
5. add location to /etc/apache2/sites-available/000-default.conf
5.1 $ sudo vim /etc/apache2/sites-available/000-default.conf
5.2 add Location to VirtualHost sections
<Location /%project-name%>
    ProxyPass "http://localhost:%new api port% "
    ProxyPassReverse "http://localhost:%new api port% "
</Location>
6.restart apache and deploy changes in source code

Now you can use http://35.229.88.94/%project-name%/api/v1
```
