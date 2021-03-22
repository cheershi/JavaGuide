# **使用 TrueLicense 生成License**

### 引用

[]: https://gitee.com/zifangsky/LicenseDemo/tree/master	"license"

### 依赖

```xml
<dependency>
    <groupId>de.schlichtherle.truelicense</groupId>
    <artifactId>truelicense-core</artifactId>
    <version>1.33</version>
    <scope>provided</scope>
</dependency>
```

### 使用JDK自带的 keytool 工具生成公私钥证书库

```sh
#生成命令
keytool -genkeypair -keysize 1024 -validity 3650 -alias "privatehibase" -keystore "privateKeys.keystore" -storepass "public_hic@123456" -keypass "private_hic@123456" -dname "CN=localhost, OU=localhost, O=localhost, L=SH, ST=SH, C=CN"

#导出命令
keytool -exportcert -alias "privatehibase" -keystore "privateKeys.keystore" -storepass "public_hic@123456" -file "certfile.cer"

#导入命令
keytool -import -alias "publichibase" -file "certfile.cer" -keystore "publicCerts.keystore" -storepass "public_hic@123456"
```

### licenseServer服务

调用 http://127.0.0.1:7000/license/generateLicense

```json
{
	"subject": "hibase",
	"privateAlias": "privatehibase",
	"keyPass": "private_hic@123456",
	"storePass": "public_hic@123456",
	"licensePath": "C:/license/license.lic",
	"privateKeysStorePath": "C:/license/privateKeys.keystore",
	"issuedTime": "2018-07-10 00:00:01",
	"expiryTime": "2021-03-19 23:59:59",
	"consumerType": "User",
	"consumerAmount": 1,
	"description": "这是证书描述信息",
	"licenseCheckModel": {
	
    "macAddress": ["04-6C-59-DB-1E-2E"],
    "cpuSerial": "BFEBFBFF000806EC",
    "mainBoardSerial": "W1KS0AC10GS"
    
	}
}


```

### 调用未认证服务

POST http://127.0.0.1:8080/license/getServerInfos

获取机器信息

```json
{
"macAddress": ["04-6C-59-DB-1E-2E"],
    "cpuSerial": "BFEBFBFF000806EC",
    "mainBoardSerial": "W1KS0AC10GS"
}
```

### 生成lic文件