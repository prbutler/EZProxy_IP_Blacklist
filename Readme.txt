--Welcome to the EZProxy IP Address Blacklist--

Previously maintained by Paul Butler, Dartmouth College, paul.r.butler@dartmouth.edu
This file/project is no longer actively maintained. Last updated: 2024-02-23

This information was formerly located on The Unofficial EZproxy Self-Support Wiki, which is shutdown. 

EZProxy_IP_Blacklist_RejectIP.txt is a list of contributed IP addresses and ranges that have been used to access, or attempt to access, EZproxy servers in an illegitimate way. This information can be used in a number of ways to better secure EZproxy. EZProxy_IP_Blacklist_IFIP.txt contains the same IP addresses presented EZProxy_IP_Blacklist_RejectIP.txt, but in a different format to be used with the IFIP method outlined below. 

NOTE: EZproxy must be restarted for changes to config.txt to take effect.

--RejectIP--
For use with the file: EZProxy_IP_Blacklist_RejectIP.txt - https://github.com/prbutler/EZProxy_IP_Blacklist/blob/master/EZProxy_IP_Blacklist_RejectIP.txt

IP addresses and ranges can be used in config.txt as a simple list of RejectIP directives, like this:
	RejectIP 123.123.123.123
	RejectIP 41.42.0.0-41.42.255.255

You may also create a list of RejectIP statements in an external file and use an include directive in config.txt to include that file. For example, create the file EZProxy_IP_Blacklist_RejectIP.txt in your EZproxy directory that contains your RejectIP statements. Then use the following statement in config.txt to include the file:

	IncludeFile EZProxy_IP_Blacklist_RejectIP.txt

OCLC provides documentation on RejectIP here: https://help.oclc.org/Library_Management/EZproxy/Configure_resources/RejectIP

--IFIP--
For use with file: EZProxy_IP_Blacklist_IFIP.txt - https://github.com/prbutler/EZProxy_IP_Blacklist/blob/master/EZProxy_IP_Blacklist_IFIP.txt

RejectIP does not create an entry in the EZproxy audit files for a rejected IP address. An alternative method to RejectIP for blocking and creating an audit entry was documented on the Unofficial EZproxy Self-Support Wiki at the Restricted IP Logging and Response page.

Since the wiki was closed and the technique is important, I added the contents of the wiki page to a text file in this repo. All credit for this work goes to Duane Denham, Computer Specialist at Oklahoma State University. See the file here: https://github.com/prbutler/EZProxy_IP_Blacklist/blob/master/Restricted_IP_Logging_and_Response.txt

This method, upon authentication, routes the user to a given html page (deny.htm in the example below) and writes a given message in the audit file (whatever_message_you_want in the example below). In user.txt the entry would be:  

IfIP 123.123.0.0-123.123.255.255; Audit whatever_message_you_want; Deny deny.htm

IFIP allows a single IP address as well as IP ranges. You can also use an include directive to link to a file that contains a list of IfIP directives. 

OCLC provides documentation on IfIP at their Common Conditions and Actions page here: https://help.oclc.org/Library_Management/EZproxy/Authenticate_users/Directives_and_configurations_for_authentication/Common_conditions_and_actions

--IntrusionAPI--
With the release of EZproxy v6.5 OCLC introduced the directive IntrusionAPI, which provides similar functionality to the audit and blocking methods discussed here. Either technique can be used. The EZproxy IP Blacklist will continue to be maintained and updated. For information on IntrusionAPI see: https://help.oclc.org/Library_Management/EZproxy/Configure_resources/IntrusionAPI

--Additional Uses--
The IP addresses listed here may also be used at the server or network level to block access to EZproxy and other campus resources. The specific methods to do so are too varied to outline here; network security personnel or server administrators at your institution should be consulted.

--Load Balancer--
If you are running EZproxy behind a load balancer, it is likely that your EZproxy server will only get requests from the load balancer's pool of IP addresses. This list can be used to reject hosts, but it should be implemented in the load balancer's configuration itself.

--Best Practices--
A common practice for fraudulent users is to first login to a VPN with a compromised 
account from institution X, and while logged into the VPN use a second compromised account 
from institution Y to access institution Y's proxied resources. This practice is used to 
conceal the country from which they are accessing EZProxy and/or circumvent block country 
directives. When investigating a compromised account if you identify this situation it is 
a good practice to send an email to abuse@the.school.edu detailing the time/date and IP 
address used. You can quickly identify the abuse email address for an institution by 
performing a whois lookup on the IP address in question. Useful sites for this are: 
http://whois.arin.net/ui/ and https://www.iplocation.net/. Another option is to block IP 
ranges for other institutions. Some institutions have listed their IP ranges here: 
https://github.com/bertrama/ezproxy-config/blob/master/reject-ip-vpn

Several public websites provide information about IP addresses, such as the ISP/provider or geographic location, which is helpful when investigating compromised user accounts. Such examples include: 	
	https://www.iplocation.net/
	https://www.whatismyip.com/ip-address-lookup/?iref=cta
	https://www.projecthoneypot.org/search_ip.php
	https://www.ip-tracker.org/locator/ip-lookup.php
	
The RejectIP directive redirects a user to reject.htm. It is good practice to provide legitimate users a way to contact your institution when they arrive at reject.htm. Examples include a link to a chat function or an email address. For troubleshooting it is helpful to suggest to the user a method to identity their IP address. Most users are familiar with Google, so https://www.google.com/search?q=what%27s+my+ip&cad=h is a good link to include on reject.htm.

--Notes--
Bash command used to sort a list of IP addresses and IP ranges (only works on a file of IP addresses without RejectIP):
sort -t . -k 1,1n -k 2,2n -k 3,3n -k 4,4n Unsorted_List_of_IPs.txt > Sorted_List_of_IPs.txt

Method used to add RejectIP to the start of each line, by using find/replace in Notepad++:
Find: ^
Replace: RejectIP

Method used to add " ; Set Match = "True"" to the end of each line, by using find/replace in Notepad++:

Find: $
Replace:  ; Set Match = "True"

Using NotePad++, to find errant characters in a list of bare addresses using the following Find:
([[:alpha:]])|([[:blank:]])|([[:unicode:]])|(["'?!;:#$%&()*+/<>=@\^_{}|~])

An explanation of the search:
	[[:alpha:]] - search for letters
	[[:blank:]] - search for blank spaces
	[[:unicode:]] - search for unicode chars
	["'?!;:#$%&()*+/<>=@\^_{}|~] - search for special characters, but not -, [, or ]. Search for [] by turning off Reg Exp searches. - and . are allowed as they part of addresses and ranges.  

In Notepad++, be sure that under Search Mode in Find/Replace Regular Expresses has been selected for the above Find commands to work.

--Errors--
Error message in messages.txt:
	Unrecognized EZproxy_IP_Blacklist_RejectIP.txt(1): ï»¿## EZProxy_IP_Blacklist_RejectIP.txt

This was encountered in a Linux environment, but did not impact RejectIP functionality. This is caused by a byte order mark at the start of the file, likely a result of the manner in which the file was created. In NotePad++, go to the Encoding menu option to confirm the encoding for your copy of EZproxy_IP_Blacklist_RejectIP.txt. You do not want it set to UTF-8 BOM. Changing the encoding to ASCII or UTF-8 and then using Save As should create a new file that will work, or try recreating the text file in a different manner. See this page for more information: https://www.w3.org/International/questions/qa-byte-order-mark 

--License--
This work is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License. http://creativecommons.org/licenses/by-sa/4.0/