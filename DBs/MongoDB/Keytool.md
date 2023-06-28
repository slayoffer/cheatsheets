Вам может понадобиться установка ключей от Яндекс Облака для работы с MongoDB. Для этого создайте на сервере отдельный каталог, скачайте ключ и добавьте ключ в `TrustStore`:

```
$ mkdir --parents ~/.mongodb
$ wget "https://storage.yandexcloud.net/cloud-certs/CA.pem" --output-document ~/.mongodb/root.crt
$ chmod 0644 ~/.mongodb/root.crtcd ~/.mongodb
$ sudo keytool -importcert \
             -alias YandexCA -file root.crt \
             -keystore YATrustStore -storepass superpass \
             --noprompt 
```

Загляните в `TrustStore` и посмотрите, что поместили туда:

```
$ keytool -list -storetype JKS -keystore /root/.mongodb/YATrustStore -storepass superpass
Keystore type: JKS
Keystore provider: SUN

Your keystore contains 1 entry

yandexca, Jan 16, 2023, trustedCertEntry,
Certificate fingerprint (SHA-256): E1:D5:3D:D1:D7:56:6D:0D:C6:91:C9:ED:6F:CA:0C:91:0F:58:B9:5D:4E:D7:F0:A9:58:AC:C7:67:A1:B2:49:37 
```

Аналогичным образом с помощью `keytool` можно посмотреть, что находится в дефолтных трастсторах java. По умолчанию он лежит в `/etc/ssl/certs/java/cacerts`. Используйте такую же команду:

```
sudo keytool -import -alias YandexCA \
    -file root.crt \
    -keystore /etc/ssl/certs/java/cacerts -storepass changeit 
```

Добавляем новые переменные касательно TrustStore (не нужно, если используем дефолтный TrustStore):

```
export JAVA_TRUST_STORE=/root/.mongodb/YATrustStore
export JAVA_TRUST_STORE_PASS=superpass 
```

Если запускать не из дефолтного TrustStore, то команде нужны следующие новые аргументы при старте приложения:

```
-Djavax.net.ssl.trustStore=${JAVA_TRUST_STORE}
-Djavax.net.ssl.trustStorePassword=${JAVA_TRUST_STORE_PASS}" 
```

Если всё будет правильно, то в логах увидим:

```
[INFO ] 2023-01-16 12:57:29.964 [cluster-ClusterId{value='63c549b904be095a339188a3', description='null'}-rc1a-mp8xkmipdaaad33u.mdb.yandexcloud.net:27018] connection - Opened connection [connectionId{localValue:1, serverValue:839}] to rc1a-mp8xkmipdaaad33u.mdb.yandexcloud.net:27018
[INFO ] 2023-01-16 12:57:29.965 [cluster-ClusterId{value='63c549b904be095a339188a3', description='null'}-rc1a-mp8xkmipdaaad33u.mdb.yandexcloud.net:27018] cluster - Monitor thread successfully connected to server with description ServerDescription{address=rc1a-mp8xkmipdaaad33u.mdb.yandexcloud.net:27018, type=REPLICA_SET_PRIMARY, state=CONNECTED, ok=true, minWireVersion=0, maxWireVersion=13, maxDocumentSize=16777216, logicalSessionTimeoutMinutes=30, roundTripTimeNanos=208885092, setName='rs01', canonicalAddress=rc1a-mp8xkmipdaaad33u.mdb.yandexcloud.net:27018, hosts=[rc1a-mp8xkmipdaaad33u.mdb.yandexcloud.net:27018], passives=[], arbiters=[], primary='rc1a-mp8xkmipdaaad33u.mdb.yandexcloud.net:27018', tagSet=TagSet{[]}, electionId=7fffffff0000000000000003, setVersion=3, topologyVersion=TopologyVersion{processId=63c53dc690618d0715d21903, counter=7}, lastWriteDate=Mon Jan 16 12:57:15 UTC 2023, lastUpdateTimeNanos=7479487460043}

[INFO ] 2023-01-16 12:57:29.966 [cluster-rtt-ClusterId{value='63c549b904be095a339188a3', description='null'}-rc1a-mp8xkmipdaaad33u.mdb.yandexcloud.net:27018] connection - Opened connection [connectionId{localValue:2, serverValue:840}] to rc1a-mp8xkmipdaaad33u.mdb.yandexcloud.net:27018 
```

При наличии ошибок в сертификатах:

```
[INFO ] 2023-01-16 12:50:55.660 [cluster-ClusterId{value='63c5482e543bbf7b33b30d3c', description='null'}-rc1a-mp8xkmipdaaad33u.mdb.yandexcloud.net:27018] cluster - Exception in monitor thread while connecting to server rc1a-mp8xkmipdaaad33u.mdb.yandexcloud.net:27018
com.mongodb.MongoSocketWriteException: Exception sending message
        at com.mongodb.internal.connection.InternalStreamConnection.translateWriteException(InternalStreamConnection.java:618) ~[mongodb-driver-core-4.2.3.jar!/:?]
        at com.mongodb.internal.connection.InternalStreamConnection.sendMessage(InternalStreamConnection.java:496) ~[mongodb-driver-core-4.2.3.jar!/:?]
        at com.mongodb.internal.connection.InternalStreamConnection.sendCommandMessage(InternalStreamConnection.java:327) ~[mongodb-driver-core-4.2.3.jar!/:?]
        at com.mongodb.internal.connection.InternalStreamConnection.sendAndReceive(InternalStreamConnection.java:277) ~[mongodb-driver-core-4.2.3.jar!/:?]
        at com.mongodb.internal.connection.CommandHelper.sendAndReceive(CommandHelper.java:83) ~[mongodb-driver-core-4.2.3.jar!/:?]
        at com.mongodb.internal.connection.CommandHelper.executeCommand(CommandHelper.java:33) ~[mongodb-driver-core-4.2.3.jar!/:?]
        at com.mongodb.internal.connection.InternalStreamConnectionInitializer.initializeConnectionDescription(InternalStreamConnectionInitializer.java:107) ~[mongodb-driver-core-4.2.3.jar!/:?]
        at com.mongodb.internal.connection.InternalStreamConnectionInitializer.initialize(InternalStreamConnectionInitializer.java:62) ~[mongodb-driver-core-4.2.3.jar!/:?]
        at com.mongodb.internal.connection.InternalStreamConnection.open(InternalStreamConnection.java:144) ~[mongodb-driver-core-4.2.3.jar!/:?]
        at com.mongodb.internal.connection.DefaultServerMonitor$ServerMonitorRunnable.lookupServerDescription(DefaultServerMonitor.java:188) [mongodb-driver-core-4.2.3.jar!/:?]
        at com.mongodb.internal.connection.DefaultServerMonitor$ServerMonitorRunnable.run(DefaultServerMonitor.java:144) [mongodb-driver-core-4.2.3.jar!/:?]
        at java.lang.Thread.run(Thread.java:833) [?:?]
```