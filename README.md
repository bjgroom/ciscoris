[![published](https://static.production.devnetcloud.com/codeexchange/assets/images/devnet-published.svg)](https://developer.cisco.com/codeexchange/github/repo/levensailor/py-cisco-ris)

# ciscoris

Uses Realtime Information Service (RIS) to capture registration status of Cisco IP Phones on CUCM
https://developer.cisco.com/docs/sxml/#risport70-api-reference

#### Install with pip or clone repo

```bash
pip install ciscoris
```

#### import ris, logcollection or other classes
```py
from ciscoris import ris
```

#### specify your CUCM details
```py
cucm = '10.0.0.1' 	# Your CUCM node
version = '11.5'  	# Your CUCM version
risuser = 'admin'	# Your CUCM user account
rispass = 'password'	# Your risuser password
```

#### instanciate your RIS object
```py
ris = ris(username=risuser,password=rispass,cucm=cucm,cucm_version=version)
```
---
#### input an array of phones

```py
voip = ['SEPF8A5C59E0F1C', 'SEP1CDEA78380DE', 'SEP01CD4EF58980']
```

#### Optional: input an array of "process nodes" or nodes which run Callmanager service
Omitting this argument queries all nodes in your cluster
```py
subs = ['10.0.0.1', '10.0.1.1', '10.2.0.1']
```

#### you can use the related `ciscoaxl` library grab process nodes via API.
```py
subs = [x['name'] for x in ucm.list_process_nodes()]
```

### check phones registration status.
```py
result = ris.checkRegistration(voip [, subs=None] )
```

#### group phones into 1000 and check registrations per group
```py
limit = lambda phones, n=1000: [phones[i:i+n] for i in range(0, len(phones), n)]

groups = limit(phones)
for group in groups:
    registered = ris.checkRegistration(group, subs)
    user = registered.CmNodes.item[0].CmDevices.item[x].LoginUserId
    regtime = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime(registered.CmNodes.item[0].CmDevices.item[x].TimeStamp))
    for item in registered.CmNodes.item[0].CmDevices.item:
        ip = item.IPAddress
	primeline = item.DirNumber
    	name = item.Name

	print('name: '+name)
	print('user: '+user)
	print('primary dn: '+primeline)
	print('ip address: '+ip)
	print('registration time: '+regtime)
```
