Logical-Volume-Manager--LVM-
============================

<h2>Tổng quan Logical Volume Manager (LVM)</h2>
<h4>I.  Giới Thiệu</h4>
<p> LVM là phương pháp cho phép ấn định không gian đĩa cứng thành những logical Volume khiến cho việc thay đổi kích thước trở lên dễ dàng hơn (so với partition). Với kỹ thuật Logical Volume Manager (LVM) bạn có thể thay đổi kích thước mà không cần phải sửa lại table của OS. Điều này thật hữu ich với những trường hợp bạn đã sử dụng hết phần bộ nhớ  còn trống của partition và muốn mở rộng dung lượng của nó</p>
<h4>II. Các thành phần LVM <h4>
<p>Trong phần này ta sẽ giới thiệu về các thành phần trong LVM <p>
<ul>
  <li><b>Physical volumes :</b> là những đĩa vật lý hoặc phân vùng đĩa của bạn chẳng hạn như /dev/hda hoặc /dev/hdb1. Khi bạn muốn sử dụng chỉ cần mount vào thôi. Đối với việc sử dụng LVM chúng ta có thể kết hợp nhiều physical volumes thành volume groups</li>
  <li><b>Volume groups :</b> là một nhóm bao gồm các physycal volumes thực và dung lượng này được sử dụng để tạo ra các logical volumes, trong đó bạn có thể làm được những điều như sau : tạo, thay đổi kích thước, gỡ bỏ và sử dụng. Bạn có thể xem volume group như 1 “phân vùng ảo”</li>
  <li><b>Logical volumes  :</b> là những volumes cuối cùng sau khi mount vào hệ thống của mình, bạn có thể thêm vào, gỡ bỏ và thay đổi kích thước một cách nhanh chóng. Kể từ khi chúng chứa trong các volume group bạn có thể làm cho nó lơn hơn bất kỳ physical volume đơn lẻ mà bạn muốn. (ví dụ bạn có 4 ổ đĩa mỗi ổ 5GB khi bạn kết hợp nó lại thành 1 volume group 20GB, và bạn có thể tạo ra 2 logical volumes mỗi disk 10GB)</li>
</ul>
