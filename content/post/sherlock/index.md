---
title: "Sherlock"
description: "Introduction to sherlock"
slug: sherlock
date: '2020-10-09 09:00:00'
image: sherlock.png
categories: 
    - Tools
    - Information Gathering
tags:
    - Penetration Testing
    - Tools
    - Information Gathering
---

<p>Sherlock là một công cụ được xây dựng trên ngôn ngữ <strong>Python</strong> và nó có thể truy tìm nhiều tài khoản người dùng được tạo bởi cùng một người trên nhiều nền tảng mạng xã hội với tên thật của người dùng hoặc tên tài khoản của họ. Bạn có thể xem thông tin chi tiết của project này bằng liên kết dưới đây:</p>
<blockquote>
<p><a href="https://github.com/sherlock-project/sherlock">https://github.com/sherlock-project/sherlock</a></p>
</blockquote>
<h1 id="yeu-cau">Yêu cầu</h1>
<ul>
<li><strong>Git</strong> (tùy chọn): Bạn có thể sử dụng công cụ này để tải sherlock từ <strong>Github</strong>.</li>
<li><strong>Python 3.6+</strong>: Sherlock chạy trên ngôn ngữ Python (phiên bản 3.6 trở lên).</li>
</ul>
<h1 id="thuc-hanh">Thực hành</h1>
<h2 id="buoc-1-cai-dat-sherlock">Bước 1: Cài đặt sherlock</h2>
<p>Đầu tiên, bạn mở giao diện cửa sổ terminal và sao chép câu lệnh dưới đây để tiến hành tải về Sherlock.</p>
<pre><code class="lang-bash">git <span class="hljs-keyword">clone</span> <span class="hljs-title">https</span>://github.com/sherlock-project/sherlock
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/10/09/sherlock-01.png#full" alt="sherlock-01.png"></p>
<p>Tiếp theo, bạn trỏ vào thư mục project vừa mới tải về và cài đặt các thư viện cần thiết cho Sherlock.</p>
<pre><code class="lang-bash">cd <span class="hljs-keyword">sherlock
</span>pip3 <span class="hljs-keyword">install </span>-r requirements.txt
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/10/09/sherlock-02.png#full" alt="sherlock-02.png"></p>
<p>Nếu bạn cần biết cách sử dụng của tất cả các lệnh mà Sherlock hỗ trợ thì có thể tham khảo câu lệnh dưới đây:</p>
<pre><code class="lang-python"><span class="hljs-attribute">python sherlock -h</span>
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/10/09/sherlock-03.png#full" alt="sherlock-03.png"></p>
<h2 id="buoc-2-xac-dinh-muc-tieu">Bước 2: Xác định mục tiêu</h2>
<p>Bản thân tôi là một người thích nhạc cổ điển, đặc biệt là ban nhạc <strong>The Beatles</strong>. Tôi muốn kiểm tra xem hiện tại có bao nhiêu mạng xã hội mà nhóm nhạc nổi tiếng này đang tham gia bằng cách dự đoán tên tài khoản. Thường thì các nghệ sĩ nổi tiếng khi tạo tài khoản sẽ muốn lấy các chữ cái viết thường và ghép lại, đơn giản sẽ là <strong>thebeatles</strong> thôi!</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/10/09/sherlock-04.png#full" alt="sherlock-04.png"></p>
<h2 id="buoc-3-truy-tim-tai-khoan">Bước 3: Truy tìm tài khoản</h2>
<p>Cuối cùng, chúng ta sẽ sử dụng câu lệnh này để tìm tất cả mạng xã hội mà nhóm nhạc này đã tham gia.</p>
<pre><code class="lang-Python"><span class="hljs-keyword">python</span> sherlock thebeatles --<span class="hljs-keyword">print</span>-found
</code></pre>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/10/09/sherlock-05.png#full" alt="sherlock-05.png"></p>
<h1 id="ket-luan">Kết luận</h1>
<p>Sherlock là một công cụ rất hữu ích trong việc truy tìm tài khoản của một người dùng xem họ đang sử dụng mạng xã hội nào. Tuy nhiên, điểm hạn chế của công cụ này là những người dùng bình thường thì sẽ đặt những tên tài khoản khác nhau, do đó sẽ rất khó để có thể truy tìm mạng xã hội nào mà họ đang tham gia hay chỉ là một tài khoản giả mạo của một người dùng nào khác.</p>
