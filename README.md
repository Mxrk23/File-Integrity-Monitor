<p align="center">
<h1>Building a File Integrity Monitor using Powershell </h1>


<h2>Overview</h2>

- When I startup the Powershell script, it is going to ask what the user wants to do: A) Collect new Baseline or B) Begin monitoring files with saved Baseline

- If the user wants to collect a new baseline it will first caculate the HASH value from the target files and then store the hash pairs in a baseline.txt file.

- If the user want to begin monitoring files with a saved baseline it will load the baseline.txt file with the hash pairs into a dictionary. This is a data structure used to store the key (filename) with it’s corresponding (hash). 


Essentially, It will then store all the key values pairs in the baseline.txt file into a hash table and will continuously loop through each target files and make sure the hash always matches to the hash in the baseline. If one of the hashes ends up being different, my program will send out a security alert notifying the user/admin that something has been changed. 
<h2>Tools and Utilities Used</h2>

- <b>VirtualBox<b>
  
- <b>Windows 10 ISO</b>

- <b>Windows Powershell ISE</b>

- <b>Baseline.txt files - Used to store the hashes from the target files and to compare the hash value to the hash value of the files to see if there is any changes, deletion or addition for each file<b>

- <b>Macbook Pro<b>

- <b> Server 2019 ISO (used for domain controller will house active directory)<b>

- <b>2x Network Adapters (Internet + Internal VMware Network)<b>

- <b>DHCP<b>

- <b>PowerShell Script<b>

- <b>Macbook Pro<b>

<h2>Lab walk-through:</h2>

<p align="center">
1) What would you like to do?.  <br/>
<br>My first step was to write a script that will ask users on what they would like to do. Either collect a new baseline or begin monitoring files with a saved basline. I’ve then ran the script which outputted the script purposes correctly on the PowerShell terminal.<br>
  


<img src="https://i.imgur.com/n3DLkuw.jpeg" alt="File Integrity Monitoring"/>


<p align="center">
1.1). Explaining script on each line.  <br/>
<br>Line 10: If the user enters a lowercase “A” then the script will automatically read it as a uppercase “a”. This is the same for line 14 for the input of “B”.

If the user puts an input of A then they will get the “Calculate Hashes, make baseline.txt” message on the console. I’ve also made the Foreground color of the text to be cyan.

if the user puts an input of B. The console will display a message indicating that it will start file monitoring using the saved baseline. The foreground color is set to yellow<br>
  


<img src="https://i.imgur.com/42GiioY.jpeg" alt="File Integrity Monitoring"/>

<p align="center">
1.2) Calculating Hash file.  <br/>
<br>On line 9-11, I’ve created a new function that calcuclates the hash file from the filepath and once it has been calculated it will returns the hash value onto the console <br>
  


<img src="https://i.imgur.com/vMl621K.jpeg" alt="File Integrity Monitoring"/>

<p align="center">
1.3. Collecting all files in the target folder.   <br/>
<br>On Line 21, I’ve wrote a function that collects all the files in the target folder by making a variable called $files = Get-Childitem and using the .\Files as the  “-Path”. Once this is done it will print all the files in the $files variable. <br>
  


<img src="https://i.imgur.com/vMl621K.jpeg" alt="File Integrity Monitoring"/>




  

