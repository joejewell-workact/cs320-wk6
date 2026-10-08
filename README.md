 # This project is for week 6 of cs320

The project consists of:
- an ansible playbook

## purpose practice installing and removing software

## Required software
- python 
- ansible core 

## inventory config  
```

[lamp_servers]
lampserver ansible_host=10.2.37.50 ansible_user=joe

[lamp_servers:vars]
ansible_python_interpreter=/usr/bin/python3
```


## How to run
