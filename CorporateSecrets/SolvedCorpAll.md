# Q1

What is the current build number on the system ?

Vậy nên, ở bài này, có lẽ họ đang bảo ta tìm được những thứ như vậy. Vậy thì để xem các thông tin này ở đâu, ta dùng google để tìm hiểu thêm

Keyword: ``` Phần lớn thông tin được lưu giữ ở đâu trong Windows ```

https://quantrimang.com/cong-nghe/tim-hieu-ve-windows-registry-phan-i-2077

Vậy là ta được biết đến Registry. Registry là 1 CSDL được dùng để lưu trữ thông số kỹ thuật trên Windows. Nó ghi nhận tất cả thông tin và cài đặt cho những phần mềm bạn cài trên máy.

Registry được chia ra làm các hive khác nhau:

**HKEY_LOCAL_MACHINE\ SYSTEM: \system32\config\system**

**HKEY_LOCAL_MACHINE\ SAM: \system32\config\sam**

**HKEY_LOCAL_MACHINE\ SECURITY: \system32\config\security**

**HKEY_LOCAL_MACHINE\ SOFTWARE: \system32\config\software**

**HKEY_USERS\ UserProfile: \winnt\profiles\username**

**HKEY_USERS.DEFAULT: \system32\config\default**

Vậy là ta đã có một nửa câu trả lời, tức là hiện tại ta sẽ phải kiểm tra xem các hive này. 

Tiếp theo, để tìm windows build number ở trong registry. Hive cần tìm là 

``` HKEY_LOCAL_MACHINE\SOFTWARE```

Cụ thể là

``` HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion ```

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/629aa2d7-ed25-44d5-b39f-d77fdf344a21)

Vậy, nhờ vào đó, ta chỉ cần tìm đúng hive SOFTWARE và export nó về máy. Sau đó sử dụng RegRipper

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/9787963f-2e55-436b-bf4c-bb45bb372c07)

Đáp án ở đây là 16299.

-------


# Q2


Tiếp tục đến bài thứ 2, ta phải tìm users


https://stackoverflow.com/questions/2919286/getting-the-username-from-the-hkey-users-values

Sau khi tìm hiểu cái link trên, thì em biết được rằng để tìm danh sách user, hive ta cần tìm là:

``` HKLM\Software ```

Cụ thể:

``` HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList ```

Vậy, sử dụng lại dumphive từ câu 1, ta chỉ cần sử dụng Keyword ở đây là ProfileList vì nó sẽ giúp liệt kê ra danh sách Profile của người dùng ở trên máy.

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/78a5c147-d406-4e5b-b70b-45a8399c6d07)

Đáp án: 6

-----

# Q3 

Đi tìm hash của fruit_apricot.jpg


Lúc này ta quay lại với FTK Imager và đến profile của người tên apricot này theo dữ liệu đã có ở bài 2

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/b8419342-d169-4617-a62a-68ac66862c38)

Ta thấy rằng đây chỉ là 1 bức ảnh bình thường

Sử dụng ```find . -name "fruit_apricot.jpg" -exec 7z h -scrcCRC64 {} \;```

**find . -name "fruit_apricot.jpg"** là câu lệnh sử dụng để tìm file tên là fruit_apricot.jpg trong directory hiện tại và cả các subdirectories

**-exec** là execute

**7z h - scrcCRC64** là sử dụng 7zip để tính hash bằng command option là *h* và scrc sử dụng để chỉ định 1 hash mà bạn muốn dùng, ở đây ta muốn dùng function của hash CRC64

**{}**  được sử dụng để find tìm thẳng vào tên file ảnh 

**\;** đây là end arguments của câu lệnh exec.

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/2f2c812c-5e82-428d-90c1-933223d2dde6)

------

# Q4

Ở bài này, ta phải đi tìm size của bức ảnh strawberry.jpg


Ở bài này, ta export ảnh về và xem Size ảnh 

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/942e420f-a090-44bc-946c-d142f41703ba)

**Lưu ý: Đề bài bảo tìm size chứ không phải size on disk**

------

# Q5

tìm kiến trúc của processor


Ở bài này ta phải đi tìm kiến trúc của processor theo kiểu gì, có rất nhiều loại kiến trúc mà ta đã được học từ môn CEA201 của FPTU, ví dụ như:

ARM, x86, Xeon,...

Tương tự câu 1, ở đây registry hive nằm ở HKLM\SYSTEM

Cụ thể là: ``` HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Environment ```

Quay lại với RegRipper rồi search keyword

Tìm Keyword là Processor_Architecture

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/e2551284-8360-4e09-a2b1-ad9b03848dc6)

Và ta đã có đáp án là AMD64

------

# Q6

Tấm ảnh của con chó ở trong thùng rác là của người nào ?

Bài này thì ta đi tìm đơn giản thôi.

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/744ac000-00b2-4aec-a7ae-77c0f7c75d18)

Ta sử dụng SID để tìm ra người xóa

SID được sử dụng để xác thực bảo mật Microsoft Windows. Đồng thời, hệ điều này này cũng dựa vào SID để thực hiện nhận dạng tài khoản trên máy tính cục bộ. Mặc dù tên tài khoản người dùng thay đổi nhưng SID vẫn có thể xác định được tài khoản đó.

https://bkhost.vn/blog/security-identifier-sid/#:~:text=SID%20l%C3%A0%20g%C3%AC%3F%20SID%20%E2%80%93%20Security%20Identifier%20trong,m%E1%BB%99t%20qu%C3%A1%20tr%C3%ACnh%20hay%20m%E1%BB%99t%20nh%C3%B3m%20b%E1%BA%A3o%20m%E1%BA%ADt.

Định dạng của SID

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/430fd74b-1e34-4f7f-88eb-b2348872a8c6)

Vậy là cái dòng trên của ta nhìn là SID, vì vậy nên lúc này, quay lại câu 1, track bằng SID

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/0e8089f0-d834-46ab-9596-db85f48124f9)

Sử dụng Ctrl + F cùng keyword là ta sẽ ra người dùng

Đáp án: hansel.apricot

-------

# Q7


Định dạng file của vegetable là gì ?

Ở đây thì ta đi tìm file vegetable.

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/0e87fc38-e17c-4789-a795-b312cfec1f56)

Vậy, nếu muốn biết 1 định dạng file, thì ta cần phải hiểu cái gì đầu tiên ? 

Trước hết, là File Signature

https://en.wikipedia.org/wiki/List_of_file_signatures

File signature được sử dụng để xác định và xác thực dữ liệu nội dung của 1 file. Ta có thể hiểu đơn giản là Magic Bytes. Ở bức ảnh trên, ta thấy dòng đầu 7z

HEX bắt đầu của 1 file 7zip là: 37 7A BC AF 27 1C

Của file bên trong FTK là: 37 7A BC AF 27 1C

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/f5e34cf0-2180-4ecf-a688-927c9aab4854)

Vậy là, từ đó ta có thể biết được rằng đây là 1 file 7z.

**Bonus: 7z là 1 file nén bằng 7zip, một loại công cụ nén tệp mã nguồn mở. Nó tương tự như RAR, ZIP và ISO nhưng với độ nén tốt hơn và được sử dụng mã hóa AES-256. Đây cũng là 1 kiểu nén tệp phổ biến ở ngày nay**

------

# Q8

Loại con gái mà Miriam Grapes đang design điện thoại cho là gì


Trước hết, ta phải đi tìm xem Miriam Grapes đang dùng trình duyệt gì, sau khi tìm hiểu thì ta thấy người này sử dụng Firefox. 

Thứ hai: History data trong Firefox phần lớn lưu trữ trong SQLite

https://www.foxtonforensics.com/browser-history-examiner/firefox-history-location

Thứ ba: ta biết rằng Downloads data được lưu trong SQLite là places.sqlite

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/63e571ed-9307-466b-abf7-fa119fa60e5a)

Ở đây, vì đã biết được những dữ kiện trên, ta sử dụng tool là DB Browser for SQLite. Bật database lên và tìm từng table một. Đáp án ở moz_places vì đây là nơi lưu trữ tất cả Downloads data khi ta download từ Firefox

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/1b00eb7f-56ba-43dd-b2d4-3cf081b225cc)

Đáp án là vsco

-----

# Q9

Tên của thiết bị

Hmm, ta có thể thấy rằng có thể sử dụng windows log để tìm ra device name

https://superuser.com/questions/1539088/find-hostname-of-an-windows-image#:~:text=As%20soon%20as%20you%20join%20a%20domain%2C%20it,in%20the%20file%20where%20joining%20the%20domain%20occurred.

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/1b927250-c694-44d5-847e-71d2901aca3a)

Thường thì log được lưu trong system32/winevt. Lúc này thì ta đi tìm bất kì 1 file logs nào rồi extract nó về máy xong bật lên, tìm một chút là ta có thể thấy

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/e4fc5815-a7c6-4243-bf5b-184504e74ed8)

-----

# Q10

SID của machine

Em xin phép không giải thích vì SID đã được giải thích ở Question trước đó. Vậy thì, để tìm SID của machine, ta tìm ở đâu ?

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/41467dff-ff22-4912-9738-d3f39e833814)

Câu trả lời ở đây là SAM. Vậy thì lại làm như các bài trước, ta tiếp tục sử dụng regripper và ngay lập tức thấy luôn đáp án

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/3887e6c7-4a3a-4061-99cc-a6a0ec861c63)

Tại vì sao em biết đây là đáp án ? Vì đây là administrator. Lúc này ta chỉ cần nhập đáp án là xong


-----

# Q11

Có bao nhiêu trình duyệt web đang được hiện diện

Sau khi google một chút thì em có tìm được cái này

https://stackoverflow.com/questions/2370732/how-to-find-all-the-browsers-installed-on-a-machine

Đại khái là, nếu để tìm các trình duyệt web có ở trong máy, ta tìm ở HKEY_LOCAL_MACHINE\SOFTWARE\Clients\StartMenuInternet

Tuy nhiên, nó không hiệu quả, khi ta tìm thì nó sẽ không hiện tất cả mà chỉ hiện có 1 trình duyệt duy nhất là IE

Lúc này thì ta sẽ phải đi tìm 1 cách khác. Ở đây, đọc đề bài thì ta biết được rằng mục đích là tìm các trình duyệt đang chạy, lúc này ta tìm hiểu SRUM

SRUM là Windows System Resource Utilization Monitor: là 1 tính năng của windows giúp tracking mức độ sử dụng của ứng dụng, network utilization và system energy state

https://www.magnetforensics.com/blog/srum-forensic-analysis-of-windows-system-resource-utilization-monitor/

Lúc này, ta sử dụng tool gợi ý của bài là srum_dump2 và tìm các browser đang được chạy

Ta có các browser sau:
- Tor
- Microsoft Edge
- Mozilla Firefox
- Chrome
- Internet Explorer

Vậy, đáp án ở đây là 5 browsers.

------

# Q12

Có bao nhiêu bí mật mà Tim có


Ở bài này, ta chú ý vào Tim, ở đây trong bài là tim.apple. Thường thì để lưu một document, ta sẽ để nó ở bên trong folder Document cho dễ tìm, nhờ việc đó, em đã tìm được gì đó

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/fb42f0b1-adbf-4cda-b9f8-29bac11f5049)

Xem qua các magic bytes, ta thấy đây là file PKZIP. Vậy là ta phải giải nén nó ? Thực chất là không phải vậy, nếu như để ý kĩ thì có 1 đoạn text bị đổi màu để không cho chúng ta nhìn thấy, lúc này ta cần phải chú ý kĩ, và khi đã tìm được thì ta có thể chọn màu bất kì và nhận được đáp án

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/68b14dca-0e32-4c60-83e0-c0edbf740434)

Đáp án: 4

------

# Q13


Tên của người nhân viên mà Tim muốn sa thải là gì ?


Xem lại bức ảnh thì câu số 12, ta có thể tên được giấu ở đây là Jim Tomato

------

# Q14

Đâu là last used username ?

Như ta đã biết, ở bài này có 5 usernames. Lúc này công việc của ta khá đơn giản, đó là tìm ở SAM

Vậy là, ta sẽ tìm ở hive là SAM

Giải thích lại SAM thì: SAM là Security Account Manager. Đây là 1 file CSDL có ở trong các đời Windows, được dùng để lưu trữ thông tin người dùng. Đồng thời, nó cũng được dùng để xác minh người dùng local và người dùng remote

Ta check SAM, sau đó so sánh giữa thời gian cuối cùng của các username để tìm ra câu trả lời

tim.apple: Sun Apr 12 00:59:31 2020
jim.tomato: Sun Apr 12 04:17:03 2020
suzy.strawberry: Sun Apr 12 03:09:25 2020
hansel.apricot: Sun Apr 12 02:08:59 2020
miriam.grapes: Sun Apr 12 01:17:13 2020

Vậy, ta có thể thấy, người login cuối cùng là jim.tomato

-------

# Q15

Người mà Tim đang gạ gẫm nắm giữ vai trò gì ?


![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/037ecc9c-670f-4b4d-b930-e6dbd2b7fae4)


BRUHHHHHHHHHHHHHHHHHHHHHh, vậy đáp án là secretary

Ở bài này, cách làm thì giống ở bài nào đó trước mà em không nhớ lắm. Nên chỉ có như này thôi:))

-------

# Q16

SID của Suzy.Strawberry

Ở đây thì ta lại tiếp tục xem SAM

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/e52e9154-c055-4437-a26e-00744659dfac)

Thì một SID bao gồm những phần như sau:

- Revision is the version level of the SID and is required to be initialized as 0x01.
- SubAuthorityCount is the number of subauthorities in the SID.
- IdentifierAuthority is the entity that created the SID, usually represented as three 16-bit numbers separated by hyphens.
- SubAuthority is a component of the SID's domain, and an SID may include up to 15 subauthorities, each of which is identified by a 32-bit subauthority value.


Ví dụ như, SID của Suzy là: S-1-5-21-2446097003-76624807-2828106174-1004

Vậy thì,

**S-** là prefix

**1** là revision level

**5** là identifier authority, có 5 giá trị valid là:
- 0 null authority
- 1 world authority
- 2 local authority
- 3 creator authority
- 4 nonunique authority
- 5 NT authority

**21-2446097003-76624807-2828106174** là domain identifier, cái xác thực 1 domain

**1004** là 1 ID liên kết để lọc ra các group admin domain bên trong 1 domain chính

------

# Q17

Tìm file path nơi tải của Tor Browser

Tìm đơn giản ở hive system, search keyword là TorBrowser là ra

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/fb975592-9e89-48d2-9dac-33e0c3803289)

------

# Q18

Hmm, ở đây ta phải đi tìm URL của video mà Tim đã xem

Sau khi check qua Firefox hay IE thì em đều không thấy Youtube nào, lúc này chỉ còn Chrome để tìm

Lúc này, ta làm giống như ở khi ta tìm trên Firefox vậy. 

Ở ví dụ dưới, ta thấy, dữ liệu lịch sử ở trên chrome được lưu ở C:\Users\user\AppData\Local\Google\Chrome\User Data\Default\History.

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/b786c8c7-9cad-49e4-b1af-9e17b206e59d)

Ở bài này, ta có dữ kiện quan trọng nhất là user là Tim. 

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/63b7d154-38e4-4dc5-8cf7-809d785105eb)

Khi ta cố gắng xem ở History theo Database, ta thấy rằng kết quả sẽ không có gì cả

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/6c0d7681-244e-4f13-ac54-3ad0f98e0f82)

Vậy là, lúc này nó sẽ không nằm ở History, vậy có lẽ nó sẽ nằm ở cả Default thì sao ? Ở nơi mà chúng ta không biết

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/3d973efa-70a0-4ea7-b28e-f729266db290)

Vậy là vẫn chưa ra, vậy thì hãy thử mở rộng phạm vi tìm kiếm... Em nhầm tên User... Phải là Jim mới đúng...

Khi mà đổi lại tên user... Thì ta chỉ cần từ làm theo đúng như cách đầu tiên ta làm, rồi sử dụng DBbrowser là được

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/41ef365d-8297-4c77-bcb7-f39620ba97eb)

Đáp án: https://www.youtube.com/watch?v=Y-CsIqTFEyY

------

# Q19

Ở bài này thì ta chỉ cần tìm các file của từng user là ra


![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/c1efa73c-20b3-489e-9edf-a00b84a08da5)

Đáp án là miriam.grapes

------

# Q20

Số lần admin login

Xem ở hive SAM là bạn sẽ ra

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/e7a71353-08bc-4c54-8ed9-3490c44f1112)

Đáp án là 10

-------

# Q21

tìm DHCP domain

DHCP chính là từ viết tắt của cụm từ Dynamic Host Configuration Protocol (được dịch là Giao thức Cấu hình Host Động). Theo đó, DHCP là giao thức có chức năng cấp phát địa chỉ IP cho tất cả các thiết bị truy cập trên cùng một mạng thông qua máy chủ DHCP được tích hợp trên router. 

Bên cạnh đó, DHCP còn có nhiệm vụ cấp thông số cần thiết của mạng đến các thiết bị. Cụ thể là thông tin về subnet mask, default gateway và dịch vụ DNS.

https://hostingviet.vn/dhcp-la-gi

Cách tìm thì:

Navigate to the following path:
   ```
   HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces
   ```

Vậy, ta chỉ cần tìm keyword là Interfaces

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/7c67da62-fc85-447e-a283-5e98512830c4)

Sau đó tìm DHCP Domain ở dưới là: fruitinc.xyz

------

# Q22


Ở bài này, ta đi tìm ảnh bìa

Bài này thì ta chỉ cần đi tìm tí là ra, vì chỉ có 1 bức ảnh duy nhất của user này

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/f7c6dad1-2aa2-414e-b7a6-32124666dbfd)

Đáp án: 04/05/2020 03:49


-----

# Q23

Jim đã chạy Tor bao nhiêu lần


Lúc này, ta cần nên biết về file ntuser

https://www.howtogeek.com/401365/what-is-the-ntuser-file/

Hidden in every user profile is a file named NTUSER.DAT. This file contains the settings and preferences for each user, so you shouldn't delete it and probably shouldn't edit it. Windows automatically loads, changes, and saves the file for you.

Khi ta tìm thử Tor Browser, em thấy một thứ khá thú vị.

```C:\Users\jim.tomato\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Start Tor Browser.lnk C:\Tor Browser\Browser\firefox.exe ```

Vậy là có thể mỗi lần chạy firefox.exe là 1 lần chạy Tor.

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/3fce3115-54db-49f1-a95f-c9e619417fe0)

Sau khi tìm hiểu qua về Tor, em được biết rằng Tor được chạy với base là Mozilla Firefox. Vậy ở đây đã làm sáng tỏ điều đó. Lúc này ta chỉ cần tìm count

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/5114099e-70a3-4c79-85ed-3a1d8b040c00)

Đáp án ở đây đã có là 2

------

# Q24

có 1 bức ảnh PNG ở trong iphone của file của Grapes


Kĩ thuật giấu file trong 1 file được gọi là Steganography, đây là 1 kĩ thuật mã hóa bên cạnh Cryptography nhưng ở một mức độ đơn giản hơn.

Có khá nhiều công cụ để có thể tìm ra những file cất giấu bên trong 1 bức ảnh.

Ở bài này, ta có thể sử dụng binwalk

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/8a1ad32f-302d-4d0d-a20f-f5ad0a4583e4)

Ta có thể thấy, có 1 file tên là 174A PNG ở trong đó

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/bbe69f93-29de-4280-a506-af7ee29568b7)

Lúc này ta có thể dùng tool online để checksum

https://emn178.github.io/online-tools/sha1_checksum.html

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/7ce9c9a0-d2b3-42f7-bc8f-03aa8b6b2b39)

Đáp án: 537fe19a560ba3578d2f9095dc2f591489ff2cde

-----

# Q25

Ở bài này, ta có thể tìm từng user một để tìm ra file docx


![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/ea889477-f5ef-4736-9213-f8b8b5ffc91f)

Vậy là ta đã tìm thấy file docx và user làm jim tomato. Nhớ lại những công việc này được lưu trữ bên trong file ntuser.dat. Export Ntuser.dat của user Jim Tomato

Sử dụng keyword để tìm là docx

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/9e4eb45e-05ca-4fbd-8dc9-da4d9b3c87a7)

Đáp án: 2020-04-11 23:23:36

-----

# Q26

MFT file


https://learn.microsoft.com/en-us/windows/win32/fileio/master-file-table

Có thể hiểu MFT file ở đây là nơi lưu trữ tất cả thông tin của file, từ kích cỡ, thời gian và cả quyền hạn, v.v

Thường thì 1 file MFT sẽ là $MFT.

Công cụ phổ biến để dump 1 file mft là mftdump. Đây cũng là công cụ mà em đã dùng trong HTB Apocalypse 2024 để giải bài :))

``` .\MFTECmd.exe -f '.\$MFT' --csv E:\CTF\TrainingEHC\CorporateSec\ --csvf mft.csv ```

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/b4cb8551-0a2e-444d-a387-623f825d580c)

Đáp án là: 219904

-----

# Q27

Bài này thì giống một bài nào đó mà em đã làm, nên nó khá nhanh

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/fefa62b1-510c-4b9c-a8dd-7f036d503d61)

Đọc hiểu chút là ta có thể biết được đáp án

Đáp án: stinky

-----

# Q28

Cloud service nào là item startup cho user Admin

Như ta đã biết, ở bài trước, để tìm dữ liệu của 1 ứng dụng của 1 người dùng, thì ta sử dụng file NTUSER.dat

Hive để lưu startup item

```HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run```

Dùng keyword ở đây là Run, là ta sẽ ra

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/f9fc4145-f5f2-4fbd-97d6-18d8a26c7fe0)

**Tại sao em biết là OneDrive thì:**

OneDrive là 1 kho lưu trữ trực tuyến (cloud) giúp người dùng có thể lưu trữ thông tin một cách bảo mật và có thể kết nối đến chúng ở bất cứ đâu.

https://support.microsoft.com/en-us/office/what-is-onedrive-9b821fd3-e93d-4268-99a5-641310dde54e

**Cloud Service là gì ?**

A cloud service is an IT service that is delivered to users on-demand via the internet from third-party servers - the “cloud”. These services are designed to provide easy, scalable access to applications, resources, and services, and are fully managed by a cloud service provider.

Cloud services can include:

- Software as a Service (SaaS): This includes applications that are accessed via a web browser. Users do not need to worry about hardware or software updates.
- Platform as a Service (PaaS): This service provides a platform for developers to build, test, and manage their own applications.
- Infrastructure as a Service (IaaS): This includes automated and scalable compute resources, complemented by cloud storage and network capability.

The main advantage of cloud services is that they eliminate the need for users to have their own hardware or infrastructure, and provides them with a cost-effective, scalable, and reliable alternative.

------

# Q29

Prefetch file



Kể từ Windows XP, Windows tạo file prefetch mỗi khi bạn chạy ứng dụng lần đầu tiên. File này chứa dữ liệu mà hệ điều hành cần để tăng tốc thời gian load của ứng dụng bất cứ khi nào bạn chạy nó. Và đây là một sự trợ giúp lớn trong quá trình khởi động vì nó giúp Windows load nhanh hơn.

Prefetch Folder thường nằm ở Windows

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/867a9bec-bf14-44d6-9649-f72a7c7f565b)

Sử dụng công cụ winprefetchview, ta tìm được như sau:

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/56abf3d1-c8fc-4060-afc0-bf7e13ef5dab)

Vậy đáp án là: FIREFOX.EXE-A606B53C.pf/21

-------

# Q30

Ở bài này, ta nhớ về bài DHCP lúc trước.

Làm như các bước giống như bài đó

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/e9f633fa-876d-40b9-b754-554dcf95eb9e)

Ở đây ta chỉ thấy xuất hiện 1 IP Address. Vì vậy nên ta có thể biết được luôn đáp án

Đáp án: 192.168.2.242 

-------

# Q31

Tìm pinned items

https://superuser.com/questions/171096/where-is-the-list-of-pinned-start-menu-and-taskbar-items-stored-in-windows-7

Sau khi search google thì em đã tìm ra

``` %AppData%\Microsoft\Internet Explorer\Quick Launch\User Pinned\TaskBar ```

Lúc này thì ta chỉ cần tìm của từng user trong FTK

Admin:

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/82ffd9f3-a887-4ed8-b60b-73387ae46084)

Hansel.Apricot:

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/96d72796-6810-4716-a2c0-e58b0f773632)

Jim:

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/a086c848-eac9-4100-91ba-c403a157d165)

Miriam:

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/e22fe726-145b-4498-b8f5-4d56c77ed0fb)

Suzy:

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/c69f576a-128c-439d-afef-ae8f66802c3a)

Tim:

Không có

Vậy, đáp án ở đây là Admin

-------

# Q32

Ở bài này thì ta dùng lại csv từ bài trước, em quên tên bài đó là câu bao nhiêu


Ứng dụng ở đây là 7zG.exe


![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/aa642f96-f214-4abd-a6e4-ceecb141d3d7)

Tiếp tục sử dụng prefetch thì ta được

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/d8589bdb-08a2-4f76-94c8-efa6ff1a3dd5)

Bởi vì giờ theo bài là UTC tức GMT +0, mà timezone ở Việt Nam là GMT+7, vậy nên ta chỉ cần lùi 7 tiếng là ra đáp án

Đáp án: 04/12/2020 02:32:09

-------

# Q33

tìm log file sequence

Ta tiếp tục dùng file csv mft đã parse

Tên file ở đây là: **fruit_Assortment.jpg**

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/1e930b9f-e8cd-435a-9978-dcc4924c6d9f)

Tìm ở bảng log file sequence

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/ac93f3c2-59d7-4395-ac03-31b517db4822)

Đáp án: 1276820064

---------

# Q34

Tìm sú của Jim

Ở bài này thì trước hết, ta phải đi tìm file doc của Jim ở đâu

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/a2ebc235-664c-444e-871d-10c5a80f346b)

Sử dụng binwalk để extract ra các file xml

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/0866f4b7-0b8f-4c4c-93bc-68316e04c209)

Khi xem qua, ta thấy magic bytes đầu của file word là PK

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/a41755f1-3a74-4617-8ab2-cc7c2dd9049d)

Mà PK ở đây là

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/aab15325-3b7f-4de7-b08d-90adad8ef17d)

Vậy là bài này có lẽ sẽ liên quan đến giải nén gì đó. Vậy là ta quay về lại bước binwalk.

Có một thứ khá mới ở đây là file.xml, em chưa từng gặp bất cứ 1 file docx nào mà bên trong nó có 1 file là file.xml

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/300ca7c7-f393-48bc-90f8-07cd324b8e9d)

Đây là 1 mẫu của file docx. Và, sau khi xem magic byte thì ta biết được rằng đây là file docx. 

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/0003565b-cd10-4832-90cb-7cefc926ffb1)

Thử đổi định dạng về docx. Bật thử lên và ta được đáp án

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/c6598881-f576-4429-a361-1f755f43f2b5)

Đáp án: Customer data is not stored securely 

--------

# Q35

Tìm slack

Nhờ vào những lần lục file, em biết được rằng file slack nằm ở user là hanser.apricot

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/20f3fd78-80c5-4994-adf2-6dbb2b3bb90c)

Thông thường thì, dữ liệu sẽ được lưu vào DB, ở đây là IndexedDB

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/039544e8-505e-4311-92e2-25cc757b6bc3)

Ở đây, ta thấy có file log, thử kiểm tra nó xem sao

Đọc qua một chút, ta thấy cụm từ gì đó xuất hiện

![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/ec77c09e-ae9d-4a1f-877e-cebebfbd5a21)

Vậy, nếu suy luận một chút, ta có thể thấy rằng nếu kneecaps ngừng hoạt động thì sẽ như nào

Vậy, đáp án ở đây là kneecaps


![image](https://github.com/anhshidou/EHCCTFTraining/assets/120787381/b9baf3fc-a749-40f9-84a5-528598668bfa)




















































