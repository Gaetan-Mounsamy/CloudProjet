# AWS exercice, screenshots and report

alls our screenshots are in a logical order, you can choose the next one in the list to see the next step. We didn't put a link on each image but you can go to the next one between to links thanks to the logical order

* First we created [our own VPC](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20145351.png) for this project with 10.0.0.4/16 ipv4 CDR for this project
* After we created [our public subnet](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20145502.png) that is linked to our vpc and allow [ipv4 auto-assign ip](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20145543.png) for later make it reacheable from internet.
* After we created an [internet gateway](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20145618.png) to make possible connexions from internet to our vpc, and we attached to it
* We created a route table and [edited](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20145802.png) is rules that allow inbound traffic to our vpc from internet.
* To secure it, we create a [security group](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20150029.png), to block all the inbound traffic and allow it on only three ports for http, https and ssh protocol.
* We created an ec2 instance from cloud9 and choose t3micro free plan, and [linked it to our vpc and public network](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20150145.png). We also chose the ssh secure shell connexion
* Then on it with [cloud9 console](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20151031.png) and ssh connexion we installed apache2 for web server,php, mariadb client, and download the website that you given to us and put it in the /var/www/html folder and change the owner of it to allow execution ot the php.
* When we created the ec2 instance we forgotten to connect it to our security group, so we [modified the security group setting](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20151910.png) on the ec2 instance to add it and solve our error
* the website run well now ! But we need to create the database and allow connexion from the website to it.
![run well](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20152137.png)
* So we created a Maria DB instance with the [free tier plan](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20152315.png) and setted the password.
* We indicate that we want to [establish a link with our EC2 instance](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20152342.png). That will automatically create an inbound rule to allow our ec2 instance to access our database with the creation of 2 security groups. We also linked the databse to our vpc.
* We also choose to [create automatically](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20152434.png) a private subnet for our DB and specified that we grant access from our VPC security group
* We saw that AWS, because we choose to create automatically the private subnet at the creation of our database, indicate that [3 self-managed private subnets were created](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20153226.png) in the process
* After we [imported the data from the SQL](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20154200.png) file to our database using cloud 9 shell of our ec2 instance, that we can do because of automatically set of the inbound rule.
* We [created 3 parameters](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20154557.png) to save username, password, endpoint and table name in the parameter store of AWS System Manager
* We created an [endpoint](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20154821.png) to give access to the php website to the database
* We managed to [create an IAM role](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20155747.png) with the good policies to allow our PHP website on our EC2 instance to access on our webserver.
* We [attached the role](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20155954.png) to our ec2 instance
* Now we can find the list of countries and the data :
![list of countries](https://github.com/Gaetan-Mounsamy/CloudProjet/blob/main/AWS_Site/Capture%20d'%C3%A9cran%202023-07-14%20160138.png)

