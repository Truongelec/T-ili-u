**TTL( time to live)**
1. **Nguyên lý**:
- Mỗi gói tin (packet) được gửi đi đều mang theo một giá trị TTL. Giá trị này được đặt bởi máy tính gửi đi và thường được tính bằng giây.
-  Khi một gói tin đi qua một router (bộ định tuyến), giá trị TTL sẽ giảm đi một đơn vị.
 -   Nếu giá trị TTL giảm xuống còn 0, gói tin đó sẽ bị loại bỏ.

## ssh command
### 1. **passrord**:
- Kết nối đến máy chủ SSH:
- Thay đổi mật khẩu:

        passwd
- Khởi động lại dịch vụ SSH:
    
        sudo systemctl restart ssh
### 2. **key**:
- Sử dụng lệnh tạo khóa
 
        ssh-keygen
 - Nhập tên file: Bạn có thể để trống để sử dụng mặc định **(~/.ssh/id_rsa)** hoặc nhập tên file mong muốn.
 - Nhập passphrase: Đây là mật khẩu để bảo vệ **private key**. Bạn có thể để trống nếu không muốn đặt mật khẩu.
- Sao chép public key:
      Vị trí public key: Khóa public thường được lưu trong file có đuôi .pub (ví dụ: id_rsa.pub).
- Sao chép nội dung: Mở file này bằng một trình soạn thảo văn bản và sao chép toàn bộ nội dung.

- Thêm public key vào máy chủ
    + Cách 1: dùng lệnh
    
             ssh-copy-id user@your_server_ip
    + cách 2
        
            cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
3. **port custom**:
- Mở file cấu hình

        sudo nano /etc/ssh/sshd_config
- Tìm dòng Port 22 và thay thế số 22 bằng số port mới bạn muốn sử dụng (ví dụ: 3333)
    
        Port 3333
- Lưu lại và thoát khỏi file
- Khởi động lại dịch vụ SSH
## scp command 
- **SCP (Secure Copy)** là một tiện ích dòng lệnh mạnh mẽ được sử dụng để sao chép các tệp và thư mục giữa các máy tính một cách an toàn. Nó dựa trên giao thức SSH, đảm bảo rằng dữ liệu được truyền đi một cách bảo mật.
- Cú pháp cơ bản của lệnh SCP:
    
        scp [options] source destination
1. **file**:
- Sao chép một tệp từ máy cục bộ đến máy chủ:

        scp file.txt user@server:/home/user/backup
- Sao chép một thư mục từ máy chủ đến máy cục bộ:
    
         scp -r user@server:/home/user/documents .
- Sao chép một tệp giữa hai máy chủ:
        
        scp -i ~/.ssh/my_key user@server:/home/user/file.txt .


2. **folder**:
- Sao chép thư mục "documents" từ máy cục bộ lên thư mục "backup" trên máy chủ:
        
        scp -r documents user@server:/home/user/backup

## rsync command
- rsync là một công cụ dòng lệnh cực kỳ hữu ích trên các hệ thống Linux/Unix, được sử dụng chủ yếu để sao chép và đồng bộ hóa các tệp và thư mục từ xa cũng như cục bộ. So với SCP, rsync thông minh hơn ở chỗ nó chỉ truyền những phần dữ liệu đã thay đổi, giúp tiết kiệm băng thông và thời gian.
- Cú pháp cơ bản:

        rsync [options] source destination

1. **folder**
- VD 1: Sao chép toàn bộ thư mục documents từ máy cục bộ lên máy chủ:

        rsync -avz documents user@server:/home/user/backup
- VD 2: Đồng bộ hóa thư mục project giữa hai máy chủ, chỉ cập nhật những phần thay đổi:
    
        rsync -avz --delete user1@server1:/home/user1/project user2@server2:/home/user2/projects
- VD 3: Sao chép các tệp có phần mở rộng .log từ máy chủ về máy cục bộ:

        rsync -avz user@server:/var/log/*.log .

2. **increamental**
- Tăng dần trong rsync có nghĩa là chương trình chỉ truyền những phần dữ liệu đã thay đổi từ nguồn đến đích, thay vì truyền toàn bộ dữ liệu mỗi lần. Điều này giúp tiết kiệm băng thông, thời gian và tài nguyên hệ thống, đặc biệt khi thực hiện các bản sao lưu hoặc đồng bộ hóa định kỳ.
- Ví dụ:
        
        rsync -avz --delete /home/user/source/ user@server:/home/user/destination/
- -a: Bảo toàn thuộc tính.
- -v: Chi tiết, hiển thị thông tin chi tiết về quá trình truyền.
- -z: Nén dữ liệu để tiết kiệm băng thông.
- --delete: Xóa các tệp trong thư mục đích không còn tồn tại trong thư mục nguồn.
- Lệnh trên sẽ đồng bộ hóa thư mục source trên máy cục bộ với thư mục destination trên máy chủ từ xa, chỉ truyền những phần đã thay đổi và xóa các tệp đã bị xóa trong thư mục nguồn.
## cat command
1. **Nhập nội dung 1 file**
    
        cat > newfile.txt
- Lệnh này sẽ tạo một tệp mới tên là newfile.txt. Bạn có thể nhập nội dung vào tệp và nhấn Ctrl+D để lưu và thoát.
2. **Nhập/tìm dòng thứ n trong file**

        cat -n file.txt  # Hiển thị nội dung của file.txt với số dòng
        
        cat file1.txt file2.txt | more  # Kết hợp nội dung của hai tệp và hiển thị từng trang


3. **Nhập nhiều dòng dùng OEF**
## echo command
1. **Chèn thêm dòng**
- Thêm một dòng mới vào cuối tệp:
  
      echo "Dòng mới được thêm vào" >> data.txt

2. **overwite**
- Ghi đè toàn bộ tệp bằng một dòng mới:
        
        echo "Đây là dòng duy nhất" > data.txt

## tail và head command
1. **tail/tailf**
- Chức năng: Hiển thị các dòng cuối cùng của một tệp.
- Cú pháp:

         tail [-n số_dòng] tên_tệp
-   VD:

        tail -n 10 error.log  # Hiển thị 10 dòng cuối của tệp error.log

2. **head**
- Chức năng: Hiển thị các dòng đầu tiên của một tệp.
- Cú pháp:
    
        head [-n số_dòng] tên_tệp
- Ví dụ:

        head -n 5 log.txt  # Hiển thị 5 dòng đầu của tệp log.txt

## sed command
1.**find và replace chuỗi trong file**
-   Vd:
 
        sed 's/a/A/g' file.txt
2.**Xóa tất cả các dòng trống trong file**

          sed '/^$/d' file.txt
## Traceroute/tracert command
- VD:
        
        traceroute google.com
- Lệnh trên sẽ thực hiện traceroute đến máy chủ Google. Kết quả sẽ hiển thị một bảng gồm các cột sau:
   - Hop: Số thứ tự của router.
    - IP: Địa chỉ IP của router.
    - ms: Thời gian trễ (milli giây) của gói tin khi đi qua router.

            traceroute to google.com (142.250.186.142), 30 hops max, 60 byte packets
            1  *       *     *
            2  *       *     * 
            3  1.1.1.1  10 ms  11 ms  12 ms
            4  2001:db8::85a3:8d0:d3:1  30 ms  29 ms  30 ms
            ...
            12  142.250.186.142  20 ms  19 ms  18 ms
## netstat command
**Hiển thị các loại kết nối:**
    -t: Hiển thị các kết nối TCP.
    -u: Hiển thị các kết nối UDP.
    -a: Hiển thị tất cả các kết nối, bao gồm cả các kết nối đang lắng nghe (listening).
    -n: Hiển thị địa chỉ IP và số cổng dưới dạng số thay vì tên dịch vụ.
    
**Hiển thị thông tin bổ sung:**
    -p: Hiển thị PID và tên chương trình đang sử dụng socket.
    -r: Hiển thị bảng định tuyến IP.
    -s: Hiển thị thống kê về các giao thức mạng (TCP, UDP, ICMP).
    -e: Hiển thị các thống kê mạng mở rộng (chỉ có trong Windows).
    -o: Hiển thị ID của tiến trình sở hữu mỗi kết nối (chỉ có trong Windows).

**Các tùy chọn khác:**
    -l: Hiển thị các socket đang lắng nghe (listening).
    -i: Hiển thị thống kê giao diện mạng.
    -z: Hiển thị các kết nối TCP trong trạng thái TIME_WAIT.

Ví dụ:
    - Hiển thị tất cả các kết nối TCP và UDP: **netstat -tu**
    - Hiển thị các kết nối TCP đang lắng nghe: **netstat -lt**
    - Hiển thị các kết nối TCP và UDP cùng với PID và tên chương trình: **netstat -tunp**
    - Hiển thị bảng định tuyến:**netstat -**r
    - Hiển thị thống kê về giao thức TCP: **netstat -s tcp**

**Một số lệnh kết hợp thường dùng:**
Kiểm tra các dịch vụ đang chạy: **netstat -tulnp**
    Xác định tiến trình đang sử dụng một cổng: **netstat -tunlp | grep :80 (để tìm tiến trình sử dụng cổng 80)**
  Kiểm tra các kết nối đến từ một máy khác: **netstat -tunlp | grep 192.168.1.100 (để tìm các kết nối đến từ 192.168.1.100)**

# sort command
- Cú pháp:
        
        sort [Tùy chọn] [Tệp đầu vào] > [Tệp đầu ra]
- Các tùy chọn thường dùng
 -n: Sắp xếp theo thứ tự số.
    -r: Đảo ngược thứ tự sắp xếp.
    -f: Bỏ qua sự khác biệt giữa chữ hoa và chữ thường.
    -t: Chỉ định ký tự phân cách các trường.
    -k: Chỉ định trường để sắp xếp.
    -u: Loại bỏ các dòng trùng lặp.

vd 1: **sort theo thứ tự tăng dần**

    sort -n numbers.txt > numbers_sorted.txt

vd 2: **sort theo thứ tự giảm dần**
    
    sort -nr numbers.txt > numbers_sorted_desc.txt

vd 3: **sort theo column**

    sort -t 'ký_tự_phân_cách' -k cột_cần_sắp_xếp [tệp_vào] > [tệp_ra]

# uniq command
1. **lọc ra các dòng lặp lại trong một file**

2. **lọc ra các dòng lặp lại trong file và đếm số lượng các dòng lặp lại**
# wc command
- Cú pháp:
    
         wc [Tùy chọn] [Tệp]
- Các tùy chọn thường dùng:
  -l: Đếm số dòng.
    -w: Đếm số từ.
    -c: Đếm số byte.
    -m: Đếm số ký tự.
    -L: Hiển thị độ dài của dòng dài nhất.

vd 1:  **Đếm số dòng trong file**
    
            wc -l myfile.txt

vd 2: **Đếm số kí tự trong file**
        
            wc -m myfile.txt

 # **Phân quyền**
 - Trong Linux, có ba chủ thể chính liên quan đến phân quyền:
    - Chủ sở hữu (Owner): Người tạo ra tệp hoặc thư mục.
    - Nhóm (Group): Một nhóm người dùng có quyền truy cập đặc biệt.
    - Khác (Others): Tất cả người dùng còn lại.

- Các quyền cơ bản:
Mỗi chủ thể có thể có ba loại quyền cơ bản:
    - Đọc (r): Cho phép xem nội dung của tệp hoặc danh sách các tệp trong thư mục.
    - Ghi (w): Cho phép sửa đổi nội dung của tệp hoặc tạo/xóa tệp trong thư mục.
    - Thực thi (x): Cho phép chạy các chương trình (đối với tệp) hoặc truy cập vào thư mục.
- Phân quyền thường được biểu diễn dưới dạng một chuỗi ký tự gồm 9 ký tự, mỗi ký tự đại diện cho một quyền của một chủ thể. Ví dụ: rwxr-xr-x = 751 có nghĩa là:

    - Chủ sở hữu: Có quyền đọc, ghi và thực thi.
    - Nhóm: Có quyền đọc và thực thi.
    - Khác: Có quyền đọc.
    - r tương đương 4
    - w tương đương 2
    - x tương đương 1  
  
  
         chown [Tùy chọn] owner:group file_hoặc_thư_mục
> owner: Tên người dùng mới sẽ trở thành chủ sở hữu.
group: Tên nhóm mới sẽ trở thành nhóm sở hữu.
file_hoặc_thư_mục: Đường dẫn đến file hoặc thư mục cần thay đổi quyền sở hữu.

 VD:

    chown -R root:admin project
 **Set Immutable Attribute**
 - Thuộc tính không đổi (Immutable Attribute) trong Linux là một cờ đặc biệt được gán cho một file hoặc thư mục, khiến nó trở nên "bất khả xâm phạm". Điều này có nghĩa là file hoặc thư mục đó không thể bị:
   
    -  Sửa đổi: Bạn không thể thay đổi nội dung của file.
    Xóa: Bạn không thể xóa file hoặc thư mục.
    Đổi tên: Bạn không thể đổi tên file hoặc thư mục.
- VD: 

            sudo chattr +i filename
 ## Find command
 1. **find các file có đuôi .log**
        
        find . -name "*.log"
2. **Tìm kiếm các folder có tên abc**

        find . -name "abc" -type d
3. **Tìm kiếm các file có tên abc**

        find . -name "abc"
4. **Tìm kiếm các file có tên abc và thực hiện phần quyền read only**
    
        find . -name "abc" -exec chmod 444 {} \;
## CP command
- **Cú Pháp**
    
        cp [tùy chọn] nguồn đích
- VD:

        cp -r project project_backup
## mv command
- Cú pháp

        mv [tùy chọn] nguồn đích
- Di chuyển file old_file.txt thành new_file.txt:

        mv old_file.txt new_file.txt
- Di chuyển thư mục temp vào thư mục data:  

        mv temp data/
## Cut command
1. Cắt ký tự thứ n trong string:

        echo "abcdefg" | cut -c n
2. Cắt từ ký tự thứ n trở về sau:

        echo "abcdefg" | cut -c n-

## Dig command
1. Kiểm tra các bản ghi A, MX, NS:

        dig example.com
2. Kiểm tra các bản ghi A, MX, NS với custom DNS: 
    
        dig @8.8.8.8 example.com
- Lệnh trên sẽ sử dụng máy chủ DNS 8.8.8.8 (Google DNS) để truy vấn thông tin về example.com.
3. Kiểm tra bản ghi MX của gmail.com:

        dig +short -t MX gmail.com

## tar/zip/unzip command
- Cú pháp

        tar [tuỳ chọn] [tên_kho_lưu_trữ] [file_hoặc_thư_mục]
- Các tùy chọn thường dùng:
> c: Tạo một kho lưu trữ mới.
>  x: Giải nén một kho lưu trữ.
>   v: Hiển thị chi tiết quá trình nén/giải nén.
>   f: Chỉ định tên file của kho lưu trữ.
>   z: Sử dụng gzip để nén.
>   j: Sử dụng bzip2 để nén

Ví dụ:
    
- Tạo một kho lưu trữ tên là archive.tar.gz chứa thư mục documents:

         tar -czvf archive.tar.gz documents

## mount/umount command
1. Kiểm tra các ổ cứng:
    - Lệnh fdisk -l: Hiển thị thông tin chi tiết về các phân vùng trên tất cả các ổ đĩa.
    - Lệnh lsblk: Hiển thị thông tin về các block device (bao gồm ổ cứng, ổ SSD, USB...) một cách trực quan hơn.

            sudo fdisk -l
            sudo lsblk
2. Mount ổ cứng vào /mnt/test
- Tạo mount point
        
        sudo mkdir /mnt/test
- Gắn kết ổ cứng:

        sudo mount /dev/sdb /mnt/test
3. Umount ổ cứng

        sudo umount /mnt/test

## Symbolic Links, Hard Links  command

### Định nghĩa

**Symbolic Link (Liên kết tượng trưng):**

  - Là một file đặc biệt trỏ đến một file hoặc thư mục khác.
   - Có thể hiểu như một shortcut trong Windows.
    Khi bạn truy cập vào symbolic link, hệ thống sẽ tự động chuyển hướng đến file hoặc thư mục mà nó đang trỏ tới.
    Symbolic link có thể trỏ đến bất kỳ file hoặc thư mục nào, kể cả trên các phân vùng khác nhau hoặc thậm chí trên các máy khác (nếu sử dụng mạng).
> Ví dụ
        
        ln -s /etc/passwd /home/user/mypasswd
-   Lệnh trên sẽ tạo một symbolic link tên là mypasswd trong thư mục home của user, trỏ đến file /etc/passwd. Khi bạn mở mypasswd, bạn sẽ thực sự đang mở file /etc/passwd.

**Hard Link (Liên kết cứng):**
    
- Là một tên khác của một file đã tồn tại.
    Cả hai tên đều trỏ đến cùng một inode (một cấu trúc dữ liệu trong hệ thống file chứa thông tin về file), nghĩa là chúng cùng chia sẻ dữ liệu.
    Không thể tạo hard link cho các thư mục.
> Ví dụ

        ln /etc/passwd /etc/passwd2
- Lệnh trên sẽ tạo một hard link tên là passwd2 trỏ đến cùng một file /etc/passwd. Cả passwd và passwd2 đều là cùng một file, chỉ có tên khác nhau

## Ls (list) command

        ls [options] [directory]


-  -l: Liệt kê chi tiết, bao gồm quyền truy cập, chủ sở hữu, nhóm, kích thước, thời gian sửa đổi cuối cùng.
- -a: Hiển thị cả các file ẩn (bắt đầu bằng dấu chấm).
- -h: Hiển thị kích thước file dưới dạng dễ đọc (ví dụ: 1K, 2.3M).
- -r: Đảo ngược thứ tự sắp xếp.

## ps command

        ps [options]

**Các tùy chọn thường dùng**:

- -a: Hiển thị tất cả các tiến trình.
- -u: Hiển thị thông tin về người dùng sở hữu tiến trình. 
- -x: Hiển thị cả các tiến trình không có terminal điều khiển.

VD:
            
            ps aux

-   Hiển thị tất cả các tiến trình với thông tin chi tiết về người dùng, CPU, bộ nhớ, thời gian bắt đầu,...
## kill command
        kill PID
 > Thay PID bằng PID (Process ID) của tiến trình muốn giết. Ví dụ: kill 1234.

## TOP commnad
**Các lệnh trong top:**
- P: Sắp xếp các tiến trình theo mức sử dụng CPU.
 -  M: Sắp xếp các tiến trình theo mức sử dụng bộ nhớ.
  -  q: Thoát khỏi top.

## Load Average
-   Load average là một chỉ số quan trọng để đánh giá tải trọng của hệ thống. Nó thể hiện số lượng trung bình các tiến trình đang chờ để được xử lý bởi CPU trong một khoảng thời gian nhất định (thường là 1, 5, và 15 phút).

    -   Giá trị thấp: Hệ thống đang hoạt động ổn định, không quá tải.
    -   Giá trị cao: Hệ thống đang quá tải, các tiến trình phải chờ đợi lâu để được xử lý.

**Process States**

Trong lệnh top, các trạng thái của tiến trình được biểu diễn bằng các chữ cái:

   > R: Running (đang chạy)
    S: Sleeping (đang ngủ, có thể đánh thức)
    D: Uninterruptible sleep (đang ngủ, không thể đánh thức)
    T: Stopped (dừng)
    Z: Zombie (tiến trình kết thúc nhưng chưa được thu hồi)

Ngoài ra, còn có các thông số:

> us: Thời gian CPU sử dụng bởi các tiến trình người dùng.
    sy: Thời gian CPU sử dụng bởi hệ điều hành.
    ni: Thời gian CPU sử dụng bởi các tiến trình có ưu tiên thấp.
    id: Thời gian CPU nhàn rỗi.
    wa: Thời gian chờ I/O.
    hi: Thời gian sử dụng CPU bởi các tiến trình ảo hóa.
    si: Thời gian sử dụng CPU bởi các tiến trình steal time (cho các hệ thống đa xử lý).
    st: Thời gian CPU bị đánh cắp bởi các hypervisor.

Zombie Process

-   Zombie process là một tiến trình đã kết thúc nhưng chưa được thu hồi bởi tiến trình cha. Nó vẫn tồn tại trong bảng tiến trình và chiếm một slot. Để tránh tình trạng quá nhiều zombie process, nên đảm bảo các tiến trình cha thu hồi con của mình một cách đúng đắn.
## Free command

-   Lệnh free cung cấp thông tin về bộ nhớ hệ thống (RAM) của máy tính.

> total: Tổng lượng RAM của hệ thống.
    used: Lượng RAM đang được sử dụng bởi các tiến trình và hệ điều hành.
    free: Lượng RAM trống không được sử dụng bởi bất kỳ tiến trình nào.
    shared: Lượng RAM được chia sẻ giữa các tiến trình.
    buff/cache: Lượng RAM đang được sử dụng làm bộ nhớ đệm để tăng tốc độ truy cập dữ liệu.
    available: Lượng RAM thực sự có thể được sử dụng ngay lập tức mà không cần phải giải phóng bộ nhớ cache.

**Lưu ý**:

- used không bằng total - free vì một phần RAM được sử dụng làm bộ nhớ cache.
 -  available là chỉ số chính xác hơn để đánh giá khả năng sử dụng RAM của hệ thống.

## df command


Lệnh df hiển thị thông tin về dung lượng đĩa (disk space) đã sử dụng trên các hệ thống file.

   > Filesystem: Hệ thống file.
    1K-blocks: Tổng số khối 1K trên hệ thống file.
    Used: Số khối đã được sử dụng.
    Available: Số khối trống có thể sử dụng.
    Use%: Tỷ lệ phần trăm dung lượng đã sử dụng.
    Mounted on: Điểm gắn kết của hệ thống file.
