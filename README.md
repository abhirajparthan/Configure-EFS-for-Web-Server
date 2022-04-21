
# Configure-EFS-for-Web-Server

Amazon provides various storage services each for different use cases. One of them is EFS .

Amazon EFS provides scalable file storage for use with Amazon EC2. You can create an EFS file system and configure your instances to mount the file system.

Amazon EFS is designed to provide massively parallel shared access to thousands of Amazon EC2 instances, enabling your applications to achieve high levels of aggregate throughput and IOPS with consistent low latencies.

----
### Configure Amazon Elastic File System (EFS) to serve files for Apache Web Servers.



######    Step - 1  Go to AWS console and search for EFS service. Click on Create file system.



![Screenshot from 2022-04-21 15-27-33](https://user-images.githubusercontent.com/103326353/164430489-d66a6f12-d6d9-424b-9802-dafb8cc94b9d.png)



Give the name as Webserver-Abhi. Note the VPC that this file system is created in. Click on Customize.



![Screenshot from 2022-04-21 15-30-19](https://user-images.githubusercontent.com/103326353/164431041-42e05423-8a47-4a7f-ba3e-a977c14548e4.png)



Here I am choosing the regional base and I have disabled the Backup option ( Its Paid option ). I have alreday configured the backup in the server.



![Screenshot from 2022-04-21 15-32-38](https://user-images.githubusercontent.com/103326353/164431590-64bed327-42ce-49db-b95e-7c4535073f4d.png)



In the "Lifecycle management' I am choosing is as None, Becauase of my website files are below 1GB. 



![Screenshot from 2022-04-21 15-47-00](https://user-images.githubusercontent.com/103326353/164434090-46964f45-bc74-4c53-94f8-e88739efd1de.png)



In the "Performance mode" I am choosing the General Purpose mode, Beasosuse the small website I am hosting. Max I/O is used for big files. eg: You are running the video applications you can use Max I/O  ( Max I/OPaid option ).



![Screenshot from 2022-04-21 15-50-53](https://user-images.githubusercontent.com/103326353/164434842-55cf2fde-8ba7-49c3-80f9-6e7949e25105.png)



The "Throughput mode" depends on retrieving the data speed. Here I am choosing "Provisioned" mode, Because I need a Minimum 1MB to retrieve speed. The Bursting mode speed depends on content size. eg: You have 10GB and 15GB content, the more Speed will get for 15GB size.



![Screenshot from 2022-04-21 15-54-46](https://user-images.githubusercontent.com/103326353/164436795-71942ed3-6392-43b3-adfa-38ea6b18c0d4.png)



Here I untick the encription ( Its Paid Option ), If you want you can click.



![Screenshot from 2022-04-21 15-57-44](https://user-images.githubusercontent.com/103326353/164438617-ee8d59d5-7126-4f74-8e39-3a6246136ae4.png)



I have added the Tag name as Webserver-Abhi



![Screenshot from 2022-04-21 15-59-41](https://user-images.githubusercontent.com/103326353/164439519-d13414e0-ebb6-4a15-a823-81da19fc4a82.png)



Network section I have added the 2 zones and one security group in the EFS. Also, Don't forget to add the 2049 port to the security group.



![Screenshot from 2022-04-21 16-01-00](https://user-images.githubusercontent.com/103326353/164440105-e85875f3-7bfa-473b-9611-9095ae9f80c6.png)



Then Click Next ==> Next in the bottom of the page and create the EFS. Final EFS Created and keep the EFS id in the plain text page. Also Ensure that the File system state is Available.



![Screenshot from 2022-04-21 17-01-05](https://user-images.githubusercontent.com/103326353/164449126-5680dfd3-1ccb-4bef-810a-a3684d3599f9.png)



My EFS id is "fs-00ddf29d91f6063cf".




###### Step 2: Go to EC2 services and create a Linux Instance. Add tag as Name: Webserver-EFS.

Make sure to have SSH and HTTP rule in the security group.



![Screenshot from 2022-04-21 16-19-18](https://user-images.githubusercontent.com/103326353/164442709-e1c24cd6-d2df-4098-b3bb-133798895f53.png)



###### Step 3: SSH into the Instance.

Now install amazon-efs-utils package which has efs mount helper using the following command:
~~~
yum install -y amazon-efs-utils
~~~

Run the following command to install Apache Server:
~~~
yum -y install httpd
~~~

Then open the /etc/fstab file and add below content. Then mount usinfg the command "mount -a"
~~~
vi /etc/fstab/
~~~~

###### fs-00ddf29d91f6063cf:/  /var/www/html   efs     defaults,_netdev        0       0

~~~
mount -a
~~~


###### Step 4: After the utility installation are complete, Mount the EFS to the server.


Then open the /etc/fstab file and add below content. Then mount usinfg the command "mount -a"
~~~
vi /etc/fstab/
~~~~

###### fs-00a03a03f34f14897:/  /var/www/html   efs     defaults,_netdev        0       0

~~~
mount -a
~~~

Then check the moting using the df -h command. The out put look like this



![Screenshot from 2022-04-21 16-58-47](https://user-images.githubusercontent.com/103326353/164448988-b8025e29-e37a-41a9-acc7-d02a83278432.png)



Then My files are located in the webserver folder and I have move this to /var/www/html/ location.



![Screenshot from 2022-04-21 17-04-25](https://user-images.githubusercontent.com/103326353/164449764-02174dcd-4948-4da8-868a-cc6544c4fcb3.png)



THen gives the apache permission to the files and load the server hostname in the browser



![Screenshot from 2022-04-21 17-06-05](https://user-images.githubusercontent.com/103326353/164449952-862bc61c-1e3f-49f1-9283-22d864eed3fd.png)


And with the next command restart and enable the apache server:

~~~
sudo systemctl  restart httpd

sudo systemctl  enable  httpd
~~~

Now the website loading with EFS file system.



![Screenshot from 2022-04-21 17-08-08](https://user-images.githubusercontent.com/103326353/164450205-0584d802-1f7f-4596-ad52-2e755fbcfd61.png)





---
## Conclusion

In this tutorial, I have shown how to setup a apache with EFS file system. This is an easier way to set up a  EFS file system using apache webserver.


---
### ⚙️ Connect with Me

 <p align="center">
<a href="mailto:aparthan275@gmail.com"><img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white"/></a>
<a href="https://www.instagram.com/_r.e.b.e.l.z_33/"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/abhiraj-parthan-82038b191"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a> 
<a href="https://www.wppredirect.tk/go/?p=918893532145&m=Abhiraj%20Parthan."><img src="https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white"/></a>
  </a></p>
</div>




