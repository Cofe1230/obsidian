
## windows cmd (powershell 아님)
```cmd
icacls.exe myec2.pem /reset 
icacls.exe myec2.pem /grant:r %username%:(R) 
icacls.exe myec2.pem /inheritance:r
```

