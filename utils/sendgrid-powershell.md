```powershell
# Provide SendGrid user name, if you are using Microsoft azure you will find the same from the portal   
$sendgridusername = "sendgrid-user"

# Enter the send grid password Note: this is not recommended for production. In production, the password should be encrypted
$SecurePassword = ConvertTo-SecureString 'MySecUreP4ssW0rD' –asplaintext –force 
$cred = New-Object System.Management.Automation.PsCredential($sendgridusername, $SecurePassword)

$sub = "This is the test email"
$To = "phinotworking@github.com"
$body = "Hello, I was just checking to send email from powershell via sendgrid"
$From = "testsendgrid@mytestdomain.com"

Send-MailMessage -From $From -To $To -Subject $sub -Body $body -Priority High -SmtpServer "smtp.sendgrid.net" -Credential $cred -UseSsl -Port 587 -BodyAsHtml
```
