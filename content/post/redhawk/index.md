---
title: Redhawk
description: "Thu thập thông tin và quét lỗ hổng với Red Hawk"
slug: redhawk
date: 2020-09-23 09:00:00+0000
image: redhawk.png
categories:
    - RedTeam
tags:
    - Open Source
    - Information Gathering
    - Tools
    - Hacking
---

<p>Red Hawk là một công cụ mã nguồn mở được sử dụng để thu thập thông tin và quét lỗ hổng nhất định. Red Hawk phát hiện Hệ thống quản lý nội dung (CMS) khi sử dụng ứng dụng web đích, địa chỉ IP, bản ghi máy chủ web, thông tin Cloudflare và dữ liệu robot.txt. Red Hawk có thể phát hiện WordPress, Drupal, Joomla và Magento CMS. Các tính năng quét khác của Red Hawk bao gồm thu thập dữ liệu WHOIS, tra cứu Geo-IP, lấy biểu ngữ, tra cứu DNS, quét cổng, thông tin tên miền phụ, IP đảo ngược và tra cứu bản ghi MX. Là một trình quét lỗ hổng, Red Hawk tìm kiếm các lỗi SQL dựa trên lỗi, các tệp nhạy cảm với WordPress và các lỗ hổng liên quan đến phiên bản WordPress.</p>
<h1 id="yeu-cau">Yêu cầu</h1>
<p>Red Hawk là một công cụ thu thập thông tin và quét lỗ hổng, được phát triển bởi R3D#@0R_2H1N A.K.A Tuhinshubhra. Đây là một dự án mã nguồn mở nên các bạn có thể tải về thông qua github:</p>
<blockquote>
<p><a href="https://github.com/Tuhinshubhra/RED_HAWK">https://github.com/Tuhinshubhra/RED_HAWK</a></p>
</blockquote>
<h1 id="thuc-hanh">Thực hành</h1>
<h2 id="buoc-1-nhap-duong-dan-host-can-quet">Bước 1: Nhập đường dẫn host cần quét</h2>
<p>Sau khi tải công cụ về, trỏ đến thư mục công cụ và chạy công cụ bằng lệnh như sau:</p>
<pre><code class="lang-bash"><span class="hljs-built_in">cd</span> RED_HAWK/
php rhawk.php
</code></pre>
<p>Sau khi chạy sẽ hiện lên giao diện và yêu cầu nhập đường dẫn của url mà bạn cần scan. Ở đây, mình sẽ chọn website đã có sẵn các lổ hỗng: <a href="http://demo.testfire.net">http://demo.testfire.net</a>.</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/23/redhawk-01.png#full" alt="redhawk-01.png"></p>
<p>Sau khi nhập xong host cần scan, chúng ta sẽ chọn giao thức truyền là http hoặc https. Đối với website này, tôi khuyên các bạn nên chọn giao thức http để có thể scan được nhiều lỗ hổng hơn. Giao diện menu sẽ hiển thị ra như hình dưới đây:</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/23/redhawk-02.png#full" alt="redhawk-02.png"></p>
<h2 id="buoc-2-kiem-tra-thong-tin-co-ban-voi-recon">Bước 2: Kiểm tra thông tin cơ bản với Recon</h2>
<p>Chúng ta sẽ chọn số 0 để sử dụng công cụ Recon cho phép thực hiện việc thu thập thông tin cơ bản như tên website, địa chỉ IP,... Một loạt các thông tin cơ bản hiện ra bao gồm như sau:</p>
<ul>
<li><strong>Site Title</strong>: Tiêu đề chính của website.</li>
<li><strong>IP adress</strong>: Địa chỉ IP của website.</li>
<li><strong>Web Server</strong>: Máy chủ dùng để host website.</li>
<li><strong>CMS</strong>: Hệ thống quản lý của website.</li>
<li><strong>Cloudflare</strong>: Chứng chỉ kỹ thuật số.</li>
<li><strong>Robots File</strong>: Có chứa file robots.txt hay không.</li>
</ul>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/23/redhawk-03.png#full" alt="redhawk-03.png"></p>
<p>Gõ Enter để tiếp tục sử dụng công cụ.</p>
<h2 id="buoc-3-kiem-tra-vi-tri-may-chu-voi-geo-ip">Bước 3: Kiểm tra vị trí máy chủ với Geo-IP</h2>
<p>Chúng ta sẽ chọn số 2 để tìm ra vị trí máy chủ thông qua công cụ GEO-IP. Chúng ta sẽ có được những thông tin cơ bản như sau:</p>
<ul>
<li><strong>IP Adress</strong>: Địa chỉ IP của website.</li>
<li><strong>Country</strong>: Quốc gia nào chứa máy chủ.</li>
<li><strong>State</strong>: Tiểu bang nào (chỉ hợp lệ đối với Mỹ).</li>
<li><strong>City</strong>: Thành phố nào.</li>
<li><strong>Latitude</strong>: Vĩ độ (tọa độ).</li>
<li><strong>Longtitude</strong>: Kinh độ (tọa độ).</li>
</ul>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/23/redhawk-04.png#full" alt="redhawk-04.png"></p>
<h2 id="buoc-4-kiem-tra-header-voi-grabbanner">Bước 4: Kiểm tra header với GrabBanner</h2>
<p>HTTP Header là một phần của response được gửi bởi máy chủ của bạn với một trang được yêu cầu. Nó bao gồm các chi tiết như Response code (thành công, chuyển hướng, v.v.), Loại nội dung, Thời gian sửa đổi lần cuối,... Sử dụng công cụ Banner Grabber hiển thị thông tin của header website sử dụng số 3.</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/23/redhawk-05.png#full" alt="redhawk-05.png"></p>
<h2 id="buoc-5-kiem-tra-dns">Bước 5: Kiểm tra DNS</h2>
<p>Ngoài ra, chúng ta còn có thể xem được địa chỉ IP thông qua DNS Lookup.</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/23/redhawk-06.png#full" alt="redhawk-06.png"></p>
<h2 id="buoc-6-kiem-tra-port-voi-nmap">Bước 6: Kiểm tra port với NMap</h2>
<p>Công cụ Nmap đã quá nổi tiếng với chúng ta, mình chỉ sử dụng một phần chức năng scan port vì đây là chức năng mạnh nhất của Nmap cho phép quét hết tất cả các port đang mở hay đóng.</p>
<p><img class="article-img" src="https://raw.githubusercontent.com/minhgiau998/image/develop/2020/09/23/redhawk-07.png#full" alt="redhawk-07.png"></p>
<p>Còn nhiều tính năng khác các bạn có thể thử như SQLi Scanner, Subdomain Scanner, Crawler,...</p>
<h1 id="ket-luan">Kết luận</h1>
<p>Công cụ Redhawk là một công cụ thu thập thông tin được tích hợp nhiều công cụ khác nhau bao gồm Recon, Whois, Geo-IP, Nmap, SQLi, ... Đây sẽ là một hành mạnh mẽ trang giúp cho chúng ta có thể thực hiện thu thập thông tin bất kỳ đối tượng nào.</p>
