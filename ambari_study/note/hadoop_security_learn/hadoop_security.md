# 安全  

- Authentication(身份验证)  
- Authorization(授权)  
- Audit  
- Compliance

## Security(total)

1. 对一个 Hadoop 平台, 加入安全机制实现数据访问权限, 实现资源管控(一个集群): 
    1. 静态资源管控;  
    2. 动态资源配置: 资源池, Fair Schedule;
2. 云环境中构建出一个独立的环境。

## security  

- Perimeter(Authentication): CM, Ambari -- (Kerberos)  
- Access(Authorization): Sentry, Ranger  
- Visibility(Audit): Navigator, Ranger  
- Data(Compliance): Navigator Encryption, KMS, Ranger KMS  

### Authentication(Kerberos)  

1. Client.  
2. The Service Providing a Desired Network Service.  
3. The Kerberos Key Distribution Center(KDC).  

- theory  

- usage  

    1. KDC
        - Active Directory(AD)
        - MIT KDC  

type | Active Directory | MIT KDC  
---|---|---
Open source |  | y  
Commercial Support | y | 
High Available | y | y 
Easy to Maintain | 
LDAP Integration | 
Capability of Windows Integration | 
 
   2. Signal Domain Trust  

   3. Cross Domain Trust  

   4. Active Directory and Kerberos  

   5.     

### Authorization(Sentry)  

User -> Group  
Group -> Sentry Role  
Sentry Role -> Sentry Perm  

Users -> multi Groups  

Group 定义在 LDAP/System.  

### Audit(Navigator)  

    自动生成表的血缘关系。  

### Compliance(KMS)  

type | Cloudera | Open Source  
---|---|---  
HDFS Data Encryption | 
HBase Encryption | 
Log File Encryption | 
Metadata Encryption | 

