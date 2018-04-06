## Request-Response Cycle
- How information gets passed around on the web
- Client-server concept

Request:                                                  Response:

<centre>
  
| CRUD          | TYPE             | HEADER | BODY |       | Status Code | HEADER | BODY       |
| ------------- |:----------------:| ------:| ----:|       | ----------- | ------:| ----------:|  
| Read Data     | Get + URL        |   Y    |   N  |       |      Y      |    Y   |      Y     |
| Create Data   | Post + URL       |   Y    |   Y  |       |      Y      |    Y   | sometimees |
| Update Data   | Patch(Put) + URL |   Y    |   Y  |       |      Y      |    Y   | sometimees |
| Delete Data   | Delete + URL     |   Y    |   N  |       |      Y      |    Y   | sometimees |

</center>

### Parts of a request
- URL
- Request method
- Headers
- Body (sometimes)

### Parts of a response
- Response status code
- Headers
- Body (most of the time)

## Request Methods
Since a URL represent the location of a particular set of information, the request methods let us indicate what we want to do with it.

Common request methods:

- GET
- POST
- PUT or PATCH
- DELETE

## CRUD
- Create
- Read
- Update
- Delete

## Response Codes
Response codes let the browser know the status of the request

Response code categories:

- 1xx: Informational (not really used)
- 2xx: Success
- 3xx: Redirection
- 4xx: User Error
- 5xx: Server Error
  - will be your best friend because it will indicate that there is a bug in your program

## Sinatra
Sinatra is a mini web-framework for Ruby we'll be using to learn the different parts of web development before we dive into Rails.

### Installing sinatra
You can speed up installation of gems on your computer by skipping the local installation of documentation for each gem:

```
$ echo "gem: --no-document" > ~/.gemrc
```

then...

```
$ gem install sinatra
```

- Routes
- Params
- Views/Templates
