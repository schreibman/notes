ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-bd79rtn3	ou-vz8d-bd79rtn3	Infrastructure
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-2pc6rhkn	ou-vz8d-2pc6rhkn	FoundationalInfrastructure
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-rzcyy8q9	ou-vz8d-rzcyy8q9	PolicyStaging
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-135sz8lc	ou-vz8d-135sz8lc	Suspended
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-k6r6b9r5	ou-vz8d-k6r6b9r5	FoundationalSecurity
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-qefevu0w	ou-vz8d-qefevu0w	Dev
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-y56bhxjw	ou-vz8d-y56bhxjw	Prod
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-nvmm3s15	ou-vz8d-nvmm3s15	Exceptions
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-njv192gk	ou-vz8d-njv192gk	BusinessUsers
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-du0wkqw5	ou-vz8d-du0wkqw5	BusinessContinuity
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-svmn6y7b	ou-vz8d-svmn6y7b	IndividualBusinessUsers
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-atcm9ies	ou-vz8d-atcm9ies	test
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-ipj4svs7	ou-vz8d-ipj4svs7	Sandbox
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-j76uu4hp	ou-vz8d-j76uu4hp	Security
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-en56aq6g	ou-vz8d-en56aq6g	Transitional
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-f8bxcjzy	ou-vz8d-f8bxcjzy	Deployments
ORGANIZATIONALUNITS	arn:aws:organizations::111111111111:ou/o-qiwpjrsz4m/ou-vz8d-owyma33q	ou-vz8d-owyma33q	Workloads



But how to recurse?


import json
import subprocess

def list_ous(parent_id, indent=0):
    """Recursively list OUs under a given parent."""
    # Call the AWS CLI to list organizational units for the current parent
    result = subprocess.run(
        ["aws", "organizations", "list-organizational-units-for-parent", "--parent-id", parent_id],
        stdout=subprocess.PIPE,
        stderr=subprocess.PIPE
    )
    
    # If there is an error, print it and exit
    if result.stderr:
        print(f"Error listing OUs for parent {parent_id}: {result.stderr.decode()}")
        return
    
    # Parse the output to JSON
    ous = json.loads(result.stdout)
    
    # Print each OU and recurse if it has children
    for ou in ous.get('OrganizationalUnits', []):
        print(' ' * indent * 4 + f"{ou['Name']} (ID: {ou['Id']})")
        list_ous(ou['Id'], indent + 1)

# Start the recursive listing from the root
root_result = subprocess.run(
    ["aws", "organizations", "list-roots"],
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE
)

# Check for errors
if root_result.stderr:
    print(f"Error listing roots: {root_result.stderr.decode()}")
else:
    roots = json.loads(root_result.stdout)
    for root in roots.get('Roots', []):
        print(f"Root (ID: {root['Id']})")
        list_ous(root['Id'])



Provides:


ot (ID: r-vz8d)
Infrastructure (ID: ou-vz8d-bd79rtn3)
    Prod (ID: ou-vz8d-acs4u8gt)
    NonProd (ID: ou-vz8d-lwlgeuep)
FoundationalInfrastructure (ID: ou-vz8d-2pc6rhkn)
    Prod (ID: ou-vz8d-qgm1p0lw)
    NonProd (ID: ou-vz8d-5zvq5eb3)
PolicyStaging (ID: ou-vz8d-rzcyy8q9)
    Prod (ID: ou-vz8d-0me4smza)
    NonProd (ID: ou-vz8d-5s8cwm6t)
Suspended (ID: ou-vz8d-135sz8lc)
    Prod (ID: ou-vz8d-0f0vlynx)
    NonProd (ID: ou-vz8d-rgu2dly2)
FoundationalSecurity (ID: ou-vz8d-k6r6b9r5)
    Prod (ID: ou-vz8d-wygek3hl)
    NonProd (ID: ou-vz8d-n3ixy0ni)
Dev (ID: ou-vz8d-qefevu0w)
    Prod (ID: ou-vz8d-rn8qsq97)
    NonProd (ID: ou-vz8d-345l605k)
Prod (ID: ou-vz8d-y56bhxjw)
    Prod (ID: ou-vz8d-fh9hbk8w)
    NonProd (ID: ou-vz8d-utshh8pf)
Exceptions (ID: ou-vz8d-nvmm3s15)
    Prod (ID: ou-vz8d-faqj6trg)
    NonProd (ID: ou-vz8d-i1jmku3g)
BusinessUsers (ID: ou-vz8d-njv192gk)
    Prod (ID: ou-vz8d-krslm0j2)
    NonProd (ID: ou-vz8d-h54smbm9)
BusinessContinuity (ID: ou-vz8d-du0wkqw5)
    Prod (ID: ou-vz8d-tu4c92v2)
    NonProd (ID: ou-vz8d-95suwirx)
IndividualBusinessUsers (ID: ou-vz8d-svmn6y7b)
    Prod (ID: ou-vz8d-yaipda13)
    NonProd (ID: ou-vz8d-a7v9oeus)
test (ID: ou-vz8d-atcm9ies)
    Prod (ID: ou-vz8d-qr0ahc4h)
    NonProd (ID: ou-vz8d-l6weau99)
Sandbox (ID: ou-vz8d-ipj4svs7)
    Prod (ID: ou-vz8d-d2lo6c8r)
    NonProd (ID: ou-vz8d-lp4lw3oj)
Security (ID: ou-vz8d-j76uu4hp)
    Prod (ID: ou-vz8d-v5sygdpa)
    NonProd (ID: ou-vz8d-s19jk6hh)
Transitional (ID: ou-vz8d-en56aq6g)
    Prod (ID: ou-vz8d-9k6zzf91)
    NonProd (ID: ou-vz8d-lrilv72y)
Deployments (ID: ou-vz8d-f8bxcjzy)
    Prod (ID: ou-vz8d-yzn0rdxr)
    NonProd (ID: ou-vz8d-800bzl3g)
Workloads (ID: ou-vz8d-owyma33q)
    Prod (ID: ou-vz8d-mkoz2y7j)
    NonProd (ID: ou-vz8d-jlyhrjnq)
[cloudshell-user@ip-10-6-83-156 ~]$ which py
/usr/bin/which: no py in  

to create 


aws organizations create-organizational-unit --parent-id ou-vz8d-lwlgeuep --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-lwlgeuep --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-5zvq5eb3 --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-5zvq5eb3 --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-5s8cwm6t --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-5s8cwm6t --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-rgu2dly2 --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-rgu2dly2 --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-n3ixy0ni --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-n3ixy0ni --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-345l605k --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-345l605k --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-utshh8pf --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-utshh8pf --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-i1jmku3g --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-i1jmku3g --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-h54smbm9 --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-h54smbm9 --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-95suwirx --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-95suwirx --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-a7v9oeus --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-a7v9oeus --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-l6weau99 --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-l6weau99 --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-lp4lw3oj --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-lp4lw3oj --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-s19jk6hh --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-s19jk6hh --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-lrilv72y --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-lrilv72y --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-800bzl3g --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-800bzl3g --name Test
aws organizations create-organizational-unit --parent-id ou-vz8d-jlyhrjnq --name Dev
aws organizations create-organizational-unit --parent-id ou-vz8d-jlyhrjnq --name Test


