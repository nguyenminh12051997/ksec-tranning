# Kết nối mạng trong VMware
Các thành phần hình thành nên mạng ảo trong VMware gồm switch ảo, card mạng ảo, DHCP server ảo và thiết bị NAT.

Switch ảo (Virtual Switch) cũng giống như switch vật lý, một Virtual Switch kết nối các thành phần mạng ảo lại với nhau. Những  switch ảo hay còn gọi là mạng ảo, chúng có tên là VMnet0, VMnet1, VMnet2… một số switch ảo được gắn vào mạng một cách mặc định. Mặc định khi ta cài Wmware thì có sẵn 3 Switch ảo như sau: VMnet0 chế độ Bridged (cầu nối), VMnet8 chế độ NAT và VMnet1 chế độ Host-only. (Ta có thể thêm, bớt, chỉnh các option của VMnet bằng cách vào menu Edit -> Virtual Network Editor…)

<img src="http://i.imgur.com/KyTqURy.png">

Để thêm hoặc bớt VMnet ta có thể chọn Add Networ và Remove NetworK

## Các cơ chế hoạt động và các mô hình cơ bản  khi cấu hình với switch ảo (VMnet):

**Chế độ Bridge**, ở chế độ này thì card mạng trên máy ảo sẹ được gắn vào VMnet0 và VMnet0 này liên kết trực tiếp với card mạng vật lý. Ở chế độ này máy ảo sẽ kết nối internet thông qua lớp card mạng vật lý và có chung lớp mạng với card mạng vật lý.

<img src="https://public.sn2.livefilestore.com/y2pt0QnN_xQOE_pgIJRAxtK5EM-sTUw3tPhgNsDZSts-Qh6fZ0W_qxt6WIiUui_jP8QpGgV0aatQfIFo39L_MjwKRk2v0XHixtbEMALXPlrISQ/briged.png?psid=1">

**Chế độ NAT**, ở chế độ này thì card mạng của máy ảo kết nối với VMnet8, VNnet8 cho phép máy ảo đi internet thông qua cơ chế NAT (NAT device). Lúc này lớp mạng bên trong máy ảo khác hoàn toàn với lớp mạng của card vật lý bên ngoài. IP của card mạng sẽ được cấp bởi DHCP VMnet8 cấp, trong trường hợp bạn muốn thiết lập IP tĩnh cho card mạng máy ảo bạn phải đảm bảo chung lớp mạng với VNnet8 thì máy ảo mới có thể đi internet.

**Cơ chế Host-only**, ở cơ chế này máy ảo được kết nối với VMnet có tính có tính năng Host-only, trong trường hợp hình này là VMnet1 (bạn có thể add nhiều VMnet Host-only). VNnet Host-only kết nối ra một card mạng ảo tương ứng ngoài máy thật (như đã nói ở phần trên, khi ta add một VMnet thì có một card tương ứng với VMnet sẽ được tạo ra ở phần trên. Trường hợp này là VMware Network Adapter VMnet1).

<img src="https://public.sn2.livefilestore.com/y2pvoXPJXGiHRiv4zknu2S7UJjcxNmiQXMNkFNlkZcSIgqKXV6631h6n3sBnQ6BTue9rr4PvO2Leemq816L5Hk_oUjUhvYzxwsoTCJ9S-3FByk/NAT.png?psid=1">

Ở chế độ này máy ảo không có kết nối internet. IP của máy ảo được cấp bởi DHCP của VMnet tương ứng.Trong nhiều trường hợp đặc biệt cần cấu hình riêng, ta có thể tắt DHCP trên VMnet và cấu hình IP bằng tay cho máy ảo

## Cách thêm 1 card mạng và cấu hình nó
Ở Network adapter  chon ADD
<img src="http://i.imgur.com/vpi3hM4.png">

sau đó khởi động máy cấu hình them một card mạng ở file hệ thống 

```vi /etc/network/interfaces```

và gán địa chỉ ip cho card eth1 như hình:
<img src="http://i.imgur.com/f6PWwI7.png">
sau đó bật chúng lên bằng câu lệnh 

```ifdown -a &&  ifup -a```

tiếp đó kiểm tra 

<img src="http://i.imgur.com/VDVnpQo.png"> 

Như vậy là đã OK


