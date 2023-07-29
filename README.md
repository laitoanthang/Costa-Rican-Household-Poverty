<div style="text-align: left; background-color:#E9F7EF; font-family:Arial; color:#526085; padding: 12px; line-height:1.25;border-radius:1px; margin-bottom: 0em; text-align: center; font-size: 58px;border-style: solid;border-color: dark green;">Kaggle Competition: Costa Rican Household Poverty Level Prediction</div>

<div style="border-radius:10px; border:#808080 solid; font-family: Tahoma; padding: 15px; background-color: ##F0E68C ; font-size:100%; text-align:left">

<div class="list-group" id="list-tab" role="tablist">
    <h3 style="text-align: left; background-color: #ACA224; font-family:Tahoma; color: white; padding: 14px; line-height: 1; border-radius:10px"><b>A. INTRODUCTION 📝</b></h3>

### **<h3 align="center"><font>📂 DATA DESCRIPTION:</font></h3>**

🏡 Đây là **challenge** ở trên kaggle được public bởi ngân hàng Inter-American Development.

- Câu hỏi được đặt ra là phân tích bài toán như thế nào?
- Dự đoán mức nghèo của các hộ gia đình ở Costa Rica để xác định hộ gia đình nào có nhu cầu hỗ trợ phúc lợi xã hội.
- Có thể phân cụm hộ gia đình bằng cách xây dựng model hợp lý với độ chính xác tốt nhất.
  Bài nộp sẽ được đánh giá dựa trên điểm số macro F1.

**{train|test}.csv** - tập dữ liệu huấn luyện

- Chia làm 2 tập: Train (có cột Target) and Test (không có cột Target).
- Mỗi dòng đại diện cho một mẫu dữ liệu.
- Nhiều người có thể là một phần của hộ gia đình. Nhưng chỉ sự dự đoán của chủ hộ là được chấm điểm.

**sample_submission.csv** - tập dữ liệu mẫu

**codebook.csv**: Giải thích ý nghĩa các cột của dataset

**Target** Feature: - 1: Cực nghèo - 2: Nghèo vừa - 3: Hộ gia đình có nguy cơ - 4: Hộ gia đình không nghèo

Link dataset: https://www.kaggle.com/competitions/costa-rican-household-poverty-prediction/data

<div class="list-group" id="list-tab" role="tablist">
    <h3 style="text-align: left; background-color: #ACA224; font-family:Tahoma; color: white; padding: 14px; line-height: 1; border-radius:10px"><b>B. DATA EXPLORATION 📝</b></h3>

Ý nghĩa các features có trong [document của cuộc thi](https://www.kaggle.com/c/costa-rican-household-poverty-prediction/data). Nhưng chú ý những điều sau

- Id: một định danh duy nhất cho mỗi cá nhân, điều này không phải là một đặc điểm mà chúng ta sử dụng!
- idhogar: một định danh duy nhất cho mỗi hộ gia đình. Biến số này không phải là một đặc điểm, nhưng sẽ được sử dụng để nhóm các cá nhân theo hộ gia đình vì tất cả các cá nhân trong một hộ gia đình sẽ có cùng định danh.
- parentesco1: cho biết nếu người này là trưởng của hộ gia đình.
- Target: nhãn, mà nên bằng nhau đối với tất cả các thành viên trong một hộ gia đình.
- age: tuổi của người được dự đoán.
- escolari: số năm học của người được dự đoán.
- hogar_nin: số trẻ em trong gia đình dưới 19 tuổi.
- hogar_adul: số người lớn trong gia đình.
- hogar_total: tổng số người trong gia đình.
- rez_esc: số năm học đã bỏ lỡ cho các em học sinh.
- meaneduc: số năm học trung bình của các thành viên trong hộ gia đình.
- dependency: tỷ lệ phụ thuộc của các thành viên trong hộ gia đình vào người đại diện cho hộ gia đình.
- region: vùng của Costa Rica, mỗi vùng có thể có các tính chất địa lý và kinh tế khác nhau.

| Column          | Description                                                                                                                                    |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | --------------- |
| v2a1            | Thanh toán tiền thuê hàng tháng                                                                                                                |
| hacdor          | =1 Quá đông phòng ngủ                                                                                                                          |
| phòng           | số phòng trong nhà                                                                                                                             |
| hacapo          | =1 Quá tải phòng                                                                                                                               |
| v14a            | =1 hộ gia đình có phòng tắm                                                                                                                    |
| tủ lạnh         | =1 nếu hộ gia đình có tủ lạnh                                                                                                                  |
| v18q            | sở hữu một máy tính bảng                                                                                                                       |
| v18q1           | số máy tính bảng hộ gia đình sở hữu                                                                                                            |
| r4h1            | Nam giới dưới 12 tuổi                                                                                                                          |
| r4h2            | Nam từ 12 tuổi trở lên                                                                                                                         |
| r4h3            | Tổng số nam giới trong hộ gia đình                                                                                                             |
| r4m1            | Nữ giới dưới 12 tuổi                                                                                                                           |
| r4m2            | Nữ từ 12 tuổi trở lên                                                                                                                          |
| r4m3            | Tổng số phụ nữ trong hộ gia đình                                                                                                               |
| r4t1            | người dưới 12 tuổi                                                                                                                             |
| r4t2            | người từ 12 tuổi trở lên                                                                                                                       |
| r4t3            | Tổng số người trong hộ gia đình                                                                                                                |
| tamhog          | kích thước của hộ gia đình                                                                                                                     |
| tamviv          | số người sống trong hộ gia đình                                                                                                                |
| escolari        | năm học                                                                                                                                        |
| rez_esc         | Đi học muộn                                                                                                                                    |
| hhsize          | quy mô hộ gia đình                                                                                                                             |
| paredblolad     | =1 nếu vật liệu chiếm ưu thế ở bức tường bên ngoài là khối hoặc gạch                                                                           |
| paredzocalo     | "=1 nếu vật liệu chiếm ưu thế trên tường bên ngoài là ổ cắm (gỗ, kẽm hoặc absbesto"                                                            |
| paredpreb       | =1 nếu vật liệu chiếm ưu thế trên tường bên ngoài là bê tông đúc sẵn hoặc xi măng                                                              |
| pareddes        | =1 nếu vật liệu chiếm ưu thế ở bức tường bên ngoài là vật liệu phế thải                                                                        |
| paredmad        | =1 nếu vật liệu chiếm ưu thế ở bức tường bên ngoài là gỗ                                                                                       |
| paredzinc       | =1 nếu vật liệu chiếm ưu thế ở bức tường bên ngoài là kẽm                                                                                      |
| paredfibras     | =1 nếu vật liệu chiếm ưu thế ở bức tường bên ngoài là sợi tự nhiên                                                                             |
| paredother      | =1 nếu vật liệu chiếm ưu thế trên tường bên ngoài là vật liệu khác                                                                             |
| pisomoscer      | "=1 nếu vật liệu chủ yếu trên sàn là khảm, gốm, đất nung"                                                                                      |
| pisocemento     | =1 nếu vật liệu chủ yếu trên sàn là xi măng                                                                                                    |
| pisoother       | =1 nếu vật liệu chiếm ưu thế trên sàn là vật liệu khác                                                                                         |
| pisonatur       | =1 nếu vật liệu chiếm ưu thế trên sàn là vật liệu tự nhiên                                                                                     |
| pisonotiene     | =1 nếu không có tầng nào trong hộ gia đình                                                                                                     |
| pisomadera      | =1 nếu vật liệu chủ yếu trên sàn là gỗ                                                                                                         |
| techozinc       | =1 nếu vật liệu chủ yếu trên mái nhà là lá kim loại hoặc kẽm                                                                                   |
| techoentrepiso  | "=1 nếu vật liệu chủ yếu trên mái nhà là xi măng sợi, gác lửng "                                                                               |
| techocane       | =1 nếu vật liệu chủ yếu trên mái nhà là sợi tự nhiên                                                                                           |
| techootro       | =1 nếu vật liệu chiếm ưu thế trên mái nhà là vật liệu khác                                                                                     |
| cielorazo       | =1 nếu nhà có trần                                                                                                                             |
| abastaguadentro | =1 nếu cung cấp nước bên trong nhà ở                                                                                                           |
| abastaguafuera  | =1 nếu cung cấp nước bên ngoài nhà ở                                                                                                           |
| abastaguano     | =1 nếu không cung cấp nước                                                                                                                     |
| public          | "=1 điện từ CNFL, ICE, ESPH/JASEC"                                                                                                             |
| planpri         | =1 điện từ nhà máy tư nhân                                                                                                                     |
| noelec          | =1 không có điện trong nhà                                                                                                                     |
| coopele         | =1 điện từ hợp tác xã                                                                                                                          |
| sanitario1      | =1 không có nhà vệ sinh trong nhà                                                                                                              |
| sanitario2      | =1 nhà vệ sinh được kết nối với cống hoặc hố phân                                                                                              |
| sanitario3      | =1 toilet thông với bể phốt                                                                                                                    |
| sanitario5      | =1 nhà vệ sinh kết nối với hố đen hoặc letrine                                                                                                 |
| sanitario6      | =1 nhà vệ sinh được kết nối với hệ thống khác                                                                                                  |
| energcocinar1   | =1 không có nguồn năng lượng chính nào được sử dụng để nấu ăn (không có bếp)                                                                   |
| Energcocinar2   | =1 nguồn năng lượng chính dùng để đun nấu điện                                                                                                 |
| Energcocinar3   | =1 nguồn năng lượng chính được sử dụng để nấu gas                                                                                              |
| energcocinar4   | =1 nguồn năng lượng chính được sử dụng để nấu than gỗ                                                                                          |
| elimbasu1       | =1 nếu đổ rác chủ yếu bằng xe bồn                                                                                                              |
| elimbasu2       | =1 nếu xử lý rác chủ yếu bằng hố thực vật hoặc chôn lấp                                                                                        |
| elimbasu3       | =1 nếu xử lý rác chủ yếu bằng cách đốt                                                                                                         |
| elimbasu4       | =1 nếu xử lý rác chủ yếu bằng cách ném vào một không gian trống                                                                                |
| elimbasu5       | "=1 nếu xử lý rác chủ yếu bằng cách ném xuống sông                                                                                             | lạch hoặc biển" |
| elimbasu6       | =1 nếu xử lý rác chủ yếu khác                                                                                                                  |
| epared1         | =1 nếu tường xấu                                                                                                                               |
| epared2         | =1 nếu tường đều                                                                                                                               |
| epared3         | =1 nếu tường tốt                                                                                                                               |
| echo1           | =1 nếu mái xấu                                                                                                                                 |
| echo2           | =1 nếu mái đều                                                                                                                                 |
| echo3           | =1 nếu mái tốt                                                                                                                                 |
| eviv1           | =1 nếu sàn xấu                                                                                                                                 |
| eviv2           | =1 nếu tầng bình thường                                                                                                                        |
| eviv3           | =1 nếu sàn tốt                                                                                                                                 |
| dis             | =1 nếu vô hiệu hóa người                                                                                                                       |
| nam             | =1 nếu là nam                                                                                                                                  |
| nữ              | =1 nếu là nữ                                                                                                                                   |
| estadocivil1    | =1 nếu dưới 10 tuổi                                                                                                                            |
| estadocivil2    | = 1 nếu uunion tự do hoặc kết hợp                                                                                                              |
| estadocivil3    | =1 nếu đã kết hôn                                                                                                                              |
| estadocivil4    | =1 nếu ly dị                                                                                                                                   |
| estadocivil5    | =1 nếu tách biệt                                                                                                                               |
| estadocivil6    | =1 nếu goá/nhi                                                                                                                                 |
| estadocivil7    | = 1 nếu độc thân                                                                                                                               |
| parentesco1     | =1 nếu chủ hộ                                                                                                                                  |
| parentesco2     | =1 nếu vợ/chồng/bạn đời                                                                                                                        |
| parentesco3     | = 1 nếu con trai/con cưng                                                                                                                      |
| parentesco4     | =1 nếu con riêng/con nuôi                                                                                                                      |
| parentesco5     | =1 nếu con rể/con rể                                                                                                                           |
| parentesco6     | =1 nếu cháu trai/người nuôi nấng                                                                                                               |
| parentesco7     | =1 nếu mẹ/bố                                                                                                                                   |
| parentesco8     | =1 nếu bố/mẹ chồng                                                                                                                             |
| parentesco9     | =1 nếu anh/chị                                                                                                                                 |
| parentesco10    | =1 nếu anh/chị dâu                                                                                                                             |
| parentesco11    | =1 nếu thành viên khác trong gia đình                                                                                                          |
| parentesco12    | =1 nếu không phải thành viên gia đình khác                                                                                                     |
| idhogar         | Định danh cấp hộ gia đình                                                                                                                      |
| hogar_nin       | Số trẻ em từ 0 đến 19 tuổi trong hộ gia đình                                                                                                   |
| hogar_adul      | Số người lớn trong hộ gia đình                                                                                                                 |
| hogar_mayor     | # người trên 65 tuổi trong hộ gia đình                                                                                                         |
| hogar_total     | # trong tổng số cá nhân trong hộ gia đình                                                                                                      |
| người phụ thuộc | Tỷ lệ phụ thuộc, được tính = (số thành viên trong hộ gia đình dưới 19 tuổi hoặc trên 64 tuổi)/(số thành viên trong gia đình từ 19 đến 64 tuổi) |
| edjefe          | số năm học của chủ hộ là nam giới, dựa trên sự tương tác của escolari (số năm học), chủ hộ và giới tính, có=1 và không=0                       |
| edjefa          | số năm học của chủ hộ là nữ, dựa trên sự tương tác của esco                                                                                    |

<div class="list-group" id="list-tab" role="tablist">
    <h3 style="text-align: left; background-color: #ACA224; font-family:Tahoma; color: white; padding: 14px; line-height: 1; border-radius:10px"><b>C. DATA PREPROCESSING 📝</b></h3>

https://github.com/laitoanthang/Costa-Rican-Household-Poverty/blob/main/datapreprocessing.ipynb
