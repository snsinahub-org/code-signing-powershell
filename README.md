# code-signing-powershell

## Generate self signed certificate

```sh
$params = @{
    Subject = 'CN=PowerShell Code Signing Cert'
    Type = 'CodeSigning'
    CertStoreLocation = 'Cert:\Localmachine\My'
    HashAlgorithm = 'sha256'
    FriendlyName = 'PowerShell Code Signing Cert'
}
$cert = New-SelfSignedCertificate @params
```

## Get certificate in Localmachine

```sh
Get-ChildItem -Path "Cert:\LocalMachine\My"
```
### Sample of output

```sh
Thumbprint                                Subject
----------                                -------
60367EDE02BFC7064B43DE2E32D52AB489847F0A  CN=PowerShell Code Signing Cert
```

## Convert the certificate to base64 

```sh
$cert = Get-ChildItem -Path "Cert:\LocalMachine\My\60367EDE02BFC7064B43DE2E32D52AB489847F0A"
$pwd = ConvertTo-SecureString -String "CERT_PASSWORD" -Force -AsPlainText

# export it as CFX file
Export-PfxCertificate -cert $cert -FilePath "c:/cert.pfx" -Password $pwd

# convert CFX to base64 string
$certContent = Get-Content -Path "c:/cert.pfx" -Encoding Byte
$certBase64 = [Convert]::ToBase64String($certContent)

# print certBase64
echo $certBase64
```
## Save cert password and base64 string as github secrets

- PFX_PASSWORD
- SELF_SIGNED_CERTIFICATE_B64

## Save cert subject as a github variable

The cert subject in this example is `CN=PowerShell Code Signing Cert`

- CERT_SUBJECT 

