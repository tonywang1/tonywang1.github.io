---
title: protobuf使用
date: 2019-10-12 15:41:13
urlname: you_know_pb
tags: [GRPC]
categories: GRPC
---
 
# Protocol Buffer初识
 
---


## 1.什么是protobuf
google开发的跨语言、跨平台、可扩展的可以序列化的数据结构，像XML一样，但是更小更快，更简单，你可以定义序列化的数据结构，然后用工具生成对应语言的源码，最后可以用各种语言对数据流结构进行读写。
## 2.protobuf优势在哪里
- 跨语言
- 跨平台
- 可扩展
- 小、快、简单
 
## 3.什么地方使用protobuf
- 数据压缩传输
- 数据压缩存储、持久化

## 4.在java中如何使用protobuf-maven插件可以直接生成java代码
1. 创建proto目录，并在里面创建bookdress.proto文件-内容如**代码4.1**
2. maven的xml中添加protobuf文件生成源码的代码-内容如**4.2**
3. 测试写入protobuf的**代码4.3**
4. 测试读取protobuf的**代码4.4**
```java 
代码4.1
syntax = "proto2";
package proto;
option java_package = "com.test.proto";
option java_outer_classname = "AddressBookProtos";
message Person {
    optional string name = 1;
    optional int32 id = 2;
    optional string email = 3;
    enum PhoneType {
        MOBILE = 0;
        HOME = 1;
        WORK = 2;
    }
    message PhoneNumber {
        optional string number = 1;
        optional PhoneType type = 2 [default = HOME];
    }
    repeated PhoneNumber phones = 4;
}
message AddressBook {
    repeated Person people = 1;
}
```

```xml 
代码4.2
  <dependencies>
    <dependency>
      <groupId>com.google.protobuf</groupId>
      <artifactId>protobuf-java</artifactId>
      <version>3.4.0</version>
    </dependency>
  </dependencies>

  <build>
    <extensions>
      <extension>
        <groupId>kr.motd.maven</groupId>
        <artifactId>os-maven-plugin</artifactId>
        <version>1.4.1.Final</version>
      </extension>
    </extensions>

    <plugins>
      <plugin>
        <groupId>org.xolstice.maven.plugins</groupId>
        <artifactId>protobuf-maven-plugin</artifactId>
        <version>0.6.1</version>
        <executions>
          <execution>
            <goals>
              <goal>compile</goal>
              <goal>test-compile</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <protocArtifact>com.google.protobuf:protoc:3.4.0:exe:${os.detected.classifier}</protocArtifact>
        </configuration>
      </plugin>
    </plugins>

  </build>
```

```java
代码4.3
package com.test;
import com.test.proto.AddressBookProtos;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
public class WritePerson {
    public static  final  String addr_book = "addr_book.file"; // 写入文件的名称
    static AddressBookProtos.Person PromptForAddress(int index) throws IOException {
        AddressBookProtos.Person.Builder person = AddressBookProtos.Person.newBuilder();
        person.setId(index);
        person.setName("proto test " + index);
        person.setEmail("a"+index+"@b.com");
        AddressBookProtos.Person.PhoneNumber.Builder phoneNumber = AddressBookProtos.Person.PhoneNumber.newBuilder().setNumber("110-"+index);
        phoneNumber.setType(AddressBookProtos.Person.PhoneType.HOME);
        person.addPhones(phoneNumber);
        return person.build();
    }

    public static void main(String[] args) throws Exception {
        AddressBookProtos.AddressBook.Builder addressBook = AddressBookProtos.AddressBook.newBuilder();
        try {
            addressBook.mergeFrom(new FileInputStream(addr_book));
        } catch (FileNotFoundException e) {
            System.err.println( " File not found.  Creating a new file.");
        }
        // 创建 address.
        addressBook.addPeople(PromptForAddress(1));
        addressBook.addPeople(PromptForAddress(2));
        addressBook.addPeople(PromptForAddress(3));
        // 保存到文件
        FileOutputStream output = new FileOutputStream(WritePerson.addr_book);
        addressBook.build().writeTo(output);
        output.close();
    }
}
```

```java
代码4.4
package com.test;
import com.test.proto.AddressBookProtos;
import java.io.FileInputStream;
public class ReadPeople {
    static void Print(AddressBookProtos.AddressBook addressBook) {
        for (AddressBookProtos.Person person : addressBook.getPeopleList()) {
            System.out.println("Person ID: " + person.getId());
            System.out.println("  Name: " + person.getName());
            if (person.hasEmail()) {
                System.out.println("  E-mail address: " + person.getEmail());
            }
            for (AddressBookProtos.Person.PhoneNumber phoneNumber : person.getPhonesList()) {
                switch (phoneNumber.getType()) {
                    case MOBILE:
                        System.out.print("  Mobile phone #: ");
                        break;
                    case HOME:
                        System.out.print("  Home phone #: ");
                        break;
                    case WORK:
                        System.out.print("  Work phone #: ");
                        break;
                }
               System.out.println(phoneNumber.getNumber());
            }
        }
    }
    public static void main(String[] args) throws Exception {
        AddressBookProtos.AddressBook addressBook = AddressBookProtos.AddressBook.parseFrom(new FileInputStream(WritePerson.addr_book));
        Print(addressBook);
    }
}
```


## 5.主要参考

- [ProtoBuf的官方](https://developers.google.com/protocol-buffers/)
- [ProtoBuf的maven集成插件](https://www.xolstice.org/protobuf-maven-plugin/examples/protoc-artifact.html)
- [google开发者文档](https://developers.google.com/protocol-buffers/docs/proto3)
 