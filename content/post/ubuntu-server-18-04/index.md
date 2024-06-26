---
title: "Ubuntu Server 18.04"
description: "How to build Ubuntu Server 18.04 ?"
slug: ubuntu-server-18.04
date: '2020-09-27 09:00:00'
image: ubuntu-server-18-04.png
categories: 
    - OS
    - Linux
tags:
    - OS
    - Linux
    - Ubuntu
---

<p>Bài viết này đưa ra tình huống bạn sẽ truy cập máy chủ từ xa. Máy chủ này đã được cài đặt sẵn hệ điều hành Ubuntu Server 18.04. Máy chủ này cần phải là máy chủ có cài đặt SSH và bạn cần có quyền truy cập root.</p>
<p>Chúng ta sẽ sử dụng địa chỉ IP của máy chủ là địa chỉ IP của máy chủ từ xa. Chúng ta sử dụng tên tài khoản để thực hiện kết nối tới máy chủ để đăng nhập.</p>
<h1 id="dang-nhap-tu-xa">Đăng nhập từ xa</h1>
<p>Từ máy tính sử dụng hệ điều hành Linux, chúng ta thực hiện kết nối đến máy chủ với ssh.</p>
<pre><code class="lang-bash"><span class="hljs-attribute">ssh</span> root<span class="hljs-variable">@remote_server_ip</span>
</code></pre>
<h1 id="c-p-nh-t-m-y-ch-">Cập nhật máy chủ</h1>
<p>Cập nhật kho phần mềm bằng câu lệnh sau:</p>
<pre><code class="lang-bash">sudo apt-<span class="hljs-built_in">get</span> <span class="hljs-keyword">update</span>
</code></pre>
<p>Cài đặt các phần mềm đã cập nhật.</p>
<pre><code class="lang-bash">apt-<span class="hljs-keyword">get</span> upgrade
</code></pre>
<h1 id="nano">Nano</h1>
<p>Nếu như bạn không tự tin với một trình soạn thảo mặc định (ví dụ như Vim), bạn nên sử dụng nano. Nếu nano chưa được cài đặt, bạn có thể cài đặt dễ dàng bằng lệnh sau:</p>
<pre><code class="lang-bash">apt-<span class="hljs-keyword">get</span> install nano
</code></pre>
<h1 id="tao-mot-nguoi-dung-moi">Tạo một người dùng mới</h1>
<p>Đăng nhập dưới quyền root là điều mà chúng ta nên hạn chế. Thay vào đó, chúng ta sẽ tạo một người dùng mới với các đặc quyền sudo.</p>
<pre><code class="lang-bash"><span class="hljs-keyword">adduser </span>user_name
</code></pre>
<p>Bạn sẽ được yêu cầu đặt mật khẩu người dùng mới, sau đó một vài câu hỏi khác có thể để trống.</p>
<p>Bây giờ chúng ta cấp quyền sudo cho tài khoản này.</p>
<pre><code class="lang-bash">usermod <span class="hljs-_">-a</span>G sudo user_name
</code></pre>
<h1 id="tao-khoa-ssh">Tạo khóa SSH</h1>
<p>Nếu bạn đã có sẵn các khóa SSH, hãy bỏ qua phần này. Trên máy tính của bạn tạo cặp khóa SSH của bạn.</p>
<pre><code class="lang-bash"><span class="hljs-attribute">ssh-keygen</span>
</code></pre>
<p>Bạn sẽ được hỏi một số câu hỏi. Chấp nhận vị trí thư mục mặc định cho khóa. Trả lời những câu hỏi khác nếu như bạn muốn.</p>
<p><strong><em>Lưu ý: Nếu bạn để trống mật khẩu, thì hệ thống của bạn sẽ kém an toàn hơn. Sở hữu các khóa sẽ đủ để có được quyền truy cập. Thuận tiện thường là kẻ thù của bảo mật.</em></strong></p>
<h1 id="sao-chep-khoa-cong-khai">Sao chép khóa công khai</h1>
<p>Trên máy tính của bạn sao chép khóa SSH công khai của bạn vào máy chủ từ xa.</p>
<pre><code class="lang-bash">ssh-<span class="hljs-keyword">copy</span>-<span class="hljs-built_in">id</span> user_name@remote_server_ip
</code></pre>
<p><strong><em>Lưu ý: Hãy nhớ sử dụng mật khẩu cho người dùng mới.</em></strong></p>
<p>Sau khi khóa được sao chép thành công, hãy đăng nhập vào máy chủ từ xa với tên user_name.</p>
<pre><code class="lang-bash"><span class="hljs-attribute">ssh</span> user_name<span class="hljs-variable">@remote_server_ip</span>
</code></pre>
<h1 id="thiet-lap-ssh">Thiết lập SSH</h1>
<p>Chúng ta sẽ định cấu hình máy chủ SSH bằng cách chỉnh sửa tập tin sshd_config.</p>
<pre><code class="lang-bash">sudo nano <span class="hljs-regexp">/etc/</span>ssh<span class="hljs-regexp">/sshd_config</span>
</code></pre>
<p>Kiểm tra và đảm bảo rằng xác thực khóa công khai được bật. Tìm dòng bắt đầu với từ PubkeyAuthentication. Hãy chắc chắn rằng nó được đặt ở trạng thái yes.</p>
<pre><code class="lang-bash"><span class="hljs-attribute">PubkeyAuthentication</span> <span class="hljs-literal">yes</span>
</code></pre>
<p>Chúng ta muốn vô hiệu hóa xác thực mật khẩu. Tìm dòng bắt đầu với từ PasswordAuthentication. Đặt giá trị thành no.</p>
<pre><code class="lang-bash"><span class="hljs-attribute">PasswordAuthentication</span> <span class="hljs-literal">no</span>
</code></pre>
<p>Chúng ta không muốn quyền root có thể đăng nhập từ xa. Tìm dòng bắt đầu với từ PermitRootLogin. Đặt giá trị thành no.</p>
<pre><code class="lang-bash"><span class="hljs-attribute">PermitRootLogin</span> <span class="hljs-literal">no</span>
</code></pre>
<p>Tải lại máy chủ SSH. Điều này có thể khiến bạn mất kết nối SSH. Chúng ta khởi động lại dịch vụ ssh.</p>
<pre><code class="lang-bash"><span class="hljs-attribute">sudo service ssh restart</span>
</code></pre>
<h1 id="fail2ban">Fail2Ban</h1>
<p>Fail2Ban là một công cụ ngăn chặn xâm nhập rất mạnh mẽ. Nó có thể xem logs và địa chỉ IP tạm thời dựa trên hoạt động đáng ngờ. Chúng ta muốn fail2ban xem nhật ký SSH của chúng ta. Nếu một địa chỉ IP thực hiện nhiều request tệ, chúng ta sẽ tạm thời cấm địa chỉ IP này.</p>
<pre><code class="lang-bash">sudo apt-<span class="hljs-keyword">get</span> install fail2ban
</code></pre>
<p>Sao chép các tập tin cấu hình.</p>
<pre><code class="lang-bash">sudo cp <span class="hljs-regexp">/etc/</span>fail2ban<span class="hljs-regexp">/fail2ban.conf /</span>etc<span class="hljs-regexp">/fail2ban/</span>fail2ban.local
sudo cp <span class="hljs-regexp">/etc/</span>fail2ban<span class="hljs-regexp">/jail.conf /</span>etc<span class="hljs-regexp">/fail2ban/</span>jail.local
</code></pre>
<p>Chỉnh sửa tập tin jail.local và cho phép theo dõi SSH.</p>
<pre><code class="lang-bash">sudo nano /etc/fail2ban/jail.<span class="hljs-keyword">local</span>
</code></pre>
<p>Tìm phần [sshd]. Thay đổi kích hoạt thành true.</p>
<pre><code class="lang-bash"><span class="hljs-section">[sshd]</span>

<span class="hljs-attr">enabled</span> = <span class="hljs-literal">true</span>
<span class="hljs-attr">port</span> = <span class="hljs-number">22</span>
<span class="hljs-attr">filter</span> = sshd
<span class="hljs-attr">logpath</span> = /var/log/auth.log
<span class="hljs-attr">maxretry</span> = <span class="hljs-number">3</span>
</code></pre>

<p>Sau đó khởi động lại dịch vụ.</p>
<pre><code class="lang-bash">sudo systemctl <span class="hljs-built_in">restart</span> fal2ban
</code></pre>
<h1 id="tuong-lua">Tường lửa</h1>
<p>Tiến hành cài đặt <strong>Uncomplicated Firewall (UFW)</strong>.</p>
<pre><code class="lang-bash">sudo apt-<span class="hljs-keyword">get</span> install ufw
</code></pre>
<p>Cấu hình tường lửa của bạn sẽ thay đổi khi bạn thêm các chương trình máy chủ. Hướng dẫn này chỉ quan tâm đến máy chủ SSH.</p>
<pre><code class="lang-bash"><span class="hljs-attribute">sudo ufw allow ssh</span>
</code></pre>
<p>Sau đó kích hoạt tường lửa.</p>
<pre><code class="lang-bash">sudo ufw <span class="hljs-built_in">enable</span>
</code></pre>
<h1 id="dat-ten-may-chu">Đặt tên máy chủ</h1>
<p>Tên máy chủ mặc định có thể gây nhầm lẫn. Trong hướng dẫn này, chúng ta sẽ đặt tên cho máy chủ của mình là demo. Thay đổi tên máy chủ với lệnh như sau:</p>
<pre><code class="lang-bash">sudo nano <span class="hljs-regexp">/etc/</span>hostname
</code></pre>
<p>Thay đổi tên thành sdemo. Bây giờ chúng ta sẽ chỉnh sửa các tập tin máy chủ.</p>
<pre><code class="lang-bash">sudo nano <span class="hljs-regexp">/etc/</span>hosts
</code></pre>
<p>Nó sẽ trông giống như nội dung dưới đây:</p>
<pre><code class="lang-bash">127<span class="hljs-selector-class">.0</span><span class="hljs-selector-class">.0</span><span class="hljs-selector-class">.1</span> <span class="hljs-selector-tag">localhost</span>

# <span class="hljs-selector-tag">The</span> <span class="hljs-selector-tag">following</span> <span class="hljs-selector-tag">lines</span> <span class="hljs-selector-tag">are</span> <span class="hljs-selector-tag">desirable</span> <span class="hljs-selector-tag">for</span> <span class="hljs-selector-tag">IPv6</span> <span class="hljs-selector-tag">capable</span> <span class="hljs-selector-tag">hosts</span>

<span class="hljs-selector-pseudo">::1</span> <span class="hljs-selector-tag">ip6-localhost</span> <span class="hljs-selector-tag">ip6-loopback</span>
<span class="hljs-selector-tag">fe00</span><span class="hljs-selector-pseudo">::0</span> <span class="hljs-selector-tag">ip6-localnet</span>
<span class="hljs-selector-tag">ff02</span><span class="hljs-selector-pseudo">::1</span> <span class="hljs-selector-tag">ip6-allnodes</span>
<span class="hljs-selector-tag">ff02</span><span class="hljs-selector-pseudo">::2</span> <span class="hljs-selector-tag">ip6-allrouters</span>
</code></pre>

<p>Và thêm tên máy chủ của bạn như thế này.</p>
<pre><code class="lang-bash">127<span class="hljs-selector-class">.0</span><span class="hljs-selector-class">.0</span><span class="hljs-selector-class">.1</span> <span class="hljs-selector-tag">localhost</span>
127<span class="hljs-selector-class">.0</span><span class="hljs-selector-class">.1</span><span class="hljs-selector-class">.1</span> <span class="hljs-selector-tag">demo</span>

# <span class="hljs-selector-tag">The</span> <span class="hljs-selector-tag">following</span> <span class="hljs-selector-tag">lines</span> <span class="hljs-selector-tag">are</span> <span class="hljs-selector-tag">desirable</span> <span class="hljs-selector-tag">for</span> <span class="hljs-selector-tag">IPv6</span> <span class="hljs-selector-tag">capable</span> <span class="hljs-selector-tag">hosts</span>

<span class="hljs-selector-pseudo">::1</span> <span class="hljs-selector-tag">ip6-localhost</span> <span class="hljs-selector-tag">ip6-loopback</span>
<span class="hljs-selector-tag">fe00</span><span class="hljs-selector-pseudo">::0</span> <span class="hljs-selector-tag">ip6-localnet</span>
<span class="hljs-selector-tag">ff02</span><span class="hljs-selector-pseudo">::1</span> <span class="hljs-selector-tag">ip6-allnodes</span>
<span class="hljs-selector-tag">ff02</span><span class="hljs-selector-pseudo">::2</span> <span class="hljs-selector-tag">ip6-allrouters</span>
</code></pre>

<p>Bây giờ chúng ta khởi động lại.</p>
<pre><code class="lang-bash"><span class="hljs-attribute">sudo reboot</span>
</code></pre>
<h1 id="thoi-gian">Thời gian</h1>
<p>Cài đặt múi giờ thích hợp với nơi bạn ở. Đối với tôi đó là giờ Châu Á.</p>
<pre><code class="lang-bash"><span class="hljs-string">sudo </span><span class="hljs-string">timedatectl </span><span class="hljs-built_in">set-timezone</span> <span class="hljs-string">Asia/</span><span class="hljs-string">Ho_Chi_Minh</span>
</code></pre>
<p>Sau đó, đảm bảo rằng máy chủ đang sử dụng Network Time Protocol (NTP).</p>
<pre><code class="lang-bash"><span class="hljs-string">sudo </span><span class="hljs-string">timedatectl </span><span class="hljs-built_in">set-ntp</span> <span class="hljs-string">on</span>
</code></pre>
<h1 id="ket-luan">Kết Luận</h1>
<p>Chúng ta đã vừa cài đặt xong các tính năng cơ bản trên hệ điều hành Ubuntu Server phiên bản 18.04.</p>
