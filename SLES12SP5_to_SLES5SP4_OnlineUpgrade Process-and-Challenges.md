# SLES 12 SP5 to SLES 15 SP4 Online Upgrade: Process & Challenges
## Online Upgrade Process
**Prerequisites**  </br>
✔ Active SUSE subscription  </br>
✔ Stable internet connection  </br>
✔ Minimum 10GB free disk space  </br>
✔ Root access  </br>
✔ Full system backup
## Step-by-Step Upgrade
### 1.	Update SLES 12 SP5 to latest patches
```
sudo zypper ref
sudo zypper update
```
### 2.	Install migration pattern
```
sudo zypper in -t pattern SUSE-migration-sles15
```
### 3.	Deregister old version
```
sudo SUSEConnect --de-register
```
### 4.	Register for SLES 15 SP4
```
sudo SUSEConnect -r YOUR_REG_CODE -p sle-module-basesystem/15.4/x86_64
```
### 5.	Start the migration
```
sudo zypper migration --product sle-module-basesystem/15.4/x86_64
```
(Follow on-screen prompts carefully)
### 6.	Reboot and verify
```
sudo reboot
cat /etc/os-release
```
## Key Challenges & Internet Dependencies
### 1. Internet Connectivity Requirements
•	Mandatory for:
  o	Package downloads (~2-4GB data)
  o	SUSEConnect registration
  o	Repository metadata updates
•	Partial failures occur if connection drops mid-upgrade
### 2. Dependency Resolution Issues
•	Common problems:
  o	Broken dependencies due to repo conflicts
  o	Missing migration patterns
  o	Third-party package incompatibilities
### 3. Service Disruptions
•	Network services may restart unexpectedly
•	Database applications may require manual intervention
### 4. Time Consumption
•	Average upgrade time: 1-3 hours (depending on bandwidth)
•	Slower connections significantly increase downtime window
## Troubleshooting Tips
### 1.	If connection fails during upgrade:
```
sudo zypper dup --no-download
```
### 2.	For repository errors:
```
sudo zypper clean
sudo zypper ref
```
### 3.	To check failed services:
```
systemctl --failed
journalctl -xe
```
## Best Practices
1.	Schedule during maintenance windows </br>
2.	Use wired connection (not WiFi)  </br>
3.	Monitor network stability during process  </br>
4.	Have console access as backup  </br>
5.	Document each step for potential rollback  </br>
## Post-Upgrade Checklist
1.	Verify all services are running:
```
systemctl list-units --state=running
```
2.	Update remaining packages:
```
sudo zypper update
```
3.	Check for orphaned packages:
```
sudo zypper packages --orphaned
```
