### Description
Given the database where all the users' data is stored in one huge XML string, 
fetch it as separate rows and columns.


Notes

The private field determines whether the user's email address should be publicly visible
If the profile is private, email_address should equal "Hidden"
The users may have multiple email addresses
If no email addresses are provided, email_address should equal "None"
If there're multiple email addresses, the first one should be shown
The date_of_birth is in the yyyy-mm-dd format
The age fields represents the user's age in years
Order the result by the first_name, and last_name columns


### My solution
```js
SELECT 
  first_name, 
  last_name, 
  date_part(
    'year', 
    age(date_of_birth)
  ) age, 
  CASE private WHEN true THEN 'Hidden' WHEN false THEN email_address END AS email_address 
FROM 
  XMLTABLE(
    'data/user' PASSING (
      SELECT 
        data 
      FROM 
        users
    ) COLUMNS first_name text path 'first_name', 
    last_name text path 'last_name', 
    date_of_birth date path 'date_of_birth', 
    private boolean path 'private', 
    email_address text path 'email_addresses/address[1]' DEFAULT 'None'
  )
  
  ORDER BY first_name, last_name
```