---
layout: post
title: "NightEagle APT: Lộ Diện Chiến Dịch Tấn Công Zero-Day Exchange Tinh Vi"
author: jane
categories: [ Công nghệ & Khoa học ]
image: assets/images/NightEagle_APT.jpg
tags: [sticky]
---


Các chiến dịch tấn công nâng cao của nhóm đe dọa dai dẳng cấp cao (APT) được biết đến với tên gọi “NightEagle,” hay còn được mã hóa nội bộ là APT-Q-95, đã được công ty an ninh mạng hàng đầu Qian Pangu tiết lộ trong một sự kiện mang tính đột phá tại Hội nghị và Triển lãm An ninh và Quốc phòng Mạng Quốc gia Malaysia năm 2025.

Kể từ năm 2023, Qian Pangu đã theo dõi tỉ mỉ nhóm này, những kẻ đã thể hiện sự linh hoạt vượt trội trong việc khai thác các lỗ hổng chưa được biết đến và triển khai mã độc tùy chỉnh để nhắm mục tiêu vào các ngành công nghiệp có giá trị cao ở Trung Quốc. Các mục tiêu này bao gồm các lĩnh vực công nghệ cao, bán dẫn chip, công nghệ lượng tử, trí tuệ nhân tạo và quân sự.

## Mục tiêu và Modus Operandi
### Mục tiêu chiến lược
Mục tiêu chính của NightEagle dường như là đánh cắp thông tin tình báo, được thực hiện với độ chính xác cao trước khi xóa sạch mọi dấu vết xâm nhập. Sự tập trung vào các lĩnh vực công nghệ tiên tiến và quân sự cho thấy mục tiêu chiến lược của nhóm nhằm thu thập dữ liệu nhạy cảm có thể ảnh hưởng đến lợi thế cạnh tranh hoặc an ninh quốc gia. Sự liên kết các mục tiêu của chúng với những thay đổi địa chính trị và sự phát triển bùng nổ của ngành công nghiệp AI tại Trung Quốc càng củng cố giả thuyết này, với việc sử dụng các tên miền như “comfyupdate.org” liên quan đến các công cụ AI là một minh chứng rõ ràng.

### Kỹ thuật ẩn mình và cơ sở hạ tầng
Phương thức hoạt động (modus operandi) của NightEagle được đặc trưng bởi việc chuyển đổi cơ sở hạ tầng nhanh chóng. Nhóm này tận dụng nguồn tài chính dồi dào để mua một lượng lớn máy chủ VPS (Virtual Private Server) và tên miền. Đáng chú ý, chúng thường sử dụng một tên miền duy nhất cho mỗi mục tiêu, kết hợp với các độ phân giải IP động để né tránh sự phát hiện. Kỹ thuật này khiến cho việc theo dõi và chặn các hoạt động của chúng trở nên cực kỳ khó khăn đối với các tổ chức phòng thủ mạng, vì mỗi chiến dịch hoặc mục tiêu có thể sử dụng một dấu vết mạng hoàn toàn mới.

## Vector Tấn công và Chuỗi Khai thác
### Giai đoạn xâm nhập ban đầu
Qian Pangu lần đầu tiên phát hiện hoạt động đáng ngờ thông qua hệ thống Qianxin Tianyan NDR (Network Detection and Response) của họ. Hệ thống này đã xác định một yêu cầu DNS bất thường tới “synologyupdates.com” – một tên miền giả mạo dịch vụ NAS của Synology. Điều đặc biệt là tên miền này lại phân giải thành các IP cục bộ như 127.0.0.1 (localhost) để che giấu vị trí máy chủ thực. Kỹ thuật này giúp kẻ tấn công làm phức tạp quá trình điều tra bằng cách ẩn đi điểm đến cuối cùng của lưu lượng truy cập ban đầu, khiến việc truy vết nguồn gốc tấn công trở nên khó khăn hơn.

### Mã độc tùy chỉnh: “SynologyUpdate.exe”
Phân tích sâu hơn thông qua Qianxin AISOC (Artificial Intelligence Security Operations Center) đã tiết lộ một loại mã độc tùy chỉnh được viết bằng ngôn ngữ Go, có tên là “SynologyUpdate.exe.” Mã độc này thuộc họ Chisel, được thiết kế để tạo điều kiện thuận lợi cho việc xâm nhập mạng nội bộ thông qua kết nối SOCKS được mã hóa cứng (hardcoded) qua cổng 443. Việc sử dụng cổng 443, thường dùng cho lưu lượng HTTPS hợp pháp, giúp mã độc này hòa lẫn vào lưu lượng mạng thông thường, tránh bị phát hiện bởi các hệ thống giám sát mạng. Mã độc này được kích hoạt bốn giờ một lần thông qua các tác vụ đã lên lịch (scheduled tasks), tạo tiền đề cho các cuộc xâm nhập sâu hơn vào hệ thống mục tiêu. Tính năng định kỳ này đảm bảo sự bền vững của quyền truy cập, ngay cả khi các phiên trước đó bị gián đoạn.

### Mã độc cư trú trong bộ nhớ: “Memory Horse”
Kho vũ khí của nhóm NightEagle bao gồm một loại mã độc độc đáo cư trú trong bộ nhớ, được gọi là “memory horse” (ngựa bộ nhớ). Mã độc này được tiêm vào các máy chủ Exchange mà không để lại dấu vết trên đĩa cứng. Đặc điểm này khiến nó gần như vô hình đối với các công cụ chống vi-rút truyền thống và các giải pháp bảo mật dựa trên tệp. Payload này được tải thông qua một DLL ASP.NET có tên “App_Web_cn*.dll.” Khi được tải, nó tạo ra các thư mục URL ảo dưới các đường dẫn như “/owa/auth/” để thực thi các chức năng độc hại. Việc sử dụng các đường dẫn trông có vẻ hợp pháp giúp mã độc này ngụy trang hoạt động của mình trong môi trường Exchange, làm cho việc phát hiện thủ công trở nên cực kỳ khó khăn.

### Khai thác Zero-day Exchange Server
NightEagle còn sử dụng một chuỗi khai thác zero-day của Exchange chưa được tiết lộ. Chuỗi khai thác này cho phép kẻ tấn công thực hiện một loạt các hoạt động độc hại nghiêm trọng: đánh cắp khóa máy chủ, hủy serialize dữ liệu, và thu thập email từ xa từ các cá nhân mục tiêu. Nhóm này có khả năng lặp đi lặp lại việc kiểm tra qua các phiên bản Exchange khác nhau cho đến khi tìm thấy một phiên bản phù hợp để khai thác. Sự tinh vi của chuỗi khai thác zero-day này cho thấy năng lực kỹ thuật vượt trội và khả năng đầu tư vào việc phát hiện các lỗ hổng chưa được biết đến, mang lại cho chúng một lợi thế đáng kể trong việc xâm nhập và duy trì quyền kiểm soát trong các môi trường tổ chức.

## Chiến thuật Exfiltration và Hoạt động
### Thu thập và truyền dữ liệu
Phân tích lưu lượng truy cập của Qianxin Tianyan NDR đã chỉ ra rằng dữ liệu email quan trọng đã bị đánh cắp trong gần một năm từ các thực thể bị ảnh hưởng. Điều này cho thấy khả năng duy trì sự hiện diện và hoạt động lén lút của NightEagle trong một thời gian dài mà không bị phát hiện, nhấn mạnh sự cần thiết của các giải pháp giám sát mạng liên tục và toàn diện.

### Khung giờ hoạt động
Các mô hình hành vi cho thấy NightEagle hoạt động nghiêm ngặt trong khoảng thời gian từ 9 giờ tối đến 6 giờ sáng theo giờ Bắc Kinh. Khung giờ này có thể cho thấy nguồn gốc của nhóm đến từ Múi giờ số 8 phía Tây của Bắc Mỹ, nơi giờ làm việc ban ngày của họ sẽ trùng khớp với khung giờ tấn công ban đêm ở Bắc Kinh. Việc tuân thủ một lịch trình hoạt động cụ thể này có thể là một chiến thuật để tận dụng thời gian hoạt động thấp điểm của các tổ chức mục tiêu hoặc để phù hợp với giờ làm việc của chính nhóm.

### Chỉ số thỏa hiệp (Indicators of Compromise – IOCs)
Thông tin tình báo đe dọa của Qian Pangu đã xác định một số tên miền độc hại và chữ ký tấn công cụ thể liên quan đến các chiến dịch của NightEagle. Các tổ chức trên toàn cầu được khuyến nghị kiểm tra máy chủ Exchange của mình để tìm kiếm các thành phần IIS và yêu cầu URL đáng ngờ. Các chỉ số thỏa hiệp chính đã được xác định bao gồm:

### Tên miền giả mạo Synology: synologyupdates.com
### Tên miền liên quan đến AI: comfyupdate.org
Các tổ chức nên cấu hình hệ thống giám sát và bảo mật của mình để phát hiện bất kỳ tương tác nào với các tên miền này, cũng như tìm kiếm các dấu hiệu của mã độc “SynologyUpdate.exe” và mã độc “memory horse.” Đặc biệt, việc kiểm tra các tác vụ đã lên lịch và các thư mục URL ảo dưới các đường dẫn /owa/auth/ trên máy chủ Exchange là rất quan trọng.

## Phát hiện và Phòng ngừa
Để đối phó với các mối đe dọa từ NightEagle, các công cụ như Qianxin’s APT-Q-95 Exchange Memory Self-Check và Tianqing Terminal Management System hiện đã có sẵn để hỗ trợ phát hiện và giảm thiểu các mối đe dọa này. Các giải pháp này được hỗ trợ bởi khả năng hợp nhất dữ liệu đa nguồn (multi-source data fusion) trên các nền tảng NDR, EDR (Endpoint Detection and Response) và AISOC. Sự kết hợp này cho phép phản ứng tự động trong vòng vài phút, nâng cao đáng kể khả năng của các tổ chức trong việc nhanh chóng xác định, phân tích và trung hòa các cuộc tấn công phức tạp như của NightEagle. Việc triển khai các hệ thống như vậy là rất cần thiết để bảo vệ chống lại các tác nhân đe dọa tiên tiến có khả năng hoạt động lén lút và dai dẳng.
