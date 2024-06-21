<p align="center">
<h1>Hashing Algorithms + Coding a File Integrity Monitor Using Powershell</h1>


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
  


<img src="https://i.imgur.com/RaEp4Yg.jpeg" alt="File Integrity Monitoring"/>

<p align="center">
2) Calculating Hash, writing it to baseline.txt.   <br/>
<br>On line 25, I used a foreach loop and used $f as a variable to hold each file path from the $files. Line 26 caculates the hash for the file stored in the $f value. Once the hash is caculated it will assign the result to the $hash variable. 
Line 26 constructs a string by combining the file path ($hash.Path) and the calculated hash ($hash.Hash) which is then outputted to the baseline.txt file indicated by “Out-File -FilePath .baseline.txt”. <br>
  


<img src="https://i.imgur.com/hMMD1V1.jpeg" alt="File Integrity Monitoring"/>


<p align="center">
2.1) Baseline.txt file created.    <br/>
<br>The script worked as it created a baseline.txt file with the corresponding hash value <br>
  


<img src="https://i.imgur.com/bAfUlO6.jpeg" alt="File Integrity Monitoring"/>

<p align="center">
2.2) Erase-Baseline-if-Already-Exists   <br/>
<br>I created a function that will stop the baseline.txt from being appended on when the script is run multiple times. On line 26, the Erase Baseline function will check the folder if a baseline.txt was previously created, if so, the new ran script will replace that file with the new baseline.txt file instead of adding the new hash values to one particular baseline.txt file.<br>
  


<img src="https://i.imgur.com/lTPoYnn.jpeg"/>

<p align="center">
3) Monitoring files with saved Baseline.  <br/>
<br>Once I’ve completed option A, I’ve moved on to creating a script for option B. My first step with this option is to load the files with the hash pairs from the baseline.txt file.

Line 43: $fileHashDictionary = @{} - This is used to create a new dictionary variable named $fileHashDictionary

Line 44: This line adds a key value pair to the  $fileHashDictionary.<br>
  


<img src="hhttps://i.imgur.com/fC1vRKj.jpeg"/>


<p align="center">
3.1) Loading files from baseline.txt file and storing them into a dictionary.
<br/>
<br>OLine 45: $filePathsAndHashes = Get-Content -Path .\baseline.txt - This line reads the content of the file (baseline.txt file) and stores it in the filePathsAndHashes variable. filePathAndHashes is essentially used as a dictionary.

Line 48: foreach ($if in $filePathesAndHashes) - This line is a foreach loop, the variable name within the loop is $if. The loop iterates through each line in the $filePathesAndHashes variable.

Line 49:  $fileHashDictionary.add($f.Split("|")[0],$f.Split("|")[1]) 
 { $f.Split(“|”)[1] - This line adds a key value to the dictionary using .add(). The first element “[0]” is used as they key (file path). The second element “[1]” is used as the value (hash).<br>
  


<img src="https://i.imgur.com/bsuo9cH.jpeg"/>



<p align="center">
3.2) While loop 
<br/>
<br>OMy next step is to make a script that will check the keys and values in the $fileHashDictionary and to continuously calculate the hashes on all of the files to ensure that they always match to the hashes located in the baseline dictionary. If one of the values changes, the user will need to be notified. To do this, I will print something to the screen that will alert the user a file has been changed.

On line 57, I’ve used a while true loop (which means it will run indefinitely) that will calculate the file hash for all of the files. Once the program gets a hash it is going to check inside the $fileHashDictionary to see if the key exists. If the key doesn’t exists the program will know that a file was added, if the key does exists but the hash is different than the program will know that the hash has been changed.

 Line 67: if($fileHashDictionary[$hash.Path] -eq $null) { 
For each file stored in the $files variable will have its hash calculated and the calculated hash value will be compared to the hash value stored in the $fileHashDictionary. Thie if statement “if($fileHashDictionary[$hash.Path] -eq $null)” will tell the program that a new file has been created.<br>
  


<img src="https://i.imgur.com/5huuAzR.jpeg"/>

<p align="center">
3.3) Notify if a file has been changed
<br/>
<br>OMy next step is to make a script that will check the keys and values in the $fileHashDictionary and to continuously calculate the hashes on all of the files to ensure that they always match to the hashes located in the baseline dictionary. If one of the values changes, the user will need to be notified. To do this, I will print something to the screen that will alert the user a file has been changed.

On line 57, I’ve used a while true loop (which means it will run indefinitely) that will calculate the file hash for all of the files. Once the program gets a hash it is going to check inside the $fileHashDictionary to see if the key exists. If the key doesn’t exists the program will know that a file was added, if the key does exists but the hash is different than the program will know that the hash has been changed.

 Line 67: if($fileHashDictionary[$hash.Path] -eq $null) { 
For each file stored in the $files variable will have its hash calculated and the calculated hash value will be compared to the hash value stored in the $fileHashDictionary. Thie if statement “if($fileHashDictionary[$hash.Path] -eq $null)” will tell the program that a new file has been created.<br>
  


<img src=""/>

<p align="center">
4) File has been deleted.
<br/>
<br>My last implementation is to create a function that checks if the file has been deleted or not. To know if a file has been deleted, the baseline will be compared to the files in the folder.

To do this, I’ve created a foreach loop that will iterate through each key ($key) in the $fileHashDictionary. 

Line 81:$baselineFileStillExists = Test-path -Path $key - This line checks if a file exists at the path specified by the current key ($key)

Line 82: if (-Not $baselineFileStillExists) {
This line is an if statement the “-Not” checks if the $baselineFileStillExists is $false meaning the file doesn’t exists. 

The Write-Host cmdlet on line 84 displays the message that the file has been deleted to the user. The message can be seen on the console after I’ve tested if the function works by deleting one of the files.<br>
  


<img src="https://i.imgur.com/JufoNNI.jpg"/>



  

