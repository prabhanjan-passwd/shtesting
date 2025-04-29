# aap_25_migration_utils
Migration utilities for AAP 2.5.

How to import credentials into a different organization:

* Create a script containing the following:

```
#!/bin/bash

echo "from awx.main.models import Organization
for o in Organization.objects.all(): print(o.id, o.name)" \
| awx-manage shell
```

* Copy this into the automation-controller-web container:

```
podman cp /tmp/list_organizations.sh  automation-controller-web:/tmp/list_organizations.sh
```

* Create a wrapper script:

```
podman exec automation-controller-web /tmp/list_organizations.sh
```

* Run the command in your playbook:

```
[aap_installer@cvlappxp22053 ~]$ ./wrapper_Script.sh | egrep BA-54321 | awk {'print $1'}
1813
[aap_installer@cvlappxp22053 ~]$
```

* Use this primary key value retrieved from the database instead of the organization ID you see displayed in the GUI.


* The aapCreds.py will fail silently.  What do we do about this?
