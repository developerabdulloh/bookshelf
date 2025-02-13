# The Great Bookshelf of Udacity

This project is a virtual bookshelf for Udacity students. Students are able to add books to the bookshelf, give them rating, update the rating and search through their book lists. 
As a part of the FullStack Nanodegree, it servec as a practice module for lessaons: API Development and Documentation. By building this project, students learn and develop their skills structuring and implementing formatted API endpoints, that leverage knowledge of HTTP and API best practices. 


All backend code follows [PEP8 style guidelines](https://www.python.org/dev/peps/pep-0008/). 

# Getting Started

## Local Development 
The instructions below are meant for the local setup only. The classroom workspace is already set for your to start practicing. 

## Backend

#### Pre-requisites
* Developers using this project should already have Python3, pip and node installed on their local machines.
## Install requirements
To install all reuirements, navigate to the `/backend` folder and run the folllowing command:
```bash
    pip install -r requirements.txt
```
requirements.txt is a file which includes all required modules and packages for API

Then to run application run the following commands:
```bash
    export FLASK_APP=flaskr
    export FLASK_ENV=development
    flask run
```
These commands put the application in development and directs our application to use `__init__.py` file in our flaskr folder. Wroking in development mode shows an interactive debugger in the console and restarts the server whenever changes are made.
If you use Window, look for the commands in the [Flask documentation](http://flask.pocoo.org/docs/1.0/tutorial/factory/).

The applcation by default runs on `http://127.0.0.1:5000/`.



## FrontEnd

From the `frontend` folder, run the following commands to start the client:
```
    npm install // install all requirements
    npm start
```
By default, the frontend will run on localhost:3000.


## Tests
In order to run tests, navigate to the `/backend` folder and run the following commands: 
```bash
psql postgres
dropdb bookshelf_test
createdb bookshelf_test
\q
psql bookshelf_test < books.psql
python test_flaskr.py
```


# API Reference

## Getting Started
* Base URL: At present this app can be run locally and it hosted by default. Default local url: ` http://127.0.0.1:5000 `
* Authentication: This version of API does not require :D

<br>

* We use Postman to send requests. <img src="http://cdn.auth0.com/blog/postman-integration/logo.png" alt="postman" width="21px" height="21px"> Another tool to send requests is curl.


## Error Handling
Errors are returned as JSON objects in folloving format:
```json
    {
        "error": <error_code>,
        "message": <error_message>,
        "success": false
    }
```

### The API will return three types of errors: <br>
* 400: Bad request 
* 404: Resource not found 
* 405: Method now allowed 

## Endpoints

## `GET /books`
* ### General:
    * Get All books
    * Results are paginated in groups of 8. Include a request argument to choose page, starting from 1.
* ### Example
    * Request: ` http://127.0.0.1:5000/books ` <br>
    * Response:
    ```json
        {
            "books": [
                {
                    "author": "Kel Newport",
                    "id": 1,
                    "rating": 6,
                    "title": "Attention"
                }
            ],
            "success": true,
            "total_books": 2
        }
    ```

    * To retrieve books from other pages, you send this request, assign the page parameter to the URL, and assign its value to the page number: ` http://127.0.0.1:5000/books?page=2 `
    ## `GET /book?page=2`
    This endpoint returns all books from the second page, if the second page doesn't exists it will return message "books not found"
    
<br>

## `GET /books/{book_id}`
* ### General:
    * Get book with id. Book id given in the URL parametres
* ### Example
    * Requests: ` https://127.0.0.1:/5000/books/1 ` - this endpoint returns book which id is 10
    * Response:
    ```json
        {
            "author": "Kel Newport",
            "id": 1,
            "rating": 6,
            "success": true,
            "title": "Attention"
        }
    ```

* ### ⚠️ Warning
    * If  in your request give a book id which does not exists in database, API returns error with message "books not found"
    * Example:
        * Request: ` http://127.0.0.1:5000/books/121221212 `
        * Response:
        ```json
            {
                "message": "books not found",
                "success": false
            }
        ```
## POST /books/search
* ### General:
    * Search books via title. This endopint return all defined books as searching resulsts
    * You should request to this endpoint with `POST` method. ⚠️ Request  should include JSON data which includes 'search' key and it value
* ### Example:
    * Request: ` POST/ 127.0.0.1:5000/books/search `
        * Request body:
        ``` json
            {
                "search": "attention"
            }
        ```
        * Response:
        ```json
            {
                "books": [
                    {
                        "author": "Kel Newport",
                        "id": 1,
                        "rating": 6,
                        "title": "Attention"
                    }
                ],
                "success": true,
                "total_books": 1
            }
        ```
* ### ⚠️ Warning
    * If you send empty request or request which not includes important value, API returns "400" error with message "bad request"
    * Example:
        * Request: ` POST/ http://127.0.0.1:5000/books/search `
            * Request body:
            ```json
                {
                    "uncorrect_key": "uncorrect_value"
                }
            ```
            or empty request body:
            ```json
                {}
            ```
            * Response:
            ```json
                {
                    "error": 400,
                    "message": "bad request",
                    "success": false
                }
            ```      
      
## POST /books
* ### General:
    * Create a new book
    * You should send request with `POST` method. Your request should include JSON data about new book


    * JSON data should include:
    ```
        "author" - book author
        "title" - book title
        "rating" - book rating
    ```
    * Then API reutrns data which includes:
    ```
        "books" - all books list,
        "created" - id of new book,
        "success" - True,
        "total_books" - number of all books
    ```


* ### Example:
    * Request: ` POST/ http://127.0.0.1:5000/books `
        * Request body:
        ```json
            {
                "title": "The Great Alone",
                "author": "Kristin Hannah",
                "rating": "4"
            }
        ```
    * Response:
    ```json
        {
            "books": [
                {
                    "author": "Kel Newport",
                    "id": 1,
                    "rating": 6,
                    "title": "Attention"
                },
                {
                    "author": "Kristin Hannah",
                    "id": 2,
                    "rating": 4,
                    "title": "The Great Alone"
                }
            ],
            "success": true,
            "total_books": 2
        }
    ```

* ### ⚠️ Warning
    * If in your request body, in JSON data not includes one of required parametres, API returns "400" error with message 'bad request'
    * Example:
        * Request ``` POST/ http://127.0.0.1:5000/books ```
            * Request body:
            ```json
                {
                    "title": "no_title"
                }
            ``` 
            - in this request required "author" parameter

            or

            ```json
                {
                    "title": "title_of_book",
                    "author": ""
                }
            ``` 
            - in this request "author" parameter is empty

            or 

            ```json
                {
                    "author": "author_of_book"
                }
            ``` 
            - "title" parameter is required

            ⚠️ "rating" parameter is not required, by default its value equals to zero.
            If "rating" not zero, you can include rating parameter to JSON data

        * Response:
        ```json
            {
                "error": 400,
                "message": "bad request",
                "success": false
            }
        ```

    * If you enter the book_id to url, API returns "405" error with message "method not allowed"
    * Example:
        * Request: ` POST/ http://127.0.0.1:5000/books/1 `
            * Request body:
            ```json
                {
                    "title": "The Great Alone",
                    "author": "Kristin Hannah",
                    "rating": "4"
                }
            ```
        * Response:
        ```json
            {
                "error": 405,
                "message": "method not allowed",
                "success": false
            }
        ```



## PATCH /books/{book_id}
* ### General:
    * Update a book:
        * Update only "title"
        * Update only "author"
        * Update only "rating"
        * Update all data
    * You should enter id of book in URL as url parameter
    * Your should send Request with method PATCH including JSON data 
    * Or your request can be empty (if request body empty, the book will not be updated)
    * API responses JSON data which includes:
    ```
        "id" - the id of updated book
        "success" - True 
    ```
* ### Example:
    * Update only "rating"
        * Request: ` PATCH/ http://127.0.0.1:5000/2 `
            * Request body:
            ```json
                {
                    "rating": "1"
                }
            ```
            updates "rating" of the book which id 2

        * Response:
        ```json
            {
                "id": 2,
                "success": true
            }    
        ```
        ⚠️ You can see updates by sending GET request to endpoint "/books"
        or by sending GET request to endpoint "/books/2"


    * Update only "title"
        * Request: ` PATCH/ http://127.0.0.1:5000/2 `
            * Request body:
            ```json
                {
                    "title": "New title"
                }
            ```
            updates "title" of the book which id 2

        * Response:
        ```json
            {
                "id": 2,
                "success": true
            }    
        ```
        ⚠️ You can see updates by sending GET request to endpoint "/books"
        or by sending GET request to endpoint "/books/2"


    * Update only "author"
        * Request: ` PATCH/ http://127.0.0.1:5000/2 `
            * Request body:
            ```json
                {
                    "title": "New author"
                }
            ```
            updates "author" of the book which id 2

        * Response:
        ```json
            {
                "id": 2,
                "success": true
            }    
        ```
        ⚠️ You can see updates by sending GET request to endpoint "/books"
        or by sending GET request to endpoint "/books/2"


    * Update all data
        * Request: ` PATCH/ http://127.0.0.1:5000/2 `
            * Request body:
            ```json
                {
                    "title": "New title",
                    "author": "New author",
                    "rating": "1"
                }
            ```
            fully updates the book which id 2

        * Response:
        ```json
            {
                "id": 2,
                "success": true
            }    
        ```
        ⚠️ You can see updates by sending GET request to endpoint "/books"
        or by sending GET request to endpoint "/books/2"


    * Not update:
        * Request: ``` PATCH/ http://127.0.0.1:5000/2 ```
            * Request body:
            ```json
                {}
            ```
            not updates the book with id 2

        * Response:
        ```json
            {
                "id": 2,
                "success": true
            }    
        ```
        ⚠️ You can see updates by sending GET request to endpoint "/books"
        or by sending GET request to endpoint "/books/2"

* ### ⚠️ Warning
    * If you enter wrong value to parametres, for example "rating": "new_rating". "rating" parameter is"integer", so you can't give  "string" value to this parameter. If you enter wrong values, API returns "400" error with message "bad request"
    * Example 
        * Request: ``` PATCH/ http://127.0.0.1:5000/2 ```
            * Request body:
            ```json
                {
                    "title": "123",
                    "author": "001"
                }
            ```
            In here, "title" and "author" should be "string" not an "integer"

        * Response:
        ``` json
            {
                "error": 400,
                "message": "bad request",
                "success": false
            }
        ```



## DELETE /books/{book_id}
* ### General:
    * Delete book. You can get book via its ID, which given in URL parametres
    * The API returns the following values to us:
        * `"books" is a list of books`
        * `"deleted" is the id of the deleted book`
        * `"total_books" is the total number of books`


* ### Example:
    * Request: ``` DELETE/ http://127.0.0.1:5000/2 ```

    * Response:
     ```json
        {
            "books": [
                {
                    "author": "Kel Newport",
                    "id": 1,
                    "rating": 6,
                    "title": "Attention"
                }
            ],
            "deleted": 2,
            "success": true,
            "total_books": 1
        }
    ```
* ### ⚠️ Warning
    * If you enter wrong id to url parameters, API returns error with message "deleting was unsuccessful"
    * Example:
        * Request: `DELETE/ http://127.0.0.1:5000/1212112`

        * Response:
        ```json
            {
                "message": "deleting was unseccussful",
                "success": false
            }
        ```
## Author
#### Abdulloh Jo'rayev
