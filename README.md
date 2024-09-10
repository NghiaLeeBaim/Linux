
### SSL

## SSL là gì ?
- Secure Sockets Layer là một tiêu chuẩn công nghệ bảo mật, truyền thông của server và client

## Có bao nhiêu cách chứng thực SSL ?
- Có 3 giải pháp chứng thực SSL
    1. Domain Validation (DV SSL)
    2. Organization Validation (OV SSL)
    3. Extended Validation (EV SSL)

## CSR file dùng làm gì trong quá trình tạo SSL
- CSR là Certificate Signing Request, đây là một đoạn text có chứa thông tin, đã được mã hóa từ server để chuẩn bị cài đặt chứng chỉ SSL

## Sử dụng OpenSSL để gen file CSR sau đó request SSL cho domain tech.training.vietnix.tech
1. Tạo prvivate key
    "openssl genrsa -out tech.training.vietnix.tech.key 2048"
2. Tạo CSR (Certificate Signing Request)
    openssl req -new -key tech.training.vietnix.tech.key -out tech.training.vietnix.tech.csr
    - Nhập 1 số thông tin như sau:
            Country Name (2 letter code) [AU]: VN
            State or Province Name (full name) [Some-State]: Ho Chi Minh
            Locality Name (eg, city) []: Ho Chi Minh
            Organization Name (eg, company) [Internet Widgits  Pty Ltd]: Vietnix
            Organizational Unit Name (eg, section) []: Tech Training
            Common Name (e.g. server FQDN or YOUR name) []: tech.training.vietnix.tech
            Email Address []: admin@vietnix.tech
3. Kiểm tra CSR 
    "openssl req -noout -text -in tech.training.vietnix.tech.csr"
4. Gửi CSR cho nhà cung cấp SSL
5. Cài đặt SSL (Sau khi nhận được chứng chỉ)
    - Cài đặt SSL trên Nginx 
        "server {
            listen 443 ssl;
            server_name tech.training.vietnix.tech;

            ssl_certificate /etc/nginx/ssl/tech.training.vietnix.tech.crt;
            ssl_certificate_key /etc/nginx/ssl/tech.training.vietnix.tech.key;

            location / {
                proxy_pass http://localhost:80;
            }
        }"

## Pem file là gì ?
- Là một định file dùng để lưu trữ các thông tin về chứng chỉ, private key, và các loại mật mã khác. Pem file thường được sử dụng trong các hệ thống liên qua đén bảo mật như HTTPS, SSL/TLS, SSH
## Private key ssl là gì ?
- Là file mã hoá được sinh ra cùng lúc khi tạo các đoạn text

## PFX file là gì ? Cách chuyển từ file crt file sang PFX file.
- Là một định dạng tệp chứa thông tin về chứng chỉ số, bao gồm chứng chỉ và private key. PFX thường được dùng để truyền tải và lưu trữ an toàn các chứng chỉ số và khóa cá nhân trong một tệp duy nhất
    "openssl pkcs12 -export -out vietnix.vn.pfx -inkey vietnix.vn.key -in vietnix.vn.crt -certfile ca_bundle.crt "
    - -export: tạo file PFX.
    - -out vietnix.vn.pfx: File PFX đầu ra sẽ có tên vietnix.vn.pfx
    - -inkey vietnix.vn.key: Chỉ định file chứa khóa riêng của vietnix
    - -in vietnix.vn.crt: Chỉ định file chứa chứng chỉ của vietnix
    - -certfile ca_bundle.crt: File CA Bundle của bên cấp chứng chỉ (nếu có)

### Domain

## Domain là gì ?
- Domain là tên miền của một trang web được phân giải từ địa chỉ ip của máy chủ. Người dùng chỉ cần nhập domain trên trình duyệt sẽ truy cập tới một website bất kỳ 

## Các trạng thái của domain
# Đơn vị Cấp phát tên miền
- pendingTransfer: dùng để chỉ trạng thái đang chờ chuyển đổi nhà đăng ký.
- pendingDelete: dùng để chỉ trạng thái đang chờ xóa tên miền.
- inactive: thuật ngữ dùng để chỉ việc tên miền chưa được đăng ký thành công.
- serverTransferProhibited: đây là thuật ngữ chỉ trạng thái không cho phép transfer tên miền mức cơ quan quản lý tên miền.
- serverRenewProhibited: không thể được gia hạn, chỉ thấy ở một số quá trình tranh chấp pháp lý.
- serverHold: tên miền đang trong trạng thái tạm ngưng mức cơ quan quản lý tên miền.
- serverDeleteProhibited: cấm xóa mức cơ quan quản lý tên miền.
- serverUpdateProhibited: cấm cập nhật thông tin tên miền mức tại cơ quan quản lý tên miền.
- addPeriod: Đây là trạng thái sau khi tên miền mới đăng ký, và bị loại bỏ sau một vài ngày.
- autoRenewPeriod: tức là trạng thái tự động gia hạn. Nhà đăng ký hủy tên miền sau khi tên miền đã được renew nhưng bạn phải trả một chi phí cho nhà cung cấp.
- pendingRestore: trạng thái domain status chờ khôi phục từ một tên miền đã hết hạn đang được khôi phục về trạng thái ACTIVE
- pendingDelete: Tên miền domain status trong trạng thái chuẩn bị xóa vì hết hạn đăng ký
- transferPeriod: dùng để chỉ việc cho phép sau khi tranfer tên miền thành công, nhà đăng ký mới có thể yêu cầu nhà cung cấp xóa tên miền.
- redemptionPeriod: đây chính là trạng thái cho thấy rằng đăng ký của bạn đã yêu cầu những registry để xóa tên miền của bạn.
- clientTransferProhibited: dùng để chỉ trạng thái không cho phép chuyển đổi nhà đăng ký cũng như transfer tên miền
- clientRenewProhibited: dùng để chỉ trạng thái không cho phép gia hạn tên miền
- clientHold: dùng để chỉ trạng thái suspend, tên miền bị tạm ngừng
- clientDeleteProhibited: dùng để chỉ trạng không cho phép xóa tên miền ở bất cứ tình huống nào
- clientUpdateProhibited: dùng để chỉ trạng thái không cho phép cập nhật thông tin tên miền
- OK: Trạng thái thể hiện tên miền đang hoạt động bình thường sau khi đăng ký.

# Nhà đăng ký tên miền 
- Trạng thái clientHold: chỉ trạng thái ngưng tên miền
- Trạng thái clientRenewProhibited: chỉ trạng thái không thể gia hạn tên miền
- Trạng thái clientDeleteProhibited: chỉ trạng thái không cho phép xóa, hủy tên miền
- Trạng thái clientTransferProhibited: chỉ trạng thái không cho phép chuyển đổi nhà đăng ký tên miền
- Trạng thái clientUpdateProhibited: chỉ trạng thái không cho phép cập nhập thông tin tên miền

## Subdomain là gì?
- Subdomain là tên miền phụ của tên miền chính. Cho phép tổ chức, quản lý các phần khác nhau của trang web hoặc một ứng dụng web dưới cùng một tên miền chính

## Virtual Hosts là gì?
- Virtual Hosts là một phương thức lưu trữ tên miền hoặc nhiều tên miền khác nhau trên cùng một máy chủ. Là một giải pháp cho phép nhúng rất nhiều tên miền trên một địa chỉ ip, một server duy nhất

### Mail Server

## Tìm hiểu MX Record
MX record là một bản ghi trong DNS. Nó thực hiện việc định vị máy chủ mail cho tên miền. Với mỗi tên miền, người dùng có thể gán nhiều MX Record dù email tạm thời bị gián đoạn hoặc không hoạt động trong một thời gian, nhưng dữ liệu vẫn không hề bị mất.

## Tìm hiểu DKIM, SPF, PTR
- DKIM (DomainKeys Identified Mail): là một phương thức để xác thực tính hợp lệ của một email. Mỗi mail khi gửi đi được gắn 1 khóa private key và sau đó được xác thực bên phía mail server nhận bằng một khóa public key được setup trong bản ghi DNS. Quá trình này đảm bảo rằng mail gửi đi không bị thay đổi trên đường tới phía bên nhận. Nói cách khác, nó ngăn chặn ai đó có thể can thiệp vào email của bạn, thay đổi nó và sau đó gửi đi với nội dung mới. Bản chất của nó giống hệt như giao thức SSH mà ta vẫn hay dùng hàng ngày. 
- SPF (Sender Policy Framework ): hoạt động với nguyên tắc xác thực một email server có được gửi email dưới tên một domain nào đó. Trong trường hợp nhận diện được email mới đến từ một địa chỉ IP không phù hợp, email sẽ được chuyển đếên miền trên internet. Nó chuyển đổi từ các dãi ip khó nhớ thành các tên miền dễ nhớ như: vietnix.vn

## Các loại recored DNS
1. A Record (Address Record): Chuyển đổi tên miền thành địa chỉ IPv4
2. AAAA Record (IPv6 Address Record): Chuyển đổi têtên miền khác như là bí danh cho tên miền chính
4. MX Record (Mail Exchange Record): Xác định máy chủ email cho tên miền
5. NS Record (Name Server Record): Xác định các máy chủ DNS chịu trách nhiệm cho tên miền
6. PTR Record (Pointer Record): Thực hiện phân giải ngược từ địa chỉ IP về tên miền
7. SOA Record (Start of Authority Record): Cung cấp thông tin cơ bản về khu vực DNS, bao gồm máy chủ DNS chính, thông tin liên hệ của quản trị viên, và các thông số như thời gian làm mới, thời gian hết hạn và thời gian lưu trữ
8. SRV Record (Service Record): Chỉ định các dịch vụ mạng khác nhau, chẳng hạn như dịch vụ VoIP hoặc dịch vụ chat, và cung cấp thông tin về máy chủ cung cấp dịch vụ đó
9. TXT Record (Text Record): Cung cấp các thông tin văn bản cho tên miền. Thường được sử dụng để lưu trữ dữ liệu xác thực, như SPF (Sender Policy Framework) cho email hoặc thông tin xác thực khác
10. CAA Record (Certification Authority Authorization Record): Xác định các cơ quan chứng thực (CA) có quyền cấp chứng chỉ SSL/TLS chn hộp thư Spam.
- PTR (Point Record ): Là một bản ghi ngược, có nhiệm vụ chuyển một địa chỉ ip đến tên miền  

### DNS

## DNS là gì ?
-  DNS (Domain name system): là hệ thống phân giải tên miền trên internet. Nó chuyển đổi từ các dãi ip khó nhớ thành các tên miền dễ nhớ như: vietnix.vn

## Các loại recored DNS
1. A Record (Address Record): Chuyển đổi tên miền thành địa chỉ IPv4
2. AAAA Record (IPv6 Address Record): Chuyển đổi tên miền thành địa chỉ IPv6
3. CNAME Record (Canonical Name Record): Chỉ định tên miền khác như là bí danh cho tên miền chính
4. MX Record (Mail Exchange Record): Xác định máy chủ email cho tên miền
5. NS Record (Name Server Record): Xác định các máy chủ DNS chịu trách nhiệm cho tên miền
6. PTR Record (Pointer Record): Thực hiện phân giải ngược từ địa chỉ IP về tên miền
7. SOA Record (Start of Authority Record): Cung cấp thông tin cơ bản về khu vực DNS, bao gồm máy chủ DNS chính, thông tin liên hệ của quản trị viên, và các thông số như thời gian làm mới, thời gian hết hạn và thời gian lưu trữ
8. SRV Record (Service Record): Chỉ định các dịch vụ mạng khác nhau, chẳng hạn như dịch vụ VoIP hoặc dịch vụ chat, và cung cấp thông tin về máy chủ cung cấp dịch vụ đó
9. TXT Record (Text Record): Cung cấp các thông tin văn bản cho tên miền. Thường được sử dụng để lưu trữ dữ liệu xác thực, như SPF (Sender Policy Framework) cho email hoặc thông tin xác thực khác
10. CAA Record (Certification Authority Authorization Record): Xác định các cơ quan chứng thực (CA) có quyền cấp chứng chỉ SSL/TLS cho tên miền. Giúp bảo mật việc cấp chứng chỉ
11. NAPTR Record (Naming Authority Pointer Record): Cung cấp thông tin về các dịch vụ hoặc kiểu dữ liệu bổ sung có thể được sử dụng trong hệ thống DNS. Thường được sử dụng trong các ứng dụng đặc biệt như ENUM (E.164 Number Mapping)
12. DS Record (Delegation Signer Record): Liên kết giữa một tên miền và bản ghi DNSSEC của nó, giúp xác minh tính toàn vẹn của các bản ghi DNS

## Nguyên tắc làm việc của DNS
1. Phân tích tên miền
    - Yêu Cầu DNS: Khi bạn nhập một tên n miền thành địa chỉ IPv6
3. CNAME Record (Canonical Name Record): Chỉ định tên miền khác như là bí danh cho tên miền chính
4. MX Record (Mail Exchange Record): Xác định máy chủ email cho tên miền
5. NS Record (Name Server Record): Xác định các máy chủ DNS chịu trách nhiệm cho tên miền
6. PTR Record (Pointer Record): Thực hiện phân giải ngược từ địa chỉ IP về tên miền
7. SOA Record (Start of Authority Record): Cung cấp thông tin cơ bản về khu vực DNS, bao gồm máy chủ DNS chính, thông tin liên hệ của quản trị viên, và các thông số như thời gian làm mới, thời gian hết hạn và thời gian lưu trữ
8. SRV Record (Service Record): Chỉ định các dịch vụ mạng khác nhau, chẳng hạn như dịch vụ VoIP hoặc dịch vụ chat, và cung cấp thông tin về máy chủ cung cấp dịch vụ đó
9. TXT Record (Text Record): Cung cấp các thông tin văn bản cho tên miền. Thường được sử dụng để lưu trữ dữ liệu xác thực, như SPF (Sender Policy Framework) cho email hoặc thông tin xác thực khác
10. CAA Record (Certification Authority Authorization Record): Xác định các cơ quan chứng thực (CA) có quyền cấp chứng chỉ SSL/TLS chn hộp thư Spam.
- PTR (Point Record ): Là một bản ghi ngược, có nhiệm vụ chuyển một địa chỉ ip đến tên miền  

### DNS

## DNS là gì ?
-  DNS (Domain name system): là hệ thống phân giải tên miền trên internet. Nó chuyển đổi từ các dãi ip khó nhớ thành các tên miền dễ nhớ như: vietnix.vn

## Các loại recored DNS
1. A Record (Address Record): Chuyển đổi tên miền thành địa chỉ IPv4
2. AAAA Record (IPv6 Address Record): Chuyển đổi tên miền thành địa chỉ IPv6
3. CNAME Record (Canonical Name Record): Chỉ định tên miền khác như là bí danh cho tên miền chính
4. MX Record (Mail Exchange Record): Xác định máy chủ email cho tên miền
5. NS Record (Name Server Record): Xác định các máy chủ DNS chịu trách nhiệm cho tên miền
6. PTR Record (Pointer Record): Thực hiện phân giải ngược từ địa chỉ IP về tên miền
7. SOA Record (Start of Authority Record): Cung cấp thông tin cơ bản về khu vực DNS, bao gồm máy chủ DNS chính, thông tin liên hệ của quản trị viên, và các thông số như thời gian làm mới, thời gian hết hạn và thời gian lưu trữ
8. SRV Record (Service Record): Chỉ định các dịch vụ mạng khác nhau, chẳng hạn như dịch vụ VoIP hoặc dịch vụ chat, và cung cấp thông tin về máy chủ cung cấp dịch vụ đó
9. TXT Record (Text Record): Cung cấp các thông tin văn bản cho tên miền. Thường được sử dụng để lưu trữ dữ liệu xác thực, như SPF (Sender Policy Framework) cho email hoặc thông tin xác thực khác
10. CAA Record (Certification Authority Authorization Record): Xác định các cơ quan chứng thực (CA) có quyền cấp chứng chỉ SSL/TLS cho tên miền. Giúp bảo mật việc cấp chứng chỉ
11. NAPTR Record (Naming Authority Pointer Record): Cung cấp thông tin về các dịch vụ hoặc kiểu dữ liệu bổ sung có thể được sử dụng trong hệ thống DNS. Thường được sử dụng trong các ứng dụng đặc biệt như ENUM (E.164 Number Mapping)
12. DS Record (Delegation Signer Record): Liên kết giữa một tên miền và bản ghi DNSSEC của nó, giúp xác minh tính toàn vẹn của các bản ghi DNS

## Nguyên tắc làm việc của DNS
1. Phân tích tên miền
    - Yêu Cầu DNS: Khi bạn nhập một tên miền vào trình duyệt, trình duyệt sẽ gửi yêu cầu đến máy chủ DNS để phân giải tên miền thành địa chỉ IP
    - DNS Resolver: Yêu cầu được chuyển đến một DNS resolver (có thể là một máy chủ DNS của ISP hoặc máy chủ DNS công cộng)
2. Cớ chế phân giải
    - Recursive Query (Yêu Cầu Đệ Quy): Nếu DNS resolver không có bản ghi cho tên miền đó trong bộ nhớ cache của nó, nó sẽ thực hiện yêu cầu đệ quy để tìm kiếm thông tin. Resolver sẽ gửi yêu cầu đến các máy chủ DNS cấp cao hơn để tìm kiếm thông tin
    - Iterative Query (Yêu Cầu Từng Bước): Trong một số trường hợp, resolver có thể gửi yêu cầu từng bước đến các máy chủ DNS khác nếu nó không có thông tin đầy đủ
3. Dựng cây DNS
    - Root Servers (Máy Chủ Gốc): Nếu resolver không tìm thấy thông tin trong bộ nhớ cache của nó, nó sẽ gửi yêu cầu đến một trong các máy chủ DNS gốc (root servers). Các máy chủ này không lưu trữ bản ghi cụ thể nhưng cung cấp thông tin về các máy chủ DNS cấp cao hơn
    - TLD Servers (Máy Chủ TLD): Các máy chủ gốc sẽ chỉ hướng resolver đến các máy chủ DNS của TLD (Top-Level Domain), như .com, .org, .net. Những máy chủ này quản lý tên miền cấp cao hơn
    - Authoritative Name Servers (Máy Chủ Tên Chính Thức): Cuối cùng, resolver sẽ đến máy chủ DNS chính thức của tên miền, nơi lưu trữ các bản ghi DNS cụ thể cho tên miền đó
4. Trả về kết quả
    - Lấy Địa Chỉ IP: Máy chủ DNS chính thức trả về địa chỉ IP cho tên miền. Thông tin này sẽ được lưu trữ trong bộ nhớ cache của resolver để sử dụng cho các yêu cầu trong tương lai
    - Kết Nối: Trình duyệt của bạn sử dụng địa chỉ IP nhận được để kết nối với máy chủ web và tải trang
5. Cập nhập đồng bộ hóa
    - SOA Record (Start of Authority Record): Bản ghi SOA trên máy chủ DNS chính thức cung cấp thông tin về cách và khi nào các bản ghi DNS của tên miền sẽ được cập nhật hoặc đồng bộ hóa
    - Cache Expiry: Các bản ghi DNS được lưu trữ trong bộ nhớ cache của resolver và máy chủ DNS có thời gian sống (TTL - Time to Live). Khi TTL hết hạn, bản ghi sẽ bị xóa và phải được làm mới
6. Các loại bản ghi DNS
7. Bảo mật DNS
    - DNSSEC (DNS Security Extensions): Cung cấp cơ chế để xác minh tính toàn vẹn của dữ liệu DNS, đảm bảo rằng thông tin không bị thay đổi hoặc giả mạo trong quá trình phân giải

## Cách phân giải địa chỉ DNS
1. Trình duyệt/ứng dụng gửi yêu cầu phân giải DNS.
2. DNS resolver kiểm tra bộ nhớ cache.
3. Nếu không có kết quả, yêu cầu được gửi đến máy chủ DNS gốc.
4. Máy chủ DNS gốc hướng đến máy chủ DNS TLD.
5. Máy chủ DNS TLD hướng đến máy chủ DNS chính thức.
6. Máy chủ DNS chính thức trả về bản ghi DNS chứa địa chỉ IP.
7. DNS resolver lưu bản ghi vào bộ nhớ cache và trả về kết quả cho trình duyệt/ứng dụng.
8. Trình duyệt/ứng dụng sử dụng địa chỉ IP để kết nối với máy chủ web.

## ping/hping3 ping đến domain vietnix.vn sau đó giải thích
- Lệnh ping là công cụ để kiểm tra kết nối giữa máy tính và server qua giao thức ICMP. Lệnh này gửi các gói ICMP Echo Request đến địa chỉ IP của tên miền và chờ phản hồi.
        ex: ping vietnix.vn
- lệnh hping3 là công cụ mạnh mẽ và đa nhiệm hơn ping được sử dụng để kiểm tra mạng, bảo mật và phân tích lưu lượng mạng
        ex: sudo hping3 -S vietnix.vn
            - sudo: mượn quyền root
            - -S: chỉ định cờ Syn(1 trong quy trình bắt tay 3 bước của TCP/IP SYN, SYN/ACK, ACK)  
        ex: sudo hping3 --udp vietnix.vn -p 80
            - --udp: gửỉ gói UDP
            - -p 80: cổng 80 đích
## ttl = là gì
- ttl (Time To Live): là thông số được sử dụng để xác định thời gian hoặc số bước mà một gói tin hoặc dữ liệu có thể tồn tại trước khi bị hủy hoặc loại bỏ. TTL giúp giúp ngăn chặn các gói tin chạy vòng lặp tránh bị lãnu thời gian mà dữ liệu, gói tin, bản ghi có thể  tồn tại trước khi bị loại bỏ hoặc hết hiệu lực
### ssh command

## Dùng password
- "ssh admin@103.90.224.90"

## Dùng key
- "ssh -i .ssh/vietnix_key_rsa admin@103.90.224.90"
## Dùng port custom
- "ssh admin@103.90.224.90 -p 22"

### scp command
- "scp [tùy chọn] [nguồn] [đích]"

## scp 1 file
- "scp /path/blog root@103.90.224.90:/path/admin "

## scp 1 folder
- "scp -v /path/blog root@103.90.224.90:/path/admin "
    - -v: chế độ verbose (hiển thị thông tin chi tiết về quá trình sao chép)

### rsync command

rsync file

rsync folderg phí tài nguyên mạng 

## time= là gì
- time là đề cập khoảng thời gian mà dữ liệu thời gian mà dữ liệu, gói tin, bản ghi có thể  tồn tại trước khi bị loại bỏ hoặc hết hiệu lực
### ssh command

## Dùng password
- "ssh admin@103.90.224.90"

## Dùng key
- "ssh -i .ssh/vietnix_key_rsa admin@103.90.224.90"
## Dùng port custom
- "ssh admin@103.90.224.90 -p 22"

### scp command
- "scp [tùy chọn] [nguồn] [đích]"

## scp 1 file
- "scp /path/blog root@103.90.224.90:/path/admin "

## scp 1 folder
- "scp -v /path/blog root@103.90.224.90:/path/admin "
    - -v: chế độ verbose (hiển thị thông tin chi tiết về quá trình sao chép)

### rsync command
- rsync [tùy chọn] [nguồn] [đích]

- -a: Chế độ lưu trữ (giữ nguyên liên kết biểu tượng, quyền, dấu thời gian, v.v.).
- -v: Chế độ chi tiết (hiển thị thông tin chi tiết về quá trình sao chép).
- -z: Nén dữ liệu trong quá trình truyền.
- -P: Hiển thị tiến trình trong khi truyền và cho phép tiếp tục truyền khi bị ngắt.
- --delete: Xóa các file ở đích nếu chúng không còn tồn tại trong nguồn.

## rsync file
// sao chép file cục bộ
- rsync -avP /home/test.txt /Download/test1.txt

## rsync folder
// Sao chép từ máy chủ từ xa
- rsync -avzP root@103.90.224.90:/Download/test1 /Download/test

## rsync increamental
// Sao chép folder và nén folder để tiết kiệm băng thông
rsync -avz /home/test home/test2

### cat command

## cat nội dung 1 file
- cat vietnix.txt  

## cat dòng thứ <n> trong file
- "cat vietnix.txt | head -n 5 | tail -n 1"
        cat filename: In toàn bộ nội dung của tệp.
        head -n <n>: Lấy n dòng đầu tiên của nội dung từ cat.
        tail -n 1: Lấy dòng cuối cùng trong số các dòng đã được in ra bởi head.

cat nhiều dòng vào 1 file bằng EOF
- "cat <<EOF > /home/nghiale/test1.txt"

### echo command

## Dùng echo để chèn thêm 1 dòng vào cuối file
- " echo "Dòng cuối của file" >> file.txt "

## Dùng echo để overwirte nội dung của file
- " echo "Nội dung ghi đè" > file1.txt" 

### tail/head command

## tail và tailf
# tail: là lệnh dùng để hiển thị một số dòng cuối cùng của một file. Mặc định, tail hiển thị 10 dòng cuối cùng của file
- "tail -n 20 technical.txt"
    - -n 20: in ra 20 dòng cuối của file 
# tailf: là lệnh dùng để theo dõi một file đang được cập nhập liên tục theo thời gian thực. Thường được dùng để theo dõi file log
- "tailf /var/log/auth.log | grep sshd"
    - grep sshd : tìm các kết nối theo giao thức ssh

### sed command

## Dùng sed để find and replace một string trong file
- "sed -i 's./vietnix/vietnix.vn/g' /etc/hosts

## traceroute/tracert command

## Sau khi traceroute xong giải thích chi tiết kết quả trả về
- "traceroute vietnix.vn"


- Thể hiện số thứ tự các router đến mạng vị trí đích qua bao nhiêu router
- Đây là địa chỉ IP của router 
- Thời gian phản hồi
    - Thời gian thử lần 1
    - Thời gian thử làn 2
    - Thời gian thử lân 3
- "* * *": là chính sách mạng hoặc tường lửa hoặc router không có phản hồi qua giao thức ICMP 

### netstat command

## hiển thị các socket đang listen
- "netstat -l"

## don't resolve hostname
- "netstat -ln"

## don't resolve portname
- "netstat -ln"

## display process name/PID
- "netstat -lnp"
    - -l: Chỉ hiển thị các socket đang lắng nghe.
    - -n: Hiển thị địa chỉ IP và cổng dưới dạng số (không phân giải tên miền).
    - -p: Hiển thị PID và tên tiến trình đang sử dụng socket.

## only show tcp socket
- "netstat -tl"
    - -t: tcp

## only show udp socket
- "netstat -ul"
    - -u: udp

### sort command

## sort theo thứ tự tăng dần
- "sort"

## sort theo thứ tự giảm dần
- "sort -r"

## sort theo column
- "sort -k4"
    - -k4: sắp xếp theo cột 4 

### uniq command

## lọc ra các dòng lặp lại trong một file
- "uniq -d vietnix.txt"

## lọc ra các dòng lặp lại trong file và đếm số lượng các dòng lặp lại
- "sort vietnix.txt | uniq -c | awk '$1 > 1'
    - sort vietnix.txt: sắp xếp các dòng trong file
    - uniq -c: Đếm số lần xuất hiện của mỗi dòng 
    - awk '$1 > 1': Lọc ra các dòng lớn hơn 1

### wc command

## Đếm số dòng trong file
- "wc -l tes1.txt"

## Đếm số kí tự trong file
- "wc -m tes1.txt"

## chmod, chown, chattr command

## Phân quyền bằng số, phân quyền bằng chữ
- Quyền bằng số
rwx 	Có full quyền 	                7
rw- 	Chỉ có quyền đọc và ghi 	    6
r-x 	Chỉ có quyền đọc và thực thi 	5
r– 	    Chỉ có quyền đọc 	            4
— 	    Không có quyền gì 	            0

- Ký tự phân quyền
u : quyền của người sở hữu ( owner )
g : quyền sở hữu của nhóm ( group )
o : quyền của mọi user khác ( others )
+ : thêm quyền
– : rút bớt quyền
= : gán quyền

Ví dụ
- chmod u=+rwx vietnix.txt: user có quyền đọc, ghi, viết
- chmod g=+rx vietnix.txt: group có quyền đọc , thực thi
- chmod o=+r vietnix.txt: owner có quyền đọc
- chmod 777 /vienix.txt : cả user, group, owner có đủ 

## Đổi owner user/group
- chown user:group /vietnix

## Set Immutable Attribute
- "sudo chattr +i vietnix.txt"

### find command
- -type d: thể loại thư mục
- -type f: thể loại file
- -name "abc": tìm tên file chứa "abc"

## find các file có đuôi .log
- find -name "*.log"

## find các folder có tên abc
- "sudo find / -type d -name "abc""

## find các file có tên abc
- "sudo find / -type f -name "abc""

## find các file có tên abc và thực hiện phần quyền read only cho file
- "sudo find / -t type f -name "abc" -exec chmod 444 {} \;"
    - -exec chmod 444 {} \;: thực hiện lệnh chmod 444 trên mỗi file thực hiện được cung cấp quyền chỉ đọc cho user, group và owner

### cp command

## cp file
- cp /home/folder1.txt /home/folder2

## cp folder
- cp /home/folder1 /home/folder2

### mv command

## mv file, folder
- mv [nguồn] [đích]
# file
- "sudo mv /home/nghiale/Desktop/test.txt /Download"

# folder
- "sudo mv /home/nghiale/Desktop/vietnix /Download"


### cut command

## cut kí tự thứ <n> trong string
- "echo "vietnix.vn" | cut -c -n" //cut từ kí tự thứ <n> trở về sau

hoặc

- "echo "Hello World" | cut -c n-" //cut từ kí tự thứ <n> trở về sau

## cut từ kí tự thứ <n> trở về sau
- "echo "Hello World" | cut -c n-"

## cut từ kí tự thứ <n> trở về trước
- "echo "Hello World" | cut -c -n"

### dig command

## Dùng Dig command để kiểm tra resolv record A, MX, NS
- record A:dig vietnix.vn A
- record MX: dig vietnix.vn MX
- record NS: dig vietnix.vn NS

## Dùng Dig command để kiểm tra resolv record A, MX, NS với custom DNS
- record A:dig @8.8.8.8 vietnix.vn A
- record MX: dig @1.1.1.1 vietnix.vn MX
- record NS: dig @9.9.9.9 vietnix.vn NS

### tar/zip/unzip command

## Nén/Giải nén file tar.gz - Nén/Giải nén file .zip
# Nén/Giải nén file tar.gz
    - Nén file: tar -czvf file.txt.tar.gz file.txt
    - Giải nén: tar -xzvf file.txt.tar.gz

# Nén/Giải nén file .zip
    - Nén file: zip -r file1.zip file1.txt
    - Giải nén: unzip file1.zip


### mount/umount command

## Add thêm một ổ cứng sdb ~ 5gb
- "sudo mount /dev/sdb1 /mnt/newdisk"

## iểm tra được có bao nhiêu ổ cứng trên máy chủ
- "lsblk" 

## Mount ổ cứng vào /mnt/test
- "mount /dev/sdb1 /mnt/test"

## Umount /mnt/test
- "unmount /dev/sdb1 /mnt/test"

### Symbolic Links, Hard Links command

## Định nghĩ Sym Link
- Sym link (liên kết mềm): tương tự như shorcut trên windows, tạo ra một liên kết dẫn tới thư mục đó. Nếu tệp gốc bị xóa thì liên kết đó trở thành không hợp lệ

## Định nghĩ Hard Link
- Hard Link (liên kết cứng): liên kết cứng là liên kết trực tiếp tới một tập tin trên ổ đĩa (không thể liên kết thư mục). Nếu tệp gốc bị xóa vẫn có thể truy cập qua hard link

## Ví dụ về Sym Link và Hard Link
- Sym link: "ln -s /home/user/file.txt /home/user/lien_ket_mem_file.txt"
    - -s: Chỉ định rằng liên kết sẽ là symbolic link

- Hard link: "ln /home/user/file.txt /home/user/lien_ket_cung_file.txt"

### ls command

## Liệt kê danh sách file/thư mục
- ls 

## Liệt kê danh sách file/thư mục và thuộc tính
- ls -l

## Show file ẩn
- "ls -a"

### ps command

show tiến trình
- "ps aux"

kill tiến trình
- "kill [PID]"

### top command

## Kiểm tra tài nguyên cpu đang sử dụng của một vài process đang chạy
- top hoặc htop

## Giải thích về Load average, us, sy, ni, id, wa, hi, si, st, zombie process, sleeping process
- Load Average: là một chỉ số quan trọng trong hệ thống Linux, cho biết mức độ tải của hệ thống trong một khoảng thời gian cụ thể.
- us: (User CPU Time) Phần trăm thời gian CPU được sử dụng bởi các quá trình trong không gian người dùng, tức là các ứng dụng và lệnh do người dùng chạy
- sy: (System CPU Time) Phần trăm thời gian CPU được sử dụng bởi các quá trình trong không gian nhân, tức là các hoạt động của hệ điều hành như quản lý phần cứng và các dịch vụ hệ thống
- ni: (Nice CPU Time) Phần trăm thời gian CPU được sử dụng bởi các quá trình với mức độ ưu tiên "nice"
- id: (Idle CPU Time) Phần trăm thời gian CPU không được sử dụng, tức là CPU không làm việc và không bị chờ
- wa: (I/O Wait) Phần trăm thời gian CPU đang chờ đợi các hoạt động I/O hoàn tất, ví dụ như đọc/ghi đĩa
- hi: (Hardware Interrupts) Phần trăm thời gian CPU dành cho việc xử lý các ngắt phần cứng
- si: (Software Interrupts) Phần trăm thời gian CPU dành cho việc xử lý các ngắt phần mềm
- st: (Steal Time) Phần trăm thời gian CPU bị "ăn cắp" bởi các máy ảo khác khi đang chạy trên một máy chủ ảo
- Zombie Process: Là các quá trình đã hoàn thành công việc của chúng nhưng vẫn còn tồn tại trong bảng điều khiển của hệ thống. Thường xuất hiện khi quá trình cha chưa thực hiện việc đọc thông tin trạng thái kết thúc của quá trình con
- Sleeping Process:Là các quá trình đang trong trạng thái chờ đợi một sự kiện nào đó và không tiêu tốn CPU. Các quá trình này có thể được đánh dấu là ngủ hoặc ngủ chờ tùy thuộc vào lý do tại sao chúng không hoạt động
    - Có hai loại trạng thái ngủ:
        * Interruptible Sleep: Quá trình có thể bị đánh thức bởi các tín hiệu.
        * Uninterruptible Sleep: Quá trình không thể bị đánh thức cho đến khi điều kiện mà nó đang chờ được thỏa mãn 

### free command

## Giải thích ram used, free, shared, buff/cache, free
- used: Bộ nhớ đã được sử dụng
- free: Bộ nhớ còn trống và chưa được sử dụng
- shared: Bộ nhớ được chia sẻ giữa các process
- buff/cache: Bộ nhớ được sử dụng bởi các buffer và cache
- available: Bộ nhớ có thể sử dụng cho các ứng dụng mà không cần phải swap

### df command

## Xem dung lượng disk
df [options]
-h: Hiển thị thông tin với đơn vị dễ đọc (KB, MB, GB).
-a: Hiển thị tất cả các hệ thống tập tin, bao gồm cả các hệ thống tập tin có dung lượng 0.
-T: Hiển thị loại hệ thống tập tin (file system type).
-i: Hiển thị thông tin về các inode thay vì dung lượng. Inode là một cấu trúc dữ liệu lưu trữ thông tin về file hoặc thư mục.

## Phân vùng / là gì
- Phân vùng ổ đĩa giúp quản lý không gian lưu trữ. Người dùng có thể  tạo, xóa, thay đổi kích thước bằng các công cụ dòng lệnh
# Các công cụ
- "sudo fdisk -l"
- "lsblk"
- "sudo blkid
