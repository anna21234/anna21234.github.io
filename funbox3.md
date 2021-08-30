## Initial enumeration:
  
With this box we start with nmap:  
  
`sudo nmap -T4 -p- the ip of the funbox vm`  
  
We have 80 and 22, possibly 33060  
Quick overview:  
`Port 80 is HTTP`  
`Port 22 is SSH`  
`Port 33060 is MySQL` from the port 3306 (they just happened to add an extra 0)  
  
If we dirb it we find a gym page, a secret page and an admin login page.  
To dirb a page write:  
`dirb http://ip of the vm box here`  
  
Some more time later using dirb we fnd a store page which is a bookstore.  
  
The admin, secret and gym pages do nothing useful.  
  
What you want is the store page.  
  
In the store page there is an admin log in at the bottom of the page. Navigate to the store admin page and use the creds admin:admin  
(Default creds are usually admin admin or admin password)  
  
After logging as admin, we can now upload new books, complete with image upload facilities.  
Time to upload a reverse shell, access it and catch it as per usual.  
  
For context:  
We catch a reverse shell with netcat after uploading it to the new book page we got as admin.  
Since the shell will be uploaded using the image upload feature, it will be stored in the image folder. Click on it, have your netcat ready and you should get a connection.  
  

## User shell:

  
  
You should now have shell as www-data.  
  
Looking around the system there is a file called password.txt  
  
In this file you find creds for a user called Tony among creds for other services such as the before mentioned bookstore admin page.  
Copy these creds over and login with SSH. We will login with SSH as the user Tony.  
  
  

## Root shell:

  
Once logged in as Tony check your sudo privs by running sudo -l  
  
It appears that Tony can run a lot of applications as root using the sudo rights. There are many privilige escalation opportunities since there are many binaries the Tony user can run.  
  
Some priv esc examples for this users are: pkexec and time  
  
To gain root using the time binary: simply write `sudo time /bin/bash`  
Alternatively to use pkexec: simply write `sudo pkexec /bin/bash`  
  

## Boom root!
