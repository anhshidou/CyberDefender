# Ở Q1 
C2 server ở đây có nghĩa là:
A command-and-control (C2) server is a main tool cyber threat actors have in their arsenal to launch and control cyber attacks. 
Threat actors use C2s to send commands to their malware and to distribute malicious programs, malicious scripts, and more. 
They also use them to receive stolen data that they exfiltrated from target servers, devices, websites, and forms. 
In short, C2s are the technical brain behind a threat actor’s malicious operations.

Nó được sử dụng để chạy các cuộc công kích vào 1 website, sử dụng chúng để phát tán các chương trình, phần mềm và đoạn code độc hại

# Ở Q2
Openwire là:
Cross language Wire Protocol
OpenWire is our cross language Wire Protocol to allow native access to ActiveMQ Classic from a number of different languages and platforms.
The Java OpenWire transport is the default transport in ActiveMQ Classic 4.x or later.

Vì vậy nên ta có thể hiểu rằng đây là dịch vụ vận chuyển các access tới ActiveMQ từ các ngôn ngữ và nền tảng khác nhau

# Ở Q3
Openwire exploit là:
This vulnerability may allow a remote attacker with network access to either a Java-based OpenWire broker or client to run arbitrary shell commands by manipulating serialized class types in the OpenWire protocol to cause either the client or the broker (respectively) to instantiate any class on the classpath.

Điều này khiến cho Attacker có thể điều khiển server từ xa thông qua lỗ hổng của Openwire Java base. Họ có thể chạy bất cứ dòng code độc hại nào mà họ muốn ở trong server đó

# Ở Q4
C2 Server giống như ở Q1
Bởi vì ELF thì sẽ là định dạng của 1 file assembly dùng để reverse engineering. Vì vậy nên ta có cơ sở để biết rằng nó là một C2 server

# Ở Q5
Ở bài này thì ta đi tìm và thấy dòng chmod +x docker. Ở đây có nghĩa là nó đã được quyền executation.

# Ở Q6
xml script bắt đầu từ "<?xml" vậy nên ta hoàn toàn có thể nhận ra class java nào đang tồn tại vì nó chỉ có 1 class duy nhất

# Ở Q7
Blog để đọc: https://www.malwarebytes.com/blog/business/2023/11/apache-activemq-vulnerability-used-in-ransomware-attacks
CVE-2023-46604 (CVSS3 score 10 out of 10): because OpenWire commands are unmarshalled, by manipulating serialized class types in the OpenWire protocol an attacker could cause the broker to instantiate any class on the classpath. The classpath is a parameter in the Java Virtual Machine or the Java compiler that specifies the location of user-defined classes and packages. This caused a deserialization of untrusted data vulnerability. To fix the issue it was necessary to improve the Openwire marshaller validation test.

To successfully exploit this vulnerability, three things are required:

Network access
A manipulated OpenWire “command” (used to instantiate an arbitrary class on the classpath with a String parameter)
A class on the classpath which can execute arbitrary code simply by instantiating it with a String parameter.

# Ở Q8
Các Articles để đọc: https://fidelissecurity.com/threatgeek/threat-intelligence/unveiling-apache-activemq-vulnerability/
https://attackerkb.com/topics/IHsgZDE3tS/cve-2023-46604/rapid7-analysis

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/3d7cb0f0-6cf0-42e7-a655-d1719e84947f)
