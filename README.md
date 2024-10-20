# Analyzing-a-MySQL-Database-Attack
In this lab, you will view and analyze a .PCAP file from a previous attack against a SQL database based on a Cisco Networking Academy training.

Step by Step →

Part 1: Open Wireshark and load the PCAP file.
Part 2: View the SQL Injection Attack.
Part 3: The SQL Injection Attack continues…
Part 4: The SQL Injection Attack provides system information.
Part 5: The SQL Injection Attack and Table Information
Part 6: The SQL Injection Attack Concludes.

Background/Scenario → SQL injection attacks allow malicious hackers to type SQL statements into a website and receive a response from the database. This allows attackers to tamper with actual data in the database, impersonate identities, and perform a variety of malicious activities.

A PCAP file has been created for you to view a previous attack against a SQL database. In this lab, you will analyze and answer the questions.

Part 1: Open Wireshark and Load the PCAP File →
The Wireshark application can be launched using a variety of methods on a Linux workstation.

1° — Launch the Security Workstation VM.

VM initialization →

![image](https://github.com/user-attachments/assets/fd91cf7a-0b7f-4f48-b458-2c1dd85efc92)

2° — Click on Applications > CyberOps > Wireshark on the desktop and navigate to the Wireshark application.

3° — In the Wireshark application, click on Open in the middle of the application under Files.

4°— Navigate to the /home/analyst/ directory and look for lab.support.files. In the lab.support.files directory and open the SQL_Lab.pcap file.

5°— Open the PCAP file in Wireshark and display the captured network traffic. This capture file spans a period of 8 minutes (441 seconds), the duration of this SQL injection attack.

.PCAP archieve open into Wireshark →

![image](https://github.com/user-attachments/assets/0e2e40a4-b8c7-43d4-85c4-9ab0a44764b1)

Part 2: View the SQL Injection Attack →
In this step, you will be viewing the beginning of an attack.

1°— In the Wireshark capture, right-click on line 13 and select Follow > HTTP Stream. Line 13 was chosen because it is an HTTP GET request. This will be very useful in following the data stream as the application layers see it and leads up to the query test for SQL injection.

![image](https://github.com/user-attachments/assets/5ec3287f-c88d-451c-b05e-825386903edb)

Source traffic is shown in red. The source sent a GET request to host 10.0.2.15. In blue, the target device is responding back to the source.

2°— In the Find field, enter 1=1. Click Find next.

![image](https://github.com/user-attachments/assets/03ae7974-092e-41b1-b81b-74913861e5b0)

3°— The attacker entered a query (1=1) into a UserID search box on the target 10.0.2.15 to see if the application is vulnerable to SQL injection. Instead of the application responding with a login failure message, it responded with a record from a database. The attacker verified that he can enter a SQL command and the database will respond. The search string 1=1 creates a SQL statement that will always be true. In the example, no matter what is entered into the field, it will always be true.

![image](https://github.com/user-attachments/assets/cd2ed76a-88fb-4add-87d6-7b05adc08798)

4° — Close the Follow HTTP Stream window.

5° — Click on Clear display filter (X) to display the entire Wireshark conversation.

Part 3: The SQL Injection Attack Continues… →
At this stage, you will be viewing the continuation of an attack.

1°— In the Wireshark capture, right-click on line 19 and click Follow > HTTP Stream.

2° — In the Find field, enter 1=1. Click Find next.

3° — The attacker entered a query (1' or 1=1 union select database (), user () #) in a UserID search box on the target 10.0.2.15. Instead of the application responding with a login failure message, it responded with the following information:

![image](https://github.com/user-attachments/assets/b087ae5a-75e6-4523-a1bb-84caff43da05)

The database name is dvwa and the database user is root@localhost. There are also multiple user accounts displayed.

4°- Close the Follow HTTP Stream window.

5°- Click Clear display filter to display the entire Wireshark conversation.

Part 4: SQL Injection Attack Provides System Information →
The attacker continues and starts to target more specific information.

1st — In the Wireshark capture, right-click on line 22 and select Follow > HTTP Stream. In red, the source traffic is shown and is sending the GET request to host 10.0.2.15. In blue, the target device is responding back to the source.

2°- In the Find field, enter 1=1. Click Find next.

3°- The attacker entered a query (1' or 1=1 union select null, version () #) in a UserID search box on the target 10.0.2.15 to find the version identifier. Notice how the version identifier is at the end of the output.

![image](https://github.com/user-attachments/assets/7a4738be-123f-491e-aa3d-b46e840e3c1a)

4° — Close the Follow HTTP Stream window.

5° — Click on Clear display filter to display the entire Wireshark conversation.

Part 5: SQL Injection Attack and Table Information →
The attacker knows that there are a large number of SQL tables that are full of information. The attacker tries to find them.

1st — In the Wireshark capture, right-click on line 25 and select Follow > HTTP stream. The source is shown in red. It sent a GET request to host 10.0.2.15. In blue, the target device is responding back to the source.

2nd — In the Find field, enter users. Click Find next.

![image](https://github.com/user-attachments/assets/ad2b7736-d99f-46f8-b819-ad1868cc7f0a)

3°- The attacker entered a query (1'or 1=1 union select null, table_name from information_schema.tables #) into a userid search box on target 10.0.2.15 to view all tables in the database. This gives a huge output of many tables, as the attacker specified “null” without any additional specifications.

![image](https://github.com/user-attachments/assets/2de5b5dc-80be-4390-8f7a-c7f5a548ae24)

4°- Close the Follow HTTP Stream window.5°- Click on Clear display filter to display the entire Wireshark conversation.

Part 6: SQL Injection Attack Concludes →
The attack ends with the biggest prize of all; password hashes.

1st — In the Wireshark capture, right-click on line 28 and select Follow > HTTP Stream. The source is shown in red. It sent a GET request to host 10.0.2.15. In blue, the target device is responding back to the source.

2nd — Click Find and type 1=1. Search for this entry. Once the text is located, click Cancel in the Find Text search box.

The attacker entered a query (1'or 1=1 union select user, password from users#) into a UserID search box on the target 10.0.2.15 to get usernames and password hashes!

![image](https://github.com/user-attachments/assets/309827b7-de69-4b7d-8ac0-c5da15dbe5aa)

3° — Using a website like https://crackstation.net/, copy the password hash into the password hash cracker and start cracking.

![image](https://github.com/user-attachments/assets/86150194-d6bf-4436-ac71-f58e71b54d7d)

4° — Close the Follow HTTP Stream window. Close all open windows.

LAB Conclusion →
At the end of this lab, I was able to observe that the direct use of variable values ​​in SQL queries exposed the organization to serious security risks. As demonstrated, simple queries manipulated with parameters such as “1=1” and “1+1” allowed an attacker to obtain sensitive information, including usernames. In addition, by performing a search for the “users” parameter, the attacker gained access to password hashes and, with the help of online tools such as the Crackstation.net website, was able to decipher them.
To prevent this type of attack, it is essential to implement two essential practices that, when combined, are highly effective in preventing SQL injections:
Use of Prepared Statements (or Parameterized Queries): Instead of directly inserting variable values ​​into an SQL query, prepared statements or parameterized queries should be used. This ensures that the database treats input as data, not as part of SQL code, eliminating the possibility of malicious manipulation.
Data Validation and Sanitation: It is crucial to validate and sanitize all input data before using it in SQL queries. This includes ensuring that the data is in the expected format (such as checking that an email field is, in fact, a valid email address) and removing or escaping potentially dangerous characters that could be used in injections.

Adopting these practices significantly strengthens security against SQL injection attacks, protecting the integrity and confidentiality of your organization’s data.
