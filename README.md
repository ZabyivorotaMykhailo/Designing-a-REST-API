# Designing-a-REST-API
Catalogue(a library - books)
Requirements

Functional requirements:
1.	The system must allow users to retrieve a paginated list of books;
2.	The system must allow users to filter books by name, language, publishing house, cover type, author, genre, and page count;
3.	The system must allow users to search for books by name or ISBN;;
4.	The system must allow the admin to delete book, language etc
5.	The system must allow users to retrieve detailed information about a single book, author, genre, or publisher by ID;
6.	The system must allow the admin to add new books, authors, genres, publishing houses, languages, and cover types ;
7.	The system must allow admins to delete books, authors genres etc. from the catalogue.

Non-Functional requirements:
1.	Security & Authorization. Access to modification endpoints (POST, PUT, PATCH, DELETE) must be restricted to authenticated users with the ADMIN role;
2.	Error Handling & Reliability. In the event of invalid user input, missing resources, or authorization failures, the system must return meaningful HTTP status codes;
3.	Performance & Caching. Read operations (GET) for static and semi-static catalogue data must support HTTP caching headers;
4.	Pagination. All list responses returning multiple resources must support pagination

<img width="974" height="619" alt="image" src="https://github.com/user-attachments/assets/8ab5e20d-eb1c-4880-8fa4-62cda5817abf" />

 


Database Entities & API Specification
1. Database Entities
book
idbook — unique identifier of the book: INT NOT NULL

name — title of the book: VARCHAR(60) NOT NULL

publicationDate — date when the book was published: DATE NOT NULL

publishing_house_idpublishing_house — foreign key referencing the publisher: INT NOT NULL

page — total number of pages: INT NOT NULL

amount — quantity of book copies available in stock: INT NOT NULL

ISBN — International Standard Book Number: VARCHAR(20) NOT NULL

description — description of the book: TEXT(300)

cover_idcover — foreign key referencing the cover type: INT NOT NULL

language_idlanguage — foreign key referencing the language of the book: INT NOT NULL

author
idauthor — unique identifier of the author: INT NOT NULL

fullName — author's full name: VARCHAR(100) NOT NULL

biography — short biography of the author: TEXT(300) NOT NULL

genre
idgenre — unique identifier of the genre: INT NOT NULL

genreName — name of the genre (e.g., Fantasy, Sci-Fi): VARCHAR(45) NOT NULL

publishing_house
idpublishing_house — unique identifier of the publishing house: INT NOT NULL

name — name of the publisher: VARCHAR(45) NOT NULL

language
idlanguage — unique identifier of the language: INT NOT NULL

name — language name (e.g., English, Ukrainian): VARCHAR(45) NOT NULL

cover
idcover — unique identifier of the cover type: INT NOT NULL

name — type of binding/cover (e.g., Hardcover, Paperback): VARCHAR(20) NOT NULL

2. API Operations Specification
Books (/books)
GET /books
Description: Browse the book catalogue with support for search, filtering, sorting, and pagination.

Query Params:

Filter: language_id, publishing_house_id, cover_id, author_id, genre_id, min_pages, max_pages

Search: search by name and ISBN

Order By: sort (publicationDate:asc/desc, name:asc/desc)

Pagination: page (default: 1), limit (default: 10)

Caching: Cache-Control: public, max-age=300

Responses: 200 OK, 400 Bad Request

GET /books/{id}
Description: Retrieve detailed information about a specific book by ID.

Responses: 200 OK, 404 Not Found

POST /books
Description: Add a new book to the catalogue (Admin only).

Responses: 201 Created, 400 Bad Request, 401 Unauthorized, 403 Forbidden

PUT /books/{id}
Description: Completely replace all fields of a book by its ID (Admin only).

Responses: 200 OK, 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found

PATCH /books/{id}
Description: Partially modify specific attributes of a book (e.g., only amount or description) (Admin only).

Responses: 200 OK, 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found

DELETE /books/{id}
Description: Remove a book from the system (Admin only).

Responses: 204 No Content, 401 Unauthorized, 403 Forbidden, 404 Not Found

Authors (/authors)
GET /authors
Description: Browse the list of authors.

Query Params:

Search: fullName

Order By: sort (fullName:asc/desc)

Pagination: page, limit

Caching: Cache-Control: public, max-age=86400

Responses: 200 OK, 400 Bad Request

GET /authors/{id}
Description: Retrieve detailed information and biography of a specific author using ID.

Responses: 200 OK, 404 Not Found

POST /authors
Description: Create a new author entry (Admin only).

Responses: 201 Created, 400 Bad Request, 401 Unauthorized, 403 Forbidden

Genres (/genres)
GET /genres
Description: Browse the complete list of genres.

Query Params:

Search: genreName

Pagination: page, limit

Caching: Cache-Control: public, max-age=86400

Responses: 200 OK

POST /genres
Description: Create a new genre entry (Admin only).

Responses: 201 Created, 400 Bad Request, 401 Unauthorized, 403 Forbidden

Publishing Houses (/publishing-houses)
GET /publishing-houses
Description: Browse the list of publishing houses.

Query Params:

Search: name

Pagination: page, limit

Responses: 200 OK

Lookups (/languages & /covers)
GET /languages
Description: List available book languages (e.g., English, Ukrainian).

Responses: 200 OK

GET /covers
Description: List cover/binding types (e.g., Hardcover, Paperback).

Responses: 200 OK
