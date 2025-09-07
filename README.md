# M04W01_Random-Forest-and-AdaBoost
Random Forest và AdaBoost

Trong học máy, một mô hình đơn lẻ thường phải đánh đổi giữa thiên lệch (bias) và phương sai (variance). Mô hình phức tạp có thể khớp dữ liệu huấn luyện tốt (thiên lệch nhỏ) nhưng dễ dao động trước mẫu mới (phương sai lớn); mô hình đơn giản thì ngược lại. Ensemble Learning đề xuất cách khác là tổng hợp các mô hình yếu vào thành một kết quả tốt hơn

Sử dụng Ensemble Learning để kết hợp tất cả các cây lại:
+ Homogenous Approach : Bagging, Boosting
+ Hetetogeneous Approach: Stacking

I. Random Forest
Là nhiều cây Quyết định, thay vì 1 cây quyết định thì sẽ sử dụng nhiều cây để quyết định kết quả, giúp giảm under, overfit của 1 cây quyết định

Ý tưởng: Tạo ra nhiều dataset dựa trên data gốc, sau đó xây dựng cây quyết định 1 cách ngẫu nhiên dựa trên dataset mới, sử dụng 2 lớp tính ngẫu nhiên là: 
+ Random Sampling: Mỗi cây được huấn luyện trên một mẫu boostrap khác nhau
+ Random Feature Selection: Tại mỗi node của cây, chỉ xem xét một tập con ngẫu nhiên các thuộc tính

Ví dụ : dữ liệu có 5 trường, thì sẽ lấy k trường ngẫu nhiêu, sau đó sử dụng các thuật toán tính Gini, IG để chọn nút gốc, sau đó tương tự, lại chọn (5- k) trường còn lại để điền nốt vào cây
Sau đó đưa tất cả output của từng cây để vote xem là kết quả nào


II. AdaBoost

AdaBooost có phương pháp học hoàn toàn khác: học từ sai lầm. Thay vì học các mô hình độc lập, AdaBoost xây dựng một chuỗi các weeklearner, mỗi learner được thiết kế để sửa chữa những sai lầm của các learner trước đó

<img width="723" height="305" alt="image" src="https://github.com/user-attachments/assets/c5e3977a-9b6d-422f-879a-4d98762d83b9" />

1. Ý tưởng: Mỗi vòng lặp, AdaBoost tập trung nhiều hơn vào những mẫu dữ liệu mà mô hình trước đó phân loại sai. Sau nhiều vòng, các mô hình yếu được gán trọng số và cuối cùng kết hợp thành một mô hình mạnh

2. Ví dụ:
Giả sử ta có tập dataset như này:
<img width="552" height="281" alt="image" src="https://github.com/user-attachments/assets/033b22d1-f3fb-4c75-8a76-8db528292cb7" />

Với sample weight ban đầu sẽ là đều hết nên = 1/8. Sau đó ta sẽ thử tính Gini cho từng thuộc tính để tìm Heart Diease:
+ Chest Pain = 5/8 *(1- (3/5)^2 - (2/5)^2) + 3/8 * (1 - (1/3)^2 - (2/3)^2 ) = 0.57
+ Blocked Arteries = 6/8 * (1 - (3/6)^2 - (3/6)^2) + 2/8 * (1 - (1/2)^2 - (1/2)^2) = 0.5
+ Tương tự với Patient Weight(>170) = 0.375

Sử dụng công thức Amount of Say = 1/2 * log((1 - Total Error) / Total Error ). Để biết với mỗi Stumpt như vậy sẽ đóng góp như thế nào.
Sau đó tính Amount of Say cho mỗi thuộc tính:
+ Chest Pain: giả sử trường hợp dự đoán sai 2 trường hợp = 1/2 * log((1 - 3/8) / (3/8)) = 0.25
+ Blocked Arteries : Sai 4 trường hợp = 0
+ .....

Sau đó chúng ta sẽ cải thiện bằng cách tạo thêm dataset mới, sao cho sẽ duplicate các trường hợp sai nhiều hơn

Solution 1: label (-1, 1): Tăng Sample weight cho các Incorrectly classified và Giảm cho các Correctly classified
Solution 2: Label (0, 1): Tăng Sample weight cho các Incorrectly classified và giữ nguyên Correctly 

Công thức của Sample Weight(mới) = Sample Weight(cũ) * e^(Amount of say * y * y_pred)
+ Trong công thức có y, y_pred để thể hiện nếu y và y_pred khác nhau thì Sample Weight của trường hợp đó sẽ lớn hơn, tương tự nếu y giống y_pred thì Sample Weight sẽ nhỏ hơn

<img width="900" height="409" alt="image" src="https://github.com/user-attachments/assets/59c79b29-1baa-4b65-93e8-d8afbf1452da" />
Như ảnh trên thì ta có thể thấy sau khi update Sample Weight thì những trường hợp sai sẽ có Weight lớn hơn

Tiếp tục sau khi đã update xong thì ta sẽ random để tạo ra dataset mới có chứa nhiều Incorrectly Classified
<img width="933" height="458" alt="image" src="https://github.com/user-attachments/assets/0c7376c1-2ca4-47b1-acb7-411d6f28d2c1" />

Sau đó ta sẽ tiếp tục tính stump cho từng thuộc tính giống như ở ví dụ, tiếp tục sau đó lại cập nhật Sample Weight..... Tiếp tục cho đến khi nào muốn dừng thì thôi
