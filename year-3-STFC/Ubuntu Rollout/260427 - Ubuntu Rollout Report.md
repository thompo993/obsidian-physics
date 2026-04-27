# Summary 
All Digitiser except the prototype digitiser in the R80 Server room have been updated to firmware version **24.12.12.02** and software version **9.5.9.1**. A fix was implemented to prevent double IP address issues from occurring in the future. 

# Double IP Address issue: 
## Cause: 
Due to an issue between Linux clients and Microsoft DHCP servers caused by Linux sending the DHCP server what's called a "Client ID" and not the MAC address as it would have done historically.