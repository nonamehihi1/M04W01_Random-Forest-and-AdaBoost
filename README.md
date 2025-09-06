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
