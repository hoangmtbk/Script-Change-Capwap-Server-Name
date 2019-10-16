#pip install python-nmap
#pip install paramiko
import nmap
import os
import sys
import select
import paramiko
import time
from pprint import pprint

def scan_n_ssh():
    
	cmd_list=["capwap client server name hivemanager-ng.rvc.net.vn\n", "save config\n" , "reboot\n"]
	#CAPWAP Default Server Name: redirector.aerohive.com
    # Get IP addresses to test
	nm = nmap.PortScanner()
    #res = n.scan("172.16.0-10.*", "22")
	res = nm.scan("192.168.1.10-61", "22")
	op = []
	f = open('log.txt', 'w')
	for i in res["scan"]:
		if res["scan"][i]["tcp"][22]["state"] == "open":
			op.append(i)
	print(op)
    # Test if the password is default
	for i in op:
		print("Trying to connect to %s:" %i)
		try:
			ssh = paramiko.SSHClient()
			ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
			ssh.connect(i,username='admin',password='aerohive')
			print("connect to %s successfully" %i)
		except paramiko.AuthenticationException:
			print("Authentication failed when connecting to %s" %i)
			continue
		except:
			print("Could not SSH to %s, waiting for it to start" %i)
			continue
		chan = ssh.invoke_shell()
		print(repr(ssh.get_transport()))
		print("*** Here we go!\n")
		chan.send('capwap client server name hm.abc.com.vn\n')
		time.sleep(5)
		chan.send('save config\n')
		time.sleep(5)
		#chan.send('reboot\n')
		#time.sleep(2)
		#chan.send('Y\n')
		#time.sleep(1)
		#chan.send('show capwap client | include \"CAPWAP server IP\"\n')
		#time.sleep(10)
		#resp = chan.recv(9999)
		#output = resp.decode('ascii').split(',')
		#print (''.join(output))
		chan.close()
		ssh.close()
	return

def main():
	while True:
		cmd = int(input("\n\nEnter 1 to Start/Next, or 2 to Stop:"))
		if cmd == 1:
			scan_n_ssh()
		elif cmd == 2:
			sys.exit(1)
		else:
			print("Invalid command...")
	return
if __name__ == "__main__":
    main()
