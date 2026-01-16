Go to the website and view the page source
![[Pasted image 20230820123933.png]]

Click on the custom.min.js file 
![[Pasted image 20230806140129.png]]

Go to cyberchef paste in the hex encoding into the input section and then select `From Hex` and it will decode the Hex 
![[Pasted image 20230806140707.png]]
Then select JS Beautify as this is what is needed to get the flag
![[Pasted image 20230806141658.png]]

Use gobuster to find differnet web paths
![[Pasted image 20230806212336.png]]

Go to /logs and go into the email_dump.txt

Researching the first phase of the SSDLC is planning so we head to /planning and put in the key from the email dump
![[Pasted image 20230806223719.png]]

After looking into the API it tells how to use the API endpoints to see what we are looking for

As we are looking for ID 5 we put this into the URL:
`http://10.10.120.130/api/?customer_id=5`
Which we are greeted with the name, email and password
![[Pasted image 20230806224229.png]]

To find the admin ID we put the following in the URL
``http://10.10.120.130/api/?customer_id=3

We then head to /realadmin and use the details to login to the admin account. Once we are logged in we are greeted with a page that allows us to execute commands from the admin page
![[Pasted image 20230819182742.png]]

We can use Burpsuite to intercept the traffic and run the commands from Burp
![[Pasted image 20230819192718.png]]

Once we have captured it in burp we can forward the request and on the website it displays what is in the current directory
![[Pasted image 20230820122620.png]]

It gives us a key to access the original file manager, so we head over to the web page where the original file manager is hosted and log in to the site with the password
![[Pasted image 20230820122826.png]]

Once logged in we can see all the files. To get the final flag we go into the index.php
![[Pasted image 20230820123638.png]]

Here we can see the restored website flag:
![[Pasted image 20230820124718.png]]
