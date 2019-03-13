# Phân biệt Convolution và Correlation
Giả sử có một Input và Filter cho CNN như sau:

<p align='center'>
    <img src="./images/1.jpeg">
</p>

Correlation được tính như sau:

<p align='center'>
    <img src="./images/2.jpeg">
</p>

Cách tính Convolution cũng giống như correlation với việc xoay filter 180 độ và tính toán giống như correlation với Filter xoay được:

<p align='center'>
    <img src="./images/11.jpeg">
</p>

# Convolution Forward và Backward
**_Chú ý_: Để đơn giản hoá quá trình tính toán của Input và Filter cho việc tính đạo hàm sau này, ta coi như phép toán convolution là giống correlation**

Tích chập sẽ được tính như sau:

<p align='center'>
    <img src="./images/10.jpeg">
</p>

Quá trình tính toán có thể được thể hiện như hình dưới:

<p align='center'>
    <img src="./images/3.jpeg">
</p>

## Tính đạo hàm của loss function theo F

<p align='center'>
    <img src="./images/4.png">
</p>

và bằng:

<p align='center'>
    <img src="./images/5.png">
</p>

Ta có thể viết lại đạo hàm theo F của hàm lỗi bằng tích chập như sau:

<p align='center'>
    <img src="./images/6.jpeg">
</p>

## Tính đạo hàm của loss function theo X
Đạo hàm theo X được tính như sau:

<p align='center'>
    <img src="./images/7.png">
</p>

Công thức tính đạo hàm ở trên có thể được viết lại bằng một operation gọi là Full_Convolution. Để tính được đạo hàm theo X, ta cần phải quay Filter 180 độ và tính Full_Convolution của filter đã xoay bằng chain rule. Được tính toán như sau:

<p align='center'>
    <img src="./images/8.jpeg">
</p>

Cụ thể:

<p align='center'>
    <img src="./images/9.jpeg">
</p>