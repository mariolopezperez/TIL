# Find process on Windows/Linux

- For Microsoft Windows:
    
    `netstat -ano | find "1234" | find "LISTEN"tasklist /fi "PID eq 1234"`
    
- For Linux:
    
    `netstat -anpe | grep "1234" | grep "LISTEN"`
    
    or
    
    `lsof -i:443`
