---
id: 723
title: Enable Authentication in ElasticSearch
date: 2016-02-17T10:10:29+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=723
permalink: /archive/:year/:month/:day/:title/
categories:
  - ElasticSearch
---
  * [Introduction](#introduction) 
      * [Authentication with Shield](#authentication) 
          * [Enable SSL on ElasticSearch](#ssl) 
              * [Alternative&nbsp; way of creating a keystore](#alternative)</ul> 
            <a name="introduction"></a> 
            
            After you have configured your <a href="https://www.elastic.co" target="_blank">ElasticSearch</a> endpoint, the next important step is to make it secure. If you have a standard installation of ElasticSearch probably you have your ES endpoint pointing to <http://localhost:9200>. In my case I do have this configuration and this is how I can query into a test index:
            
            <pre>GET http://localhost:9200/test/persons/1 HTTP/1.1
User-Agent: Fiddler
Host: localhost:9200
Content-Length: 0</pre>
            
            And ElasticSearch will return an HTTP Status 200 OK with my resultset:
            
            <pre>HTTP/1.1 200 OK
Content-Type: application/json; charset=UTF-8
Content-Length: 196

{"_index":"test","_type":"persons","_id":"1","_version":1,"found":true,"_source":{
   "first_name":"Raffaele",
   "last_name":"Garofalo",
   "city":"Izmir",
   "e_mail":"raffaeu@gmail.com"
}}</pre>
            
            The major problem of the **default installation** of ES is that is totally unsecure.
            
              * First is running over an HTTP protocol, which means that all information sent are in clear text, including your authentication credentials&nbsp; 
                  * Second, there is no authentication mechanism in place, which means that **anybody** can get the data from my endpoint</ul> 
                So let’s see how we can enable Authentication and how we can enable SSL to make our ES endpoint secure.
                
                <a name="authentication"></a>
                
                ## Enable Authentication with Shield<sub></sub><sup></sup>
                
                One of the features that I really love about ES is the plugin architecture. You can easily install on an existing ES endpoint any of the available plugins with a couple of lines of code and configure them at runtime.
                
                One of the most important plugins available <a href="https://www.elastic.co/products/shield" target="_blank">for ES is Shield</a>****. Shield is a plugin that provides plenty of options to secure your ES endpoint, just to mention few:
                
                  * Enable Basic Authentication with Username and Password 
                      * Provide three different type of basic User [admin | power_user | guest] 
                          * Create custom roles with custom permissions 
                              * Enabling Message Authentication 
                                  * Auditing 
                                      * Custom authentication providers such as LDAP, OAuth, Active Directory and more</ul> 
                                    > **Note:**  
                                    > If you have multiple _nodes_ for your _cluster_, in order to have Shield running correctly you **must** stop all nodes and install shield over all your nodes. This is the only option available so you **must** take offline your ES while installing Shield
                                    
                                    In order to install Shield you should open an ElasticSearch SHELL, which in Windows means “_open a DOS console and point to your /bin folder where the elasticsearch program is available”:_
                                    
                                    <pre># browse the elastic search dir
$ CD C:\Program Files\elasticsearch-2.1.1\bin</pre>
                                    
                                    Then you have to install two plugins, **License** and **Shield**. This is required because Shield is **partially free**, which means that:
                                    
                                    > When your license expires, Shield operates in a degraded mode where access to the Elasticsearch cluster health, cluster stats, and index stats APIs is blocked. Shield keeps on protecting your cluster, but you won’t be able to monitor its operation until you update your license. For more information, see [License Expiration](https://www.elastic.co/guide/en/shield/current/license-management.html#license-expiration).
                                    
                                    <pre>$ plugin install license
$ plugin install shield</pre>
                                    
                                    Until you reboot your ES endpoint, you will not be able to have the Authentication enabled. After reboot, if you try to issue the previous request, you will get an HTTP 401 UNHAUTORIZED response:
                                    
                                    <pre>HTTP/1.1 401 Unauthorized
WWW-Authenticate: Basic realm="shield"
Content-Type: application/json; charset=UTF-8
Content-Length: 357

{"error":{"root_cause":[{"type":"security_exception","reason":"missing authentication token for REST request [/test/persons/1]","header":{"WWW-Authenticate":"Basic realm=\"shield\""}}],"type":"security_exception","reason":"missing authentication token for REST request [/test/persons/1]","header":{"WWW-Authenticate":"Basic realm=\"shield\""}},"status":401}</pre>
                                    
                                    So the next step is to create an Admin user that we can use to start to query our ES endpoint:
                                    
                                    <pre>#browse /bin/shield directory
$ esusers useradd es_admin -r admin</pre>
                                    
                                    With the esusers useradd command we created a new user named **es_admin** with the param –r we defined the role **admin** (_available roles are admin, power_user, guest_).
                                    
                                    Second step, we need to issue an authenticated request to ES using our account in the following way:
                                    
                                    <pre>GET http://localhost:9200/test/persons/1 HTTP/1.1
User-Agent: Fiddler
Host: localhost:9200
Content-Length: 0
Authorization: Basic ZXNfYWRtaW46c2VjcmV0</pre>
                                    
                                    Where the “strange” string is just _username : password_ combination, converted by <u>Fiddler in a 64 bit string</u>.
                                    
                                    All right, so now you have your ES secured and the basic user management in place but you have another problem. Now you are sending per each request a username and password over the web, so you must enable SSL communication on your ES endpoint to encrypt the communication.
                                    
                                    > **Note:**  
                                    > If you want to create custom roles with custom access, you need to modify the file [elasticsearch]/config/shield/roles.yml. In this file you can have a look at the current roles [admin, power_user, user] and modify them or create new one. For example you may have a role that grant permissions only to a specific index (_see multi-tenant architecture_).  
                                    > Remember, every time you modify a configuration file in ES you MUST restart the service otherwise ES will keep using the cached .yml configuration files.
                                    
                                    <a name="ssl"></a>
                                    
                                    ## Enable SSL on ElasticSearch
                                    
                                    First of all you need a X.509 certificate for your domain. There is no shortcut here, if you have a domain where you host your ES endpoint you must purchase an SSL certificate from a CA authority that covers your domain. This procedure is a bit painful and verbose but when done it’s done for your entire ES installation. _You can also create a cert file from your own Windows/Linux machine, for example, using OPENSSL or similar but remember that your certification file will not be signed by a known CA authority and you may encounter problems during production_.
                                    
                                    In my case I host ES in a public DNS and I have a Domain Certificate from a CA authority in the form of .pfx file. Using OPENSSL I create a new .pem file from my .pfx using this command:
                                    
                                    <pre>openssl pkcs12 -in certificate.pfx -out certificate.pem -nodes</pre>
                                    
                                    Anyway you must have access to a **.pem** certificate file in order to continue.
                                    
                                    <pre>keytool -importcert -keystore node01.jks -file cacert.pem -alias my_ca</pre>
                                    
                                    This command (<a href="http://docs.oracle.com/javase/7/docs/technotes/tools/solaris/keytool.html" target="_blank">keytool</a>) will import the certificate into the Java Keystore Database, which contains a list of trusted certificates.
                                    
                                    Next you need to generate a private key and a certificate for your node as following:
                                    
                                    <pre>keytool -genkey -alias node01 -keystore node01.jks -keyalg RSA -keysize 2048 -validity 712 -ext san=dns:node01.example.com,ip:192.168.1.1</pre>
                                    
                                    With this command we create a key and a public certificate valid for 712 days (_which is the classic example shown on ElasticSearch documentation_). This command will also prompt you for some questions that will be registered together with the certificate. The _san_ attribute allows to specify the DOMAIN where you are hosting your ES endpoint.
                                    
                                    So, right now you have a node’s certificate but it needs to be signed by a CA authority, actually the one who issued the certificate for you.
                                    
                                    <pre>keytool -certreq -alias node01 -keystore node01.jks -file node01.csr -keyalg rsa -ext san=dns:node01.example.com,ip:192.168.1.1</pre>
                                    
                                    Now we have a **Certificate Signing Request .csr** that we can send to our CA. The CA will sign it and returns to you a .crt file. You can also use OPENSSL and generate the .crt by yourself if you wish. At this point you have to import the .crt into your .jks database with the following command:
                                    
                                    <pre>keytool -importcert -keystore node01.jks -file node01-signed.crt -alias node01</pre>
                                    
                                    Now we can configure SHIELD to use SSL and HTTPS transport protocol. Just let ES knows where your keystore is located and which is the password for your keystore and for your certificate:
                                    
                                    <pre># ---------------------------------- SSL -----------------------------------
shield.ssl.keystore.path: C:\Program Files\elasticsearch-2.2.0\elasticsearch-2.2.0\config\cert\keystore.jks
shield.ssl.keystore.password: [keystore password]
shield.ssl.keystore.key_password: [certificate password]
shield.transport.ssl: true
shield.http.ssl: true</pre>
                                    
                                    At this point your Elastic Search will be hosted still on port 9200 (_if you didn’t override this setting_) but available only over HTTPS.
                                    
                                    <a name="alternative"></a>
                                    
                                    ## Alternative way of creating a keystore
                                    
                                    In my specific scenario I didn’t want to modify the certificate because my CA authority is quite strict so I didn’t have the option to import a .pem, modify and create a .csr and so on so I used an alternative way.
                                    
                                    First of all I have created my keystore:
                                    
                                    <pre>keytool -genkey -alias mydomain -keyalg RSA -keystore keystore.jks -keysize 2048</pre>
                                    
                                    Then I have imported by .pfx file as is, without any modification:
                                    
                                    <pre>keytool -importkeystore -srckeystore mypfxfile.pfx -srcstoretype pkcs12 
-destkeystore keystore.jks -deststoretype JKS</pre>
                                    
                                    Then I created a .csr request for my DNS but I will not send the request to my CA authority:
                                    
                                    <pre>keytool -certreq -alias mydomain -keystore keystore.jks -file mydomain.csr</pre>
                                    
                                    And finally I moved the two files, .jks and .csr into the config directory of elastic search and configured elastic search using the keystore password and the original certificate password. It worked like a charm and I didn’t need to send a .csr and import a .crt.