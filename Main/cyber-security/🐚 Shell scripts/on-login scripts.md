# Running scripts on login

To run scripts on logging into a shell, we have to put a script in the home directory with a costume name. This name, depending on the system can end in: `.login`, `.profile`, `.bashrc`, `.bash_profile`

To easily find which one it is, just make a script with each one of them, printing out the names:
```bash
echo "this is .profile"
```

#linux #linux_scripting 