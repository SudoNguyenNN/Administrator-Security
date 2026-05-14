## Tạo NAT trên Pfsense

### 1. Yêu Cầu

- Đã có Interface Wan và Lan
- Route default chuyển về Gateway WAN

### 2. Tạo Nat Manual trên Pfsense

<img width="1153" height="546" alt="image" src="https://github.com/user-attachments/assets/407ccc9c-2153-443a-a4eb-924b879795b7" />

<img width="1147" height="496" alt="image" src="https://github.com/user-attachments/assets/87c36415-9a2d-41f2-a8da-fda5247a922e" />

Xong tạo thêm một rules mới trên cùng

<img width="1147" height="946" alt="image" src="https://github.com/user-attachments/assets/a84b8241-57d1-459d-b5d5-c2d042e31fbf" />

 #### Chỉ nên sửa phần source : để các IP Private bên bạn

Lưu lại và test kết quả ping từ private sang mạng public

<img width="1155" height="816" alt="image" src="https://github.com/user-attachments/assets/9d508a49-56e1-403c-8dd1-dad5a2da9534" />

Kết quả ping được từ mạng Lan sang bên ngoài internet => OK.
