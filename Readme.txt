--Welcome to the EZProxy IP Address Blacklist--

This information was formally located on The Unofficial EZproxy Self-Support Wiki: https://pluto.potsdam.edu/ezproxywiki/index.php/IP_Address_Blacklist

EZProxy_IP_Address_Blacklist_RejectIP.txt is a list of contributed IP addresses and ranges that have been used to access, or attempt to access, EZproxy servers in an illegitimate way. This information can be used in a number of ways to better secure EZproxy. EZProxy_IP_Address_Blacklist_IFIP.txt contains the same IP addresses presented EZProxy_IP_Address_Blacklist_RejectIP.txt, but in a different format to be used with the IFIP method outlined below. 

This list is maintained by Paul Butler at Ball State University. 
To contribute an IP address(es) to the list email them to: prbutler@bsu.edu

NOTE: EZproxy must be restarted for changes to config.txt to take effect.

--RejectIP--

For use with the file: EZProxy_IP_Address_Blacklist_RejectIP.txt

IP addresses and ranges can be used in config.txt as a simple list of RejectIP directives, like this:
	RejectIP 123.123.123.123
	RejectIP 41.42.0.0-41.42.255.255

You may also create a list of RejectIP statements in an external file and use an include directive in config.txt to include that file. For example, create the file EZProxy_IP_Address_Blacklist_RejectIP.txt in your EZproxy directory that contains your RejectIP statements. Then use the following statement in config.txt to include the file:

	IncludeFile EZProxy_IP_Address_Blacklist_RejectIP.txt

OCLC provides documentation on RejectIP here: http://www.oclc.org/support/services/ezproxy/documentation/cfg/rejectip.en.html

--IFIP--
For use with file: EZProxy_IP_Address_Blacklist_IFIP.txt

RejectIP does not create an entry in the EZproxy audit files for a rejected IP address. An alternative method to RejectIP for blocking and creating an audit entry has been documented on the Unofficial EZproxy Self-Support Wiki at the Restricted IP Logging and Response page: https://pluto.potsdam.edu/ezproxywiki/index.php/Restricted_IP_Logging_and_Response

This method, upon authentication, routes the user to a given html page (deny.htm in the example below) and writes a given messages in the audit file (whatever_ message_you_want in the example below). In User.txt the entry would be:  

IfIP 123.123.0.0-123.123.255.255; Audit whatever_ message_you_want; Deny deny.htm

IFIP allows a single IP address as well as IP ranges. You can also use an include directive to link to a file that contains a list of IfIP directives. 

OCLC provides documentation on IfIP at their Common Conditions and Actions page here: http://www.oclc.org/support/services/ezproxy/documentation/usr/common.en.html

--Additional Uses--
The IP addresses listed here may also be used at the server or network level to block access to EZproxy and other campus resources. The specific methods to do so are too varied to outline here; network security personnel or server administrators at your institution should be consulted.

--Load Balancer--
If you are running EZproxy behind a load balancer, it is likely that your EZproxy server will only get requests from the load balancer's pool of IP addresses. This list can be used to reject hosts, but it should be implemented in the load balancer's configuration itself.

--Best Practices--
A common practice for fraudulent users is to first login to a VPN with a compromised account from institution X, and while logged into the VPN use a second compromised account from institution Y to access institution Y's proxied resources. This practice is used to conceal the country from which they are accessing EZProxy and/or circumvent block country directives. When investigating a compromised account if you identify this situation it is a good practice to send an email to abuse@the.school.edu detailing the time/date and IP address used. You can quickly identify the abuse email address for an institution by performing a whois lookup on the IP address in question. Useful sites for this are: http://whois.arin.net/ and http://www.iplocation.net/. Another option is to block IP ranges for other institutions. Some institutions have listed their IP ranges here: https://github.com/bertrama/ezproxy-config/blob/master/reject-ip-vpn

Several public websites provide information about IP addresses, such as the ISP/provider or geographic location, which is helpful when investigating compromised user accounts. Such examples include: 							
	http://www.whatismyip.com/ip-address-lookup/?iref=cta 	
	https://www.projecthoneypot.org/search_ip.php
	http://www.ip-tracker.org/locator/ip-lookup.php
	
The RejectIP directive redirects a user to reject.htm. It is good practice to provide legitimate users a way to contact your institution when they arrive at reject.htm. Examples include a link to a chat function or an email address. For troubleshooting it is helpful to suggest to the user a method to identity their IP address. Most users are familiar with Google, so https://www.google.com/#q=what's+my+ip is a good link to include on reject.htm.

--Notes--
Bash command used to sort a list of IP addresses and IP ranges (only works on a file of IP addresses without RejectIP):
sort -t . -k 1,1n -k 2,2n -k 3,3n -k 4,4n Unsorted_List_of_IPs.txt > Sorted_List_of_IPs.txt

Method used to add RejectIP to the start of each line, by using find/replace in Notepad++:
Find: ^
Replace: RejectIP

Method used to add " ; Set Match = "True"" to the end of each line, by using find/replace in Notepad++:

Find: $
Replace:  ; Set Match = "True"

In Notepad++, be sure that under Search Mode in Find/Replace Regular Expresses has been selected for the above to work.