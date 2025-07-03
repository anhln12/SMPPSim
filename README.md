This repo is for self learn.  I modified the origial logging to Slf4j logging which helps me for debugging:)

SMPPSim official website tutorial:
http://www.seleniumsoftware.com/user-guide.htm#quick

REQUIERD SOFTWARE:
1. java environment java 1.6.x or later.
2. maven 2.x or 3.x

QUICK START

How to run?
java -jar smppsim.jar conf/logback.xml conf/smppsim.props

How to change credentials?
All credential information is in the conf/smppsim.props

***attention***:
the smpp-lib may not be downloaded, please get the maven dependency 
in the conf/ folder and put it into your local maven repository:
/.m2/respository/smpp/smpp-lib


# Add file smpp-lib-1.3.jar
mvn install:install-file -Dfile="D:\SourceCode\Java\SMPPSim\conf\smpp-lib\1.3\smpp-lib-1.3.jar" -DgroupId=smpp -DartifactId=smpp-lib -Dversion="1.3" -Dpackaging=jar -DgeneratePom=true -DcreateChecksum=true
mvn clean package


Enviroment
java -version
java version "20.0.1" 2023-04-18
Java(TM) SE Runtime Environment (build 20.0.1+9-29)
Java HotSpot(TM) 64-Bit Server VM (build 20.0.1+9-29, mixed mode, sharing)

mvn -version
Apache Maven 3.9.9 (8e8579a9e76f7d015ee5ec7bfcdc97d260186937)
Maven home: D:\apache-maven-3.9.9
Java version: 20.0.1, vendor: Oracle Corporation, runtime: C:\Program Files\Java\jdk-20
Default locale: en_US, platform encoding: UTF-8
OS name: "windows 11", version: "10.0", arch: "amd64", family: "windows"

# Test MO
1. Cấu hình SMPPSim để sinh MO
Mở file conf/smppsim.props enable tính năng mô phỏng MO
```
generateMO=true
moInterval=10000 # Khoảng cách giữa các tin MO (ms) là 10s
```

2. Run SMPPSim
```
java -cp target/smppsim.jar com.seleniumsoftware.SMPPSim.SMPPSim conf/logback.xml conf/smppsim.props
```

3. Kết nối SMPP client (MO Receiver)
Tạo một ứng dụng để bind đến SMPPSim như một ESME (Receiver), để nhận tin MO mà SMPPSim sinh ra.

Code python dùng smpplib, ví dụ:
```
import smpplib.client
import smpplib.consts

client = smpplib.client.Client('127.0.0.1', 2775)

client.set_message_received_handler(lambda pdu: print(f"Received message: {pdu.short_message}"))

client.connect()
client.bind_receiver(system_id='smppclient1', password='password')

while True:
    client.listen()
```

Kiểm tra logs đầu ra
- Mỗi 10s SMPPSim sẽ gửi một tin MO
- Bạn sẽ thấy output như:
```
Received message: This is a test MO message
```

Cần chỉnh nội dung MO gửi ra, trong smppsim.props bạn có thể thêm
```
moMessageText=Tin nhan tu thiet bi di dong (MO)
```


