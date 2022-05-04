# Driver for FT4232 

## Introduction 
 - Có 2 loại driver cho các loại USB-TTL của dòng FTDI là VCP và D2XX Drivers. D2XX được dùng để truy cập vào firmware của FT4232 nên hiện tại sẽ không sử dụng driver này. VCP được dùng cho máy tính nhận biết các loại USB FT là một hay nhiều cổng COM ảo để có thể thực hiện giao tiếp serial.
 - Phiên bản source code này được xây dựng cho các dòng sản phẩm của Tecknique với kernel v5 và v4.
 
## How to install driver VCP to SoM hoặc uSoM
  - STEP 1: Tải thư mục theo dạng .zip trên github theo đường dẫn https://github.com/gremsy-software-team/ftdi_driver.git, giải nén và đặt vào thư mục "oclea-sdk-1.x.x/oclea-sdk/tsdk-kernel-modules". 
  Note: Ưu tiên các phiên bản mới của Oclea-SDK.
  - STEP 2: Ra ngoài Oclea-sdk để tiến hành build chương trình.
  	sudo ./build_app.sh -p s5l
  - STEP 3(optional) : Để đảm bảo tiến hành cài đặt thành công vô trong thư mục platform/{device_name}/fakeroot/lib/module/{kernel_version}/extra và tìm file ftdio_sio.ko
  - STEP 4: Sau khi build thành công tiến hành tạo ảnh
  	sudo ./build_ota.sh -p s5l --dts platform/s5l/dts/oclea_s5l_breakout_evk.dts
  Kết nối vào cổng USB device của SoM và tiến hành nạp mạch
  	sudo oclea-flash -o output/s5l/upgrade.7z -s setting.json
  _ STEP 5 (optional): Đối với cv25 hoặc các dòng SoM có thiết lập usb mặc định là device, để chuyển về dạng host sử dụng cú pháp .  
  	echo HOST > /proc/ambarella/usbphy0
  Test cắm FT4232 vào cổng USB của S5L và kiếm tra S5L có phát hiện được các cổng của FT4232 hay không.
  
## Check connection of FT4232 
 - search các cổng tty kết nối với s5l 
	![Test Image 1](https://github.com/gremsy-software-team/ftdi_driver/blob/develop/ftdi_sio/doc/image.png)
 - Nếu hiển thị đầy đủ 4 thiết bị tty -> cài đặt thành công
 
  
  
  
