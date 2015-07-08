--Welcome to the EZProxy IP Address Blacklist--

This information was formally located at: https://pluto.potsdam.edu/ezproxywiki/index.php/IP_Address_Blacklist

This is a list of contributed IP addresses and ranges that have been used to access, or attempt to access, EZproxy servers in an illegitimate way. This information can be used in a number of ways to better secure EZproxy.

EZproxy must be restarted for changes to config.txt to take effect.

--RejectIP--


--IFIP--


--Additional Uses--


--Load Balancer--


--Best Practices--


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