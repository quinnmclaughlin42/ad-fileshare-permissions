<p align="center">
<img src="https://www.kakasoft.com/wp-content/uploads/2021/09/image-20-1024x516.png" height=45% width=45% alt="NTFS Permissions Icons"/>
</p>

<h1>Configuring Network File Sharing and Permissions</h1>
This walkthrough outlines the implementation of network file sharing, and configuring access to users within the network<br />


<h2>Video Demonstration</h2>

- ### [YouTube: Showcasing Network File Sharing and Permissions Using Active Directory - Quinn McLaughlin](https://youtu.be/BZn1JZLmBnQ)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Virtual Network)
- Remote Desktop
- Active Directory Users and Computers

<h2>Operating Systems Used </h2>

- Windows Server 2022 Datacenter
- Windows 10 Pro (22H2)

<h2>High-Level Overview Steps</h2>

1. Create and share folders from the Domain Controller to Client, with varrying levels of access
2. Enable specified group share permissions within those folders
3. Showcase the access to those folders being limited due to parameters set previously
4. Add a new user to a specified group (in this case, ‘accountants')
5. Showcase the change in access due to new group permissions

<h2>Deployment and Configuration Explanation</h2>

<p>
<img src="https://github.com/user-attachments/assets/26cb6a97-ecda-42d7-b7e7-b6bc07a1dee5" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To begin, we initally log into our Domain Controller as a previously created admin. Within Active Directory (Users and Computers), we'll select a randomly created user to work with for the remainder of the project. In this instance we chose a user by the name of "caf.ler". After choosing, we access the 'C:' drive within our Domain Controller's File Explorer and create 4 unique folders by the names of "read-access", "write-access", "no-access", and lastly "accounting".
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/75fd7c67-41e0-4536-a198-27e9653b2f07" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
With our folders now created, right clicking allows us to enter the 'Properties' window, and within we'll select the 'Sharing' tab. Clikcing share, we can now give access to individual users or groups at varrying levels. For the “read-access” folder, we enter the group name of “Domain Users” and give them read-only permissions. The “write-access” folder follows the same rules, with the exception being the Domain Users having ‘write’ permissions as well. Our “no-access” folder will instead be shared with only Domain Admins, who will be given read and write access. Finally the “accounting” folder will be ignored for now, but we’ll come back to it later.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/59afb812-9a28-4cdc-a20c-e689ea85fc39" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Using our chosen user (caf.ler), we use Remote Desktop to sign into our Client Virtual Machine. Upon logging in, we can navigate to File Explorer and search using our Domain Controllers name (in this instance, we navigate with "\\DC-1"), which in turn shows us the folders we created there. Clicking on the folders will give us the permissions we allowed prior, with “no-access” blocking our access altogether, “read-access” allowing us to enter but failing to let us make changes, and “write-access” giving us full customizability of its contents. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/6a7fe179-725e-4225-88ea-ec5f7570cfdb" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, we navigate back to the Domain Controller VM, and open Active Directory Users and Computers. Within, we create a new organizational group titled “_GROUPS”, which will keep us more organized. Within this new organizational group, we’ll add a group called “ACCOUNTANTS”. Going back into the (C:) drive within File Explorer will allow us to now change the sharing properties of our created “accountants” folder. We decide to share this folder with the group we just made; “ACCOUNTANTS”. Refreshing the Client VM’s File Explorer shows this update, allowing us to see the folder, but unable to access it due to the permissions we set.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/ee104df2-49c3-4616-bc5a-6580f8e9303e" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly, we sign out of our Client VM before navigating back to our Domain Controller VM. Within Active Directory Users and Computers, we double click the ‘ACCOUNTANTS’ group, allowing us to modify the members within. Adding the user we just signed out of the Client VM with (caf.ler) should now allow that specified user to meet the parameters of interacting with the ‘accountants’ folder. Sure enough, using Remote Desktop to log back in as ‘caf.ler’ and navigating back to the shared folders in File Explorer, we see we now have the full read and write permissions that we expected.
</p>
<br />
