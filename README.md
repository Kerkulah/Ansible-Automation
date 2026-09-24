
<h1> Ansible Automation
</h1>



<h2>Project Overview</h2>
Setup a dedicated Ansible controller in the homelab, establish key based SSH access to all managed hosts (five Linux VMs + the Proxmox host itself), build an inventory, and validate two playbooks (security_checks.yml, linux_updates.yml) before scheduling unattended nightly runs. 
<br  />
<br  />
<img src="https://imgur.com/X0bKeCW.jpg"  height="80%" width="100%">



<br  />
<br />

<br />
<h2>On each Linux target, created a dedicated ansible user with passwordless sudo and confirmed via id ansible showing group membership in sudo</h2>

VM 101, 102, 104, 108, 109
<img src="https://imgur.com/jzljC33.jpg"  height="80%" width="100%">
<br />
<br />

<h2>SSH key generation</h2>
Generated a dedicated ed25519 keypair on the controller for Ansible only use, separate from any personal keys. Verified controller network config and reachability to all target subnets before key distribution.
<img src="https://imgur.com/T0MiOyQ.jpg"  height="80%" width="100%">
<br />
<br />
<h2>Key distribution</h2>
Copied the public key to all five Linux hosts via ssh copy id, confirmed by their returned hostnames and Re-ran the loop with the private key explicitly to confirm passwordless login worked end to end.

<img src="https://imgur.com/HbgAfvV.jpg"  height="80%" width="100%">

<br />
<h2>Final inventory</h2>
Ansible all -m ping returned SUCCESS/pong for every host, confirming end to end connectivity.
<img src="https://imgur.com/Jy7ft04.jpg"  height="80%" width="100%">

<br />
<br />

<h2>Playbook validation</h2>
I ran --syntax-check followed by --check (dry-run) on both playbooks
security_checks.yml: 5/5 hosts ok, unreachable=0, failed=0
linux_updates.yml: batched two hosts at a time (serial: 2) to avoid patching the whole lab simultaneously; all hosts unreachable=0, failed=0

<img src="https://imgur.com/31yXXrs.jpg"  height="80%" width="100%">
<img src="https://imgur.com/0971wgK.jpg"  height="80%" width="100%">
<br />

<br />  

<br/>

<h2>Nightly Automation</h2>
This image Confirm run-nightly.sh completes successfully end to end (all four playbooks: security checks, config backup, patching, VM status report) when run manually, then verify the cron schedule is correctly in place for unattended nightly execution.

<br />  
<br/>
<img src="https://imgur.com/AGSp9I2.jpg"  height="80%" width="100%">

<br />  

<br/>

