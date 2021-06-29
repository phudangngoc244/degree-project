# Mục lục

1. [Giới thiệu](#giới-thiệu)
    * [Các chức năng chính](#các-chức-năng-chính)
    * [Sơ đồ tổ chức hệ thống](#sơ-đồ-tổ-chức-hệ-thống)
    * [Framework](#framework)
2. [Chạy ứng dụng](#run-application)
    * [Cài đặt docker, docker-compose](#docker-compose)
    * [Chạy ứng dụng trên docker-compose](#run-with-docker-compose)
    * [Khởi tạo user admin](#init-user-admin)
    * [ Tạo người dùng có quyền thêm danh sách bằng](#init-create-degree-user)
    * [Thêm mới và xem bằng](#init-degree-and-viewing)
    * [Tắt và bật ứng dụng](#turn-on-off-application)
3. [Giới thiệu về blockchain](#gioi-thieu-ve-blockchain)
    * [Điểm mạnh](#diem-manh)
    * [Điểm yếu](#diem-yeu)
    * [Mô hình vận hành](#mo-hinh-van-hanh)
    * [Nền tảng block](#nen-tang-block)
    * [Chuỗi kết nối blockchain](#chuoi-ket-noi-blockchain)
    * [Smart Contract và ứng dụng](#smart-contract-va-ung-dung)
4. [Giới thiệu Bigchaindb lưu trữ dữ liệu dựa trên công nghệ blockchain](#gioi-thieu-ve-bigchaindb)
    * [Xây dựng trên công nghệ blockchain](#xay-dung-tren-cong-nghe-blockchain)
    * [Kiến trúc](#kien-truc)
    * [Mô hình dữ liệu](#mo-hinh-du-lieu)
    * [Mã hóa](#ma-hoa)
    * [Công cụ phát triển](#cong-cu-phat-trien)

<a name="gioi-thieu"></name>

## Giới thiệu
Project lưu trữ bằng điện tử dựa trên công nghệ blockchain. Với ưu điểm có thể lưu dữ liệu lâu dài (hơn 50 năm) và tránh được hiển tượng dữ liệu bị sửa đổi bởi một người tấn công vào hệ thống mạng.

<a name="cac-chuc-nang-chinh"></name>

### Các chức năng chính
* Xem bằng đại học bằng hình ảnh minh họa
* Tạo mới một thông tin bằng đại học
* Danh sách sinh viên tốt nghiệp và bằng tương ứng
* Thêm một người dùng mới
* Sửa thông tin người dùng hiện tại
* Đăng nhập, đăng xuất trong hệ thống

<a name="so-do-to-chuc-he-thong"></name>

### Sơ đồ tổ chức hệ thống

![](./img/project_architecture.png?raw=true)

* Client (sẽ kết nối với BigchianDB server và Django server): xử lý các nghiệp vụ của chương trình, truy xuất và hiển thị thông tin.
* BigChainDB server (sẽ kết nối với Client và MogoDB server): xử lý các vấn đề về bảo mật, xác thực thông tin, chuyển đổi thông tin.
* MongoDB server (sẽ kết nối với BigChainDB server): Lưu trữ dữ liệu, các thông tin không yêu cầu tính riêng tư.
* Django server (sẽ kết nối với Client): Xử lý nhận yêu cầu và gửi trả các thông tin cần thiết, lưu trữ dữ liệu, các thông tin yêu cầu tính 
riêng tư cao.

<a name="framework"></name>

## Framework
* Ứng dụng được xây dựng trên nền tảng client Reactjs và Database server BigchianDB
* Dùng thư viện react-table để thực hiện tìm kiếm lấy dữ liệu và hiển thị danh sách thông tin bằng đại học cần thiết
* Ứng dụng Django server để lưu các thông tin bảo mật như thông tin tài khoản và mật khẩu của người dùng. Do BigchainDB sự cho phép bất kỳ ai cũng có thể đọc dữ liệu nên cần phải sử dụng server riêng đối với các dữ liệu riêng tư

<a name="run-application"></name>

## Chạy ứng dụng

<a name="docker-compose"></name>

### Cài đặt docker, docker-compose
Cài đặt docker theo đường dẫn [Docker-compose installed](https://docs.docker.com/compose/install/) sau đó chuyển tới bước tiếp theo.

<a name="run-with-docker-compos"></name>

### Chạy ứng dụng trên docker-compose
```
    docker-compose up -d
```

<a name="init-user-admin"></name>

### Khởi tạo user admin
Hệ thống cần một user admin để quản lý, tạo mới người dùng. Admin user sẽ được tạo trên server django - (máy chủ tập trung, lưu giữ thông tin đăng nhập người dùng, hiện đang chạy như là một container trên docker). 

Trước tiên liệt kê ra danh sách container và tìm id của django container. (Chú ý tên django container sẽ có đuôi là degreeserver)
```
    docker container ls --all
```
![](./img/container-list.png?raw=true)

Sau khi có được thông tin django container tiếp tục gõ câu lệnh sau để tạo mới admin user. Trong đó `container_id` sẽ tương ứng django container id.

```
    docker exec -it container_id python manage.py createsuperuser
```

Nhập username, password theo yêu cầu của phần mềm, khi kết thúc sẽ có message thông báo username và password.

<a name="init-create-degree-user"></name>

### Tạo người dùng có quyền thêm danh sách bằng
Truy cập [http://localhost:3000/](http://localhost:3000/) -> login -> nhập username, password của admin user -> chọn tạo mới người dùng -> nhập đầy đủ thông tin người dùng -> bấm Create -> bấm logout

<a name="init-degree-and-viewing"></name>

### Thêm mới và xem bằng
Sau khi tạo mới người dùng -> chọn login -> nhập user, password của người dùng mới tạo -> chọn tạo mới bằng -> nhập thông tin bằng -> bấm Create

Sau đó bấm vào biểu tượng HUST Degree góc trên bên trái -> nhập mã số sinh viên -> Danh sách bằng sẽ được hiển thị ra bên dưới

<a name="turn-on-off-application"></name>

### Tắt và bật ứng dụng
Bật ứng dụng
```
    docker-compose start -d
```

Tắt ứng dụng
```
    docker-compose stop
```

<a name="gioi-thieu-ve-blockchain"></a>

## Giới thiệu về blockchain
Đối với chúng ta blockchain đang là một từ khóa khá mới và đặc biệt. Từ các báo đài, các phương tiện truyền thông đang ngày ngày
nói về chủ đề blockchain. Một ứng dụng đặc biệt của blockchain là bitcoin, đang là làn sóng trong công cuộc sử dụng đồng tiền ảo ứng
dụng trong cuộc sống. Chỉ vậy thôi cũng đã rất có sức hút tìm hiểu về công nghệ blockchain.

<a name="diem-manh"></a>

### Điểm mạnh
1. Không phụ thuộc vào trung gian thứ 3

    Ngày nay các ngân hàng đang rất phát triển, đi cùng với đó là các chính sách khiến chúng ta lệ thuộc. Chúng ta tin tưởng ngân hàng là bên trung gian thứ 3 để lưu trữ tiền và thực hiện các giao dịch thanh toán, chuyển khoản, mua hàng. Nhưng khi sự phụ thuộc quá lớn bạn sẽ thấy một số điểm không tốt.

    * Phụ thuộc vào ngân hàng, chính sách, các phí phụ thu vô lý.
    * Số tiền có thể bị mất khi ngân hàng phá sản.
    * Thông tin có thể bị rò rỉ nếu bảo mật ở ngân hàng không tốt.
    * Có thể xảy đến vần đề ký thuật trong một số trường hợp, khiến tiền gửi đi rồi nhưng vẫn còn trong ví người gửi.

    Các vấn đề nêu trên chỉ là một phần các yếu điềm khi phụ thuộc bên thứ 3. Nhưng giờ đây các vấn đề đó có thể khắc phục khi sử dụng blockchain. Blockchain quy định rõ chúng ta không cần lưu trữ tiền, dữ liệu trên bất kỳ một nền tảng nào khác như ngân hàng hay các dịch vụ lưu trữ điện tóan google drive..., hay bất kỳ nơi nào đó. Tiền của bạn, dữ liệu của bạn (gọi chung là asset) thì chỉ có bạn có thể sử dụng và đọc được. Tiền sẽ được trực tiếp chuyển từ ví của người gửi tới ví của người nhận mà không có sự tham gia của bất kỳ một bên thứ 3 nào. Điều này tạo tiền đề cho sự tin tưởng, và truy xuất các giao dịch trong trường hợp thống kê hoặc kiểm tra lại.

2. Sẽ không có hiện tượng Double Spent

    Double Spent là hiện tưởng khi bạn gửi tiền cho một ai đó và số tiền đó vẫn còn trong túi của bạn. Điều này thực sự quá vô lý. Nguyên nhân là khi chúng ta sử dụng ngân hàng là nơi đề lưu trữ tiền và chuyển khoảng, một giao dịch sẽ được chuyển đến ngân hàng và thực hiện ngay chính bên trong ngân hàng đó. Và quy trình thực hiện chỉ đơn giản là họ lấy tiền từ tài khoản người A trong dữ liệu và chuyển đến tài khoảng người B ngay chính bên trong ngân hàng đó. Điều này khiến tiền chỉ đơn giản là dữ liệu, hay chỉ là một thứ gì đó tồn tại trên máy tính và không thực sự thực tế. Và như vậy vấn đề sẽ dễ phát sinh khi hệ thống của họ có vấn đề. Dẫn đến có thể họ thông báo tiền đã chuyển đi nhưng thực tế vẫn còn trong túi của bạn.

    Blockchain khắc phục điều này bằng cách thực hiện với một vài bằng chứng như proof of work (bằng chứng làm việc). Nhưng đề ngắn gọn mình sẽ nói vắn tắt. Blockchain có cơ chế khi thực hiện một giao dịch các thông tin, trình tự làm việc, kết quả đều được thực hiện một cách tường minh có giám sát. Như vậy khi thực hiện xong mọi thứ đều rất tường minh và sẽ không có hiện tượng chuyển tiền rồi mà vẫn còn trong túi.

3. Bảo mật

    Blockchain sử dụng cơ chế mã hóa khiến nội dung không thể bị đọc đuọc trong trường hợp có người đánh cắp. Và để khẳng định cho điều đó đã một báo cáo phân tích rằng cần tới 50 năm thì người ta mới có thể giải mã được số thông tin bị mã hóa đó.

4. Tồn tại song song với internet

    Bộ máy vận hành Blockchain sẽ luôn luôn tồn tại. Do cơ chế không phụ thuộc dữ liệu lưu trữ phân tán nên ai cũng có thể tạo ra một hệ thống vận hành blockchain. Blockchain chỉ biến mất khi internet biến mất.

5. Xây dựng nên sự tin tưởng

    Vấn đề này sẽ được giải thích khi tìm hiểu về smartcontract và ứng dụng của nó. Nói sơ qua thì smartcontract sẽ là một hợp đồng số có các điều khoản mọi người có thể tin tưởng được có thể lưu trữ tiền tệ điện tử và chỉ hoạt động dựa theo những gì đã được lập trình sẵn và ai cũng có thể đọc được nó, không có bất kỳ bí mật gì được che dấu bên trong smartcontract.

<a name="diem-yeu"></a>

### Điểm yếu
Đã có những bài báo phân tích rất rõ điểm yếu của blockchain và mình xin được trính xuất một bài rất hay của tạp chí techtalk. Các bạn click vào [đây](https://techtalk.vn/nhuoc-diem-cua-blockchain.html) để tìm hiểu thêm.

<a name="mo-hinh-van-hanh"></a>

### Mô hình vận hành
![](img/Ethereum_network.png?raw=true)

đây là một mô hình tiêu biểu của etheneum, cũng là một mô hình tổng quan của blockchain (do ethereum được xây dựng dựa trên công nghệ blockchain). Ethereum xây dựng trên công nghệ blockchain. Và được xây dựng dựa trên các node mạng kết nối với nhau. Node ở đây có thể hiểu đơn giản là một máy tính cá nhân, máy tính xách tay, hoặc là máy để bàn. Nhờ mô hình mạng kết nối này dữ liệu có thể lưu trữ ở bất kỳ node nào trong mạng và sẽ hạn chế được sự mất mát khi có một node trong hệ thống bị hỏng và mất dữ liệu. Tất nhiên dữ liệu sẽ được mã hóa để người không có thẩm quyền không thể đọc được và nhờ như vậy dữ liệu sẽ luôn an toàn.

<a name="nen-tang-block"></a>

### Nền tảng block
![](img/Ethereum_transaction.png?raw=true)

Bitcoin là một đồng tiền ảo. Và để có thể trao đổi các đồng tiền ảo đó chúng ta cần phải thực hiện các giao dịch. Các giao dịch trên nền tảng blockchain sẽ được lưu lại và mã hóa một cách cẩn thận, đó là lý do vì sao ứng dụng blockchain vào các hệ thống tài chính rất được khuyến khích. Có một khái niệm để nói về bản lưu thông tin một giao dịch trên blockchain đó là transaction.

Ở đây hãy nói về transaction trên mạng Ethereum. Với mỗi transaction sẽ có các thông tin chủ yếu như là địa chỉ người nhận, số tiền (đơn vị tiền ảo), địa chỉ người gửi, và một số phần đặc biệt của transaction, chúng ta sẽ tìm hiểu sau. Vậy Làm sao để lưu trữ các transaction? Khối block sẽ lưu giữ các transaction.

![](img/Ethereum_block.png?raw=true)

Transaction sau khi được tạo ra sẽ gửi tới các node khác sau đó sẽ được lưu trữ vào các khối block trong các node. Block lưu trữ các transaction như là lưu trữ các thông tin giao dịch. Như vậy block cũng như là nơi lưu trữ các dữ liệu của hệ thống.

<a name="chuoi-ket-noi-blockchain"></a>

### Chuỗi kết nối blockchain
![](img/Ethereum_blockchain.png?raw=true)

Blockchain có nghĩa là các khối block liên kết lại với nhau. Khi các khối block liên kết lại như vậy blockchain là một bản hoàn chỉnh bao gồm đầy đủ các dữ liệu. Mỗi node sẽ có một bản sao của blockchain. Như vậy khi có một vấn đề gì xảy ra khiến mất mát dữ liệu ở một node nào đó, sẽ dễ dàng để phục hồi dữ liệu.

<a name="smart-contract-va-ung-dung"></a>

### Smart Contact và ứng dụng
Smart Contract là một ứng dụng hay của công nghệ blockchain. Khi mà việc kiểm soát dòng lưu hành đồng tiền cực kỳ khó khăn. Smart Contract như là một chíếc ví điện tử giữ tiền và làm những hành động được lập trình trước và không theo bất kỳ sự điều khiển của ai. Mọi hành động, mã nguồn của nó đều được công bố. Ai cũng có thể xem mã và biết cách hoạt động từ đó. Smart Contract là ứng dụng hiệu quả để mọi người có thể trao tiền của mình vào đó và an tâm rằng không có một hành động nào ảnh hưởng tới đồng tiền của mình mà mình không biết. Dưới đây là một ví dụ về tính ứng dụng của Smart Contract.

![](img/Ethereum_smartcontract_1.png?raw=true)

Ở trên là mô hình quyên góp tiền cho một start-up. Một start-up với các ý tưởng về làm sản phẩm đang kêu gọi các sự đầu tư từ cộng đồng. Cộng đồng quyên góp tiền vào start-up và hy vọng khoản đầu tư mình sẽ sinh lãi khi start-up thành công. Nhưng người đầu tư có thực sự biết được tiền mình đóng góp có được sử dụng đúng cách hay không?

![](img/Ethereum_smartcontract_2.png?raw=true)

Nếu như người kêu gọi thực sự có ý tưởng các khoản tiền sẽ được sử dụng trong mục đích làm ra sản phẩm. Nhưng nếu như nghược lại, người kêu gọi tiền thực sự không có ý tưởng về làm ra sản phẩm mà chỉ là giả mạo.

![](img/Ethereum_smartcontract_3.png?raw=true)

Người kêu gọi không sử dụng tiền vào mục đích chính xác và như vậy các nhà đầu tư sẽ mất tin tưởng cũng như ca thán về sư mất mát tiền của họ.
Smart Contract có thể khác phục được vấn đề trên khi người có ý tưởng sử dụng Smart Contract là nơi giữ tiền, nhận tiền đóng góp, và chuyển tiền cho nhà đầu tư.

![](img/Ethereum_smartcontract_4.png?raw=true)

Mỗi khi có người đóng góp vào quỹ, khoản tiền sẽ được chuyển tới Smart Contract và lưu giữ địa chỉ người đóng góp. Khi người quản lý muốn sử dụng khoản tiền cho mục đích nào đó thì cần phải ghi đầy đủ các thông tin thiết yếu như là người nhận tiền, bao nhiêu tiền, lý do. Rồi sau đó bản ghi sẽ được gửi về cho từng người đã đóng góp tiền, nếu như họ chấp thuận thì khoản tiền mới được chuyển đi còn nếu không thì khoản tiền vẫn ở trong Smart Contract. Như vậy việc sử dụng nguồn tiền sẽ luôn minh bạch và tốt cho cả 2 bên để tin tưởng nhau.

<a name="gioi-thieu-ve-bigchaindb"></a>

## Giới thiệu về Bigchaindb lưu trữ dữ liệu dựa trên công nghệ blockchain

<a name="xay-dung-tren-cong-nghe-blockchain"></a>

### Xây dựng trên công nghệ blockchain
1. Giới thiệu
    * BigchainDB là một cơ sở dữ liệu blockchain có thể mở rộng, phân cấp, không thể thay đổi được đối tượng và sở hữu cá nhân.
    * BigChainDB cho phép triển khai các ứng dụng quy mô lớn trong nhiều trường hợp sử dụng và các nghành công nghiệp từ sở hữu trí tuệ, định danh đến các chuỗi cung ứng, IoT và trí tuệ nhân tạo. 
    * BigchainDB cung cấp giải pháp độc nhất cho các nhà phát triển, các dự án khởi nghiệp và các doanh nghiệp để xây dựng thành công các khái niệm, nền tảng và các ứng dụng mơ ước.
    
2. Ưu điểm
    BigChainDB do xây dựng trên nền tảng của blockchain và xử dụng như là hệ quản trị cơ sở dữ liệu phân tán. Khi bitcoin trở nên nổi bật trên thị trường nó cũng mang những ưu điểm nổi trội. BigChainDB sở hữu tất cả các điểm nổi bật giữa bitcoin và hệ cơ sở dữ liệu phân tán

    ![](img/bigchaindb_adventage.png?raw=true)

    * BigChainDB sử dụng cơ chế Decentralization để có thể giao tiếp với các node trong mạng hệ thống, đối với Decentralization hệ thống sẽ chia ra quản lý bởi nhiều node khác nhau, mỗi node sẽ dữ một bản sao dữ liệu chung. Khi có bất kì sự cập nhật dữ liệu ở một node nào đó. Các dữ liệu sẽ được gửi tới các node khác và đồng bộ. Do đó khả năng mất mát dữ liệu cũng được giảm đi đáng kể.

    * Khả năng chịu lỗi Fault Tolerance được xem như một ưu điểm của Blockchain. BigchainDB kế thừa khả năng này để nâng cấp hệ thống ngăn ngừa các lỗi xảy.

    * Immutability là khả năng không thể bị thay đổi. Khi blockchain sử dụng tính bất biến để bảo tồn dữ liệu tránh đi khả năng thay đổi bởi yếu tố bên ngoài. Nó đã thực sự rất hữu dụng cho các ứng dụng tài chính khi các dữ liệu cần phải thực chính xác.

    * Khi nói về khả năng sở hữu, mọi người có thể sở hữa tài sản, tài liệu của riêng họ, nhưng nếu như có ai đó muốn đánh cắp tài liệu của bạn. Họ đơn giản chỉ cần lấy đi quyền sở hữu tài liệu đó. BigchainDB xác thực quyền sở hữu bằng các mã hóa phức tạp và khó bị thay đổi, nên dường như tính bảo mật rất cao và rất hữu dụng. Ngoài ra đối với quyền sở hữu còn cho khả năng trao đổi nó cho một ai đó khác. Điều đó phù hợp với các ứng dụng mua bán khi mà các giao dịch bán sản phẩm quyền sở hữu sẽ chuyển từ người sở hữu tạm thời đến tay người sở hữu tương lai.

    * Khả năng thực hiện các lệnh lưu trữ chuyển đổi nhanh chóng. Khi các dữ liệu có thể nhanh chóng được lưu trữ. Mọi giao dịch chuyển đổi thực hiện rất nhanh.

    * Khả năng thực hiện một cách nhanh chóng với độ trễ thấp, không mất quá nhiều thời gian cho các chức năng. Kế thừa được tính ưu điểm về tốc độ trong hệ quản trị cơ sở dữ liệu truyền thống. BigchainDB có thể trở nên phù hợp cho các dữ liệu lớn cần lưu trữ và truy xuất thông tin một cách nhanh chóng.

    * Truy xuất dữ liệu nhanh chóng khi xử dụng hệ quản trị cơ sở dữ liệu mongoDB. Giảm tối đa thời gian chờ khi thực hiện các truy vấn cơ sở dữ liệu.

    Nếu đo Bitcoin blockchain bằng các tiêu chí CSDL truyền thống:
    * Thông lượng: vượt quá 1 triệu (tps).
    * Độ trễ: của một phần nhỏ của một giây.
    * Khả năng lưu trữ: đạt hàng petabyte và nhiều hơn.
    * Thông lượng và khả năng lưu trữ tăng lên khi các nút được thêm vào.

    Các công nghệ phân tán với khả năng kết nối với các hệ thống tài chính hiện đại, các chuỗi cung ứng, các nghành công nghiệp sáng tạo và ngay cả bản thân mạng internet đòi hỏi các yêu cầu cao về thông lượng, độ trễ và khả năng lưu trữ và nó vượt quá khả năng của bitcoin.

<a name="kien-truc"></a>

### Kiến trúc

Một production node trong Bigchain bao gồm:
* BigchainDB Server
* MongoDB Server 3.4+
* Tendermint
* Lưu trữ cho MongoDB và Tendermint

![](img/bigchaindb_mechanism.png?raw=true)

BigchainDB Server: Là phần giao tiếp giữa Tendermint. Thực hiện giao tiếp với BigChainDB Client. Nhận các transaction và chuyển cho Tendermint. Nếu các transaction hợp lệ sẽ được lưu trữ vào MongoDB.

Tendermint: Giao tiếp với các tendermint ở các node khác. Thực hiện xác thực transaction, nhận transaction từ các node khác trong mạng. Lưu trữ transaction vào MongoDB nếu hợp lệ. Đảm nhiệm khi có bất kì một Tendermint ở node nào trong mạng phát xinh vấn đề gây lỗi (ví dụ như bị thay đổi giữ liệu) thì node sẽ bị cô lập.

MongoDB Server: Lưu trữ giữ liệu trên hệ thống local của node đó. Dữ liệu sẽ được trao đổi giữa BigchainDB Server và Tendermint.

Mô hình vận hành mạng có thể hình dung như sau:

![](img/bigchaindb_communication.png?raw=true)

Giả sử khi dữ liệu được tạo ở Node 1. Client sẽ giao tiếp với BigchainDB server và gửi transaction lên. BigchainDB server nhận transaction gửi cho Terdermint xác thực. Khi transaction hợp lệ nó sẽ được lưu tại cơ sở dữ liệu mongoDB trên node, đồng thời transaction được gửi cho các Terdermint khác trong kết nối. Các terdermint khác nhận và thực hiện các xác thực liên quan sau đó lưu vào cơ sở dữ liệu mongo tại node đó.

<a name="mo-hinh-du-lieu"></a>

### Mô hình dữ liệu
1. Asset

    BigchainDB cơ bản lưu trữ tất cả các dữ liệu dưới hình thức các tài liệu JSON. BigchainDB tập trung lưu trữ khi coi mọi đối tượng là asset. Ví dụ: một chiếc xe đạp được coi như là một asset bao gồm các thông tin như ngày sản xuất, màu sơn, giá tiền.

    ![](img/bigchaindb_assert.png?raw=true)

2. Transaction

    BigchainDB đóng gói asset vào trong transaction và tạo ra các thuộc tính cần thiết cho transaction.
    Một mô hình điển hình có cấu trúc như sau:

    ![](img/bigchaindb_transaction.png?raw=true)

    * id: khóa chính của cơ sở dữ liệu, là định danh đồng thời cũng là mã băm của giao dịch.
    * version: Phiên bản của giao dịch, với BigchainDB Server phiên bản 2.0.0 thì giá trị được chấp nhận là “2.0”.
    * inputs: Danh sách các đầu vào, các đầu vào biến đổi hoặc sử dụng các đầu ra của giao dịch trước đó bằng cách đáp ứng các yêu cầu về bảo mật,... Khi tạo mới 1 giao dịch thì phải có ít nhất 1 đầu vào.
    * outputs: Danh sách các đầu ra, mỗi đầu ra phải đáp ứng được các yêu cầu về mặt bảo mật nếu muốn sử dụng, chuyển đổi. Nó đồng thời cũng thể hiện phần tài sản (asset) gắn với đầu ra đó.
    * asset: giữ các thông tin asset được lưu trữ, các thông tin asset sau khi được chuyển đi cùng transaction thì sau này không thể thay đổi được.
    * metadata: các dữ liệu bổ sung cho asset, và có thể thay đổi được sau này. Nên nó thường sẽ lưu các thông tin về asset có thể thay đổi sau này.

<a name="ma-hoa"></a>

### Mã hóa
1. Chữ ký số ed25519

    Ed25519 là một hệ thống chữ ký số khóa công khai với nhiều tính năng nổi trội:
    * Kiểm tra chữ ký đơn nhanh
    * Kiểm tra chữ ký nhanh trong cả trường hợp kiểm tra hàng loạt
    * Việc ký được thực hiện rất nhanh
    * Sinh khóa nhanh
    * Mức độ an toàn cao
    * Khóa phiên dễ dùng
    * Chống xung đột
    * Không dùng mảng chỉ số bí mật
    * Không có các điều kiện bí mật nhánh
    * Độ dài chữ ký nhỏ

    Mã hóa thường được sử dụng khi tạo mới transaction. Mã hóa sẽ sử dụng các thành phần đầu vào trong transaction để tạo ra output cho transaction đó. Mã hóa không được sử dụng cho transfer transaction

2. Threshhold

    Threshhold là mã hóa sẽ lấy các dữ liệu đầu vào của transaction giải mã và đối chiếu với chữ ký số của người sở hữu asset. Threshhold sử dụng cho các transfer transaction. Input của transaction hiện tại sẽ tương ứng với output của transaction chứa đối tượng cần chuyển trước đó. Sau khi xác nhận thành công threshhold mã hóa ngược lại các thông tin và đặt mã hóa vào output transaction.

<a name="cong-cu-phat-trien"></a>

### Công cụ phát triển

BigchainDB sử dụng 2 driver chủ yếu để giao tiếp với server đó là bigchaindb-driver và js-bigchaindb-driver.

* bigchaindb-driver: được xây dựng trên ngôn ngữ python. Thích hợp kết hợp với các ứng dụng desktop để gửi và nhận transaction. Ngoài ra cũng có thể kết hợp với một số framework web development để tương tác BigchianDB server.

* js-bigchaindb-driver: được xây dựng trên ngôn ngữ javascript. Thích hợp kết hợp với các framework web client-side như React, Angular. Hoặc sử nhúng vào site thông thường. Hỗ trợ tốt cho giao tiếp BigchainDB server trên giao diện website.
