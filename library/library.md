#Library Management System.

##Functional Requirement
    1. Create book item
    2. Search for book item
    3. Create user
    4. User to reserve a book item

##Non Functional Requirement

    1. System to be available all the time
    2. Book search to get back in 2 secs.
    3. Handle User growth.


#High Level Design


##API Design
   1. create book item
    endpoint: POST  /library/book/create
    input:
        book name
        book author
        book publisher
        book year
        book category

    output:
        202: Created OK response.
        {
            book: {
                id: 124,
                name: book name,
                author: Author name
            }
        } 

        409: Conflict
        {
            "error": "Book already registered"
        }

        400: Bad input
        {
            "error": "Your input looks like invalid, please check your input"
        }

    2. Searh for a book
        endpoint: GET /library/book/lookup
        input: {
            name: <book name>
            author: <author>
            publisher: <publisher>
            year: <year>
        }

        output: 
        
        200:    {
            book: id
            name: <book name>
            available: True/False
        }

        404: {
            "Error": "Book not available"
        }

        400: {
            "error": "Invalid parameter"
        }

    3. Create user
        endpoint: POST /library/user/register
        input: {
            username: <name>
            First Name: <FN>
            Last Name: <LN>
            age: 40
            city: Fremont CA
        }
        output:
        202: {
            id: <unique id>
            name: <FN LN>
        }

        400: {
            "error": "invalid parameter"
        }

        404: {
            "error": "Not Found"
        }

        409: {
            "Error": "User already exists"
        }

    4. Reserver a book
        endpoint: POST /library/book/reserve
        input: {
            bookid: 646846454,
            userid: 546845135
            startDate: today()
            endDate: today()+ 14 days
        }

        output: 200: {
            booking id: 983428398234,
            bookid: <bookid>,
            userid: <userid>
        }
        400: {
            "error": "invalid parameter"
        }

        404: {
            "error": "Not Found"
        }

        409: {
            "Error": "Book already resevered by someone else."
        }


##Draw the diagram

    User logs to system --> CDN --> Load Balancer

                Search Book service                     Reserve a book service

                       |                                            | 

                    cache service                                 database (write to the database)
                                                                    use the primary instance endpoint

                        |

                    database
                    use the read replica endpoint.

                    RDBMS since is the structured data and lot of
                    data integrity/relationships.

##Deep Dive

Identify the issue area and solution the problem.



Non Functional Requirment

    System should be available 100 %

    1. Place the application behind the load balancer
        Setup Autoscaling for EC2 instance- so that it will increase or decrease based on the incoming traffic.
        DB - Sharding can be done.

    2. Book search result in less than 2 secs
        Load Balancer - based on Geo location
        Implement Cache if needed. that will spead up the book details
        Vertical scalling on the ec2 instance.
        Database - Indexing can be done for the specific table.
            implement hashing in sharding and return the result.

    3. Handle User Growth / Scaling
        Load Balancing, and Horizontal scaling will handle the user growth.




