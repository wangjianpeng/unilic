USign 1.0.0
-

Verified to be working with:
    
* 2021.3.23 OSX
* 2022.2.16 Win

#### 在ubuntu中实现步骤
1. openssl req -config openssl.conf -newkey rsa:2048 -keyout private.key -out request.csr
2. openssl x509 -req -days 365 -in request.csr -signkey private.key -out certificate.crt
3. openssl x509 -in certificate.crt -outform der -out certificate.der
4.  ls -l certificate.der
5.  openssl x509 -in certificate.crt -pubkey -noout > mypub.key

Process:

1. Generate keypair and cert with provided `openssl.conf`. Cert size in DER format must be 1267 bytes (这里应该是unicode编码，一个字母占两个字节)
2. Replace cert in `Unity.Licensing.EntitlementResolver.dll` using HEX editor or `patch` binary
3. Modify `license.xml` template to your needs
4. Generate `Unity_lic.ulf` signed with private key using `sign` binary
5. `ts.go` is an implementation of `TimeStamp` encoder/decoder just in case.

Usage:

* `patch /path/to/Unity.Licensing.EntitlementResolver.dll`
* `sign`
* `cp Unity_lic.ulf /path/to/license/dir`

Block domains in `/etc/hosts`

```
# unity
127.0.0.1 license.unity3d.com
127.0.0.1 core.cloud.unity3d.com
127.0.0.1 activation.unity3d.com
127.0.0.1 cdp.cloud.unity3d.com

# this one seems to be optional
# blocking won't allow signing into Unity ID
127.0.0.1 api.unity.com
```
