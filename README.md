# Event Storming and Spring with a Splash of DDD (Vietnamese)

_Nguyên tắc để triển khai phần mềm nhanh hơn trong khi vẫn tăng cường độ tin cậy:_ <br>

- **HIỂU** - Giúp các nhóm hiểu và khắc phục khoảng cách giữa các business problems phức tạp (gọi là "domain") và model
  trong mã đại diện cho nó. Vấn đề phổ biến nhất mà tôi gặp phải là các domain models được đưa vào sản xuất thường cách
  xa so với những gì mà các chuyên gia domain hình dung.
- **PHÂN CHIA** - Phân chia chức năng phần mềm thành các mô-đun. Bằng mô-đun, tôi có ý nói đến bất kỳ phần độc lập nào
  của doanh nghiệp có thể là một hoặc nhiều đơn vị triển khai. Điều quan trọng là mỗi mô-đun phải được triển khai như
  các sản phẩm độc lập để chúng ta có thể áp dụng các phong cách kiến trúc khác nhau.
- **THỰC HIỆN** - Tái cấu trúc theo hướng microservices bằng cách thay đổi tư duy từ monoliths sang các hệ thống phân
  tán—hoặc
  ngăn cản việc đi theo con đường này khi nó không cần thiết!
- **TRIỂN KHAI** - Cải thiện quá trình phân phối bằng cách mở rộng nhận thức về các thói quen như Phát triển Dựa trên
  Kiểm
  thử (TDD), Tích hợp Liên tục (CI), và Phân phối Liên tục (CD).
- **XÂY DỰNG GIÁ TRỊ** - Sử dụng Spring Boot và Spring Cloud để rút ngắn thời gian cần thiết để mang lại giá trị kinh
  doanh.
  Cho phép các nhà phát triển dành nhiều thời gian nhất có thể để hiểu bản thân miền kinh doanh.

### Mô hình miền

Khi nói đến việc hiểu doanh nghiệp mà bạn đang xây dựng phần mềm, không có framework lập trình nào có thể giúp chúng ta
một cách kỳ diệu để hiểu và mô hình hóa một miền phức tạp. Tôi không kỳ vọng sẽ có một công cụ như vậy xuất hiện, bởi vì
nói chung, không thể dự đoán được cách mà một miền sẽ phát triển và thay đổi trong tương lai. Tuy nhiên, có một số miền
kinh doanh trừu tượng phổ biến mà hầu hết mọi người đều quen thuộc—như bán hàng, quản lý hàng tồn kho, hoặc danh mục sản
phẩm. Khi bắt đầu mô hình hóa miền từ đầu, không cần phải phát minh lại bánh xe. Dưới đây là một tài nguyên tuyệt vời mà
tôi khuyên dùng cho việc mô hình hóa miền phức tạp: [**Enterprise Patterns and MDA: Building Better Software with
Archetype Patterns and UML**.](https://www.amazon.com/Enterprise-Patterns-MDA-Building-Archetype/dp/032111230X)

### Hiểu, Phân chia, và Liên tục Chinh phục

Khi triển khai phần mềm nhanh chóng, chúng ta không được hy sinh khả năng hiểu mã của người khác sau này. May mắn thay,
chúng ta có một tập hợp các nguyên tắc và thực hành giúp đỡ—dưới dạng Thiết kế Hướng Miền (Domain-Driven Design - DDD).
Cá nhân tôi thích nghĩ về DDD như là một quá trình học tập lặp đi lặp lại về những điều chưa biết. Tác động phụ của việc
áp dụng DDD là chúng ta có thể làm cho mã của mình dễ hiểu hơn, có thể mở rộng và nhất quán hơn cho cả các nhà phát
triển và doanh nghiệp. Với DDD, mã nguồn của chúng ta có thể trở thành nguồn sự thật duy nhất về cách một miền nên hoạt
động. Chức năng phần mềm vốn dĩ là để thay đổi. Nhưng khi một nhà phát triển không thể truyền đạt mã nguồn cho doanh
nghiệp bằng những thuật ngữ mà họ hiểu, chức năng đó trở nên trang trí và khó thay đổi hoặc thay thế.

Ngay cả những miền phức tạp nhất cũng có thể được phân chia thành:

- **Các tiểu miền nhỏ hơn nhưng vẫn khá phức tạp (gọi là miền cốt lõi)** - đây có lẽ là lợi thế cạnh tranh lớn nhất của
  doanh nghiệp chúng ta, vì vậy chúng ta đầu tư rất nhiều công sức ở đó.

- **Các tiểu miền đơn giản và dễ hiểu mà có thể không độc đáo đối với doanh nghiệp của chúng ta (gọi là miền chung)** -
  chúng ta cần chúng để doanh nghiệp hoạt động nhưng nó không mang lại lợi thế cạnh tranh cho khách hàng của chúng ta.
  Hãy nghĩ đến quản lý hàng tồn kho hoặc lập hóa đơn. Người dùng của chúng ta sẽ không quay lại chỉ vì hóa đơn được
  thiết kế đẹp.

Việc xác định những sản phẩm nhỏ hơn này giúp chúng ta có bản phác thảo đầu tiên về cách tổ chức mã của mình thành các
mô-đun. Mỗi tiểu miền tương ứng với một mô-đun riêng biệt. Hiểu được sự khác biệt giữa miền cốt lõi và miền chung giúp
chúng ta thấy rằng chúng có thể cần các phong cách kiến trúc khác nhau.

May mắn thay, chúng ta có rất nhiều thành phần để lựa chọn!

### Ví dụ

Tôi rất vui mừng thông báo rằng, cùng với người bạn của mình, [Michał Michaluk](https://x.com/michal_michaluk), chúng
tôi đã tạo ra một sáng kiến mang tên #dddbyexamples. Mục đích của sáng kiến này là kết nối các phần khác nhau của hệ
sinh thái Spring với sự quan tâm của những người đam mê DDD. Bạn có thể xem các ví dụ mẫu của chúng tôi tại đây. Cho đến
nay, có hai ví dụ mẫu. Một ví dụ tập trung vào Event Sourcing (nguồn sự kiện) và Command Query Responsibility
Segregation (phân tách trách nhiệm truy vấn lệnh), trong khi ví dụ kia tập trung vào một ví dụ DDD từ đầu đến cuối. Cả
hai đều được triển khai bằng Spring Boot.

Hãy cùng đi sâu vào ví dụ từ đầu đến cuối này. Chúng ta sẽ triển khai một hệ thống quản lý thẻ tín dụng đơn giản hóa.
Chúng ta sẽ phân đoạn công việc thành các bước: Hiểu, Phân chia, Thực hiện và Triển khai. Yêu cầu chưa rõ ràng lắm, và
hiện tại chúng ta biết rằng hệ thống cần phải có khả năng:

- **Gán hạn mức ban đầu cho thẻ**
- **Rút tiền**
- **Tạo báo cáo với số tiền cần phải trả (vào cuối kỳ thanh toán)**
- **Thanh toán nợ**
- **Đặt hàng hoặc thay đổi thẻ nhựa cá nhân hóa**

### Hiểu

Để hiểu rõ hơn về vấn đề kinh doanh mà chúng ta đang đối mặt, chúng ta có thể sử dụng một kỹ thuật nhẹ nhàng gọi là
**[Event Storming](https://en.wikipedia.org/wiki/Event_storming)**. Tất cả những gì chúng ta cần là không gian rộng lớn
trên một bức tường, các ghi chú dán (sticky
notes), và cả những người từ phía kinh doanh lẫn kỹ thuật cùng có mặt trong một phòng. Bước đầu tiên là ghi lại những gì
có thể xảy ra trong miền của chúng ta trên các ghi chú màu cam. Đây chính là các sự kiện trong miền (domain events). Lưu
ý rằng chúng ta sử dụng thì quá khứ và không có thứ tự cụ thể nào.

![](https://github.com/pilloPl/eventstorming-with-spring/blob/master/just-events.png?raw=true)

Sau đó, chúng ta phải xác định nguyên nhân của mỗi sự kiện. Các chuyên gia miền sẽ biết nguyên nhân và rất có thể chúng
ta có thể phân loại nguyên nhân này thành:

- **Một lệnh trực tiếp tới hệ thống** - ghi chú màu xanh bên cạnh sự kiện.
- **Một sự kiện khác** - trong trường hợp này, chúng ta đặt các sự kiện đó cạnh nhau.
- **Một khoảng thời gian đã trôi qua** - ghi chú nhỏ ghi là "time" (thời gian).

![](https://github.com/pilloPl/eventstorming-with-spring/blob/master/events-and-causes.png?raw=true)

Ngoài ra còn có một ghi chú màu xanh lá: **plastic card personalization view** (xem cá nhân hóa thẻ nhựa). Đây là một
thông điệp trực tiếp gửi đến hệ thống và gây ra sự kiện **plastic card personalization displayed** (hiển thị cá nhân hóa
thẻ nhựa). Nhưng đây là một truy vấn, không phải là lệnh. Đối với các view (lượt xem) và mô hình đọc, chúng ta sẽ sử
dụng ghi chú màu xanh lá cây.

Bước tiếp theo rất quan trọng. Chúng ta cần biết liệu nguyên nhân đó có đủ để sự kiện miền xảy ra hay không. Có thể có
điều kiện khác cần phải được đáp ứng. Có thể có nhiều hơn một điều kiện. Những điều kiện này được gọi là **invariants
** (bất biến). Nếu có, chúng ta ghi chúng vào các ghi chú màu vàng và đặt giữa các sự kiện và nguyên nhân.

![](https://github.com/pilloPl/eventstorming-with-spring/blob/master/invariants.png?raw=true)

Nếu chúng ta áp dụng thứ tự thời gian cho các sự kiện của mình, chúng ta sẽ có được một cái nhìn tổng quan rất tốt về
miền của mình. Hơn nữa, chúng ta sẽ hiểu rõ hơn về các quy trình kinh doanh cơ bản. Kỹ thuật này nhẹ nhàng, nhanh chóng,
thú vị và mô tả hơn nhiều so với hàng loạt tài liệu văn bản hay các mockup giao diện người dùng. Tuy nhiên, nó vẫn chưa
tạo ra một dòng mã nào, phải không?

### Phân chia

Để tìm ra ranh giới giữa các mô-đun kinh doanh, chúng ta có thể áp dụng nguyên tắc **cohesion** (sự gắn kết): những thứ
thay đổi cùng nhau và được sử dụng cùng nhau nên được giữ chung trong một mô-đun. Ví dụ, trong một mô-đun duy nhất. Làm
thế nào chúng ta có thể nói về sự gắn kết khi chỉ có một tập hợp các ghi chú màu sắc? Hãy xem xét.

Để kiểm tra các **invariants** (bất biến) (ghi chú màu vàng), hệ thống phải đặt một số câu hỏi. Ví dụ, để rút tiền, cần
phải có hạn mức đã được gán. Hệ thống phải thực hiện một truy vấn: "Chào, có hạn mức được gán không?". Mặt khác, có
những lệnh và sự kiện có thể thay đổi câu trả lời cho câu hỏi đó. Ví dụ, lệnh đầu tiên để gán hạn mức sẽ thay đổi câu
trả lời từ không thành có mãi mãi. Đây là một chỉ số rõ ràng về các hành vi có sự gắn kết cao có thể gộp chung vào một
mô-đun hoặc lớp.

Hãy áp dụng phương pháp này ở tất cả các nơi. Trên các ghi chú màu xanh lá cây, chúng ta sẽ viết tên của một truy
vấn/view mà hệ thống cần kiểm tra trong quá trình xử lý mỗi bất biến. Đồng thời, hãy làm nổi bật khi câu trả lời cho
truy vấn/view đó có thể thay đổi do sự kiện. Theo cách đó, các ghi chú màu xanh lá cây có thể được đặt cạnh các bất biến
hoặc cạnh các sự kiện.

![](https://i.imgur.com/G9XVk63.png)

Hãy tìm kiếm mô hình sau:

- **Lệnh CmdA** được kích hoạt và gây ra **Sự kiện EventA**.
- **Sự kiện EventA** ảnh hưởng đến **view SomeView**.
- **SomeView** cũng cần thiết trong khi xử lý một **bất biến** bảo vệ **CmdB**.
- Điều này có nghĩa là **CmdA** và **CmdB** có thể là ứng viên tốt để nằm trong cùng một mô-đun!
- Hãy đặt các lệnh đó (cùng với các bất biến và sự kiện) cạnh nhau.

Việc làm như vậy có thể phân đoạn miền của chúng ta thành các điểm gắn kết rất chặt chẽ. Dưới đây là một phân vùng
mô-đun được đề xuất. Hãy nhớ rằng đây chỉ là một phương pháp gợi ý, bạn có thể có một thiết lập khác. Kỹ thuật đề xuất
cho chúng ta cơ hội tốt để xác định các mô-đun có sự kết nối lỏng lẻo. Phương pháp này chỉ là một phương pháp gợi ý (
không phải quy tắc cứng nhắc) có thể giúp chúng ta tìm các mô-đun độc lập.

Hơn nữa, nếu bạn suy nghĩ về điều đó, các mô-đun đề xuất có ranh giới ngôn ngữ. **Thẻ tín dụng** có ý nghĩa khác nhau
đối với kế toán và marketing, mặc dù đây là cùng một từ. Trong thuật ngữ DDD, đây được gọi là **Bounded Contexts** (Bối
cảnh giới hạn). Những bối cảnh này sẽ là các đơn vị triển khai của chúng ta. Đồng thời, sự tổng quát này phải xem xét
liệu hiệu ứng có nên là ngay lập tức hay cuối cùng. Nếu nó có thể nhất quán cuối cùng, phương pháp gợi ý này không mạnh
mẽ như vậy, mặc dù vẫn có mối quan hệ.

![](https://i.imgur.com/YJBU0WO.png)

Bước cuối cùng trong phần **PHÂN CHIA** là xác định cách các mô-đun giao tiếp với nhau. Đây là quá trình được gọi là
**context mapping** (hoạch định bối cảnh). Dưới đây là một số chiến lược tích hợp:

- **Một mô-đun gửi truy vấn đến mô-đun khác** - Ví dụ, mô-đun **Statement** cần hỏi **Card Operations** xem có bất kỳ
  khoản rút tiền nào không. Bởi vì nếu không có, nó sẽ không phát hành bất kỳ báo cáo nào.

- **Một mô-đun lắng nghe các sự kiện được gửi bởi mô-đun khác** - Hệ quả trực tiếp của sự kiện **Money Repaid** (Tiền đã
  thanh toán) là sự kiện **Statement Closed** (Báo cáo đã đóng). Điều này có nghĩa là **Statements** sẽ đăng ký nhận các
  sự kiện do **Card Operations** phát ra. Đây là điều đã bị bỏ qua ở đầu buổi Event Storming. **Context Mapping** thực
  sự là thời điểm chúng ta phát hiện ra nhiều thông tin mới.

- **Một mô-đun phát lệnh đến mô-đun khác** - Không có ví dụ nào như vậy trong hệ thống của chúng ta.

Việc xác định các chiến lược tích hợp giúp đảm bảo rằng các mô-đun có thể phối hợp hiệu quả với nhau, hỗ trợ việc duy
trì sự nhất quán và đồng bộ trong toàn hệ thống.

![](https://i.imgur.com/vBhouxJ.png)

### Triển khai

Việc phân tách phần mềm theo chức năng giúp rất nhiều trong quá trình bảo trì. **Modular monolith** (monolith mô-đun) là
một khởi đầu tốt, nhưng thực tế là nó là một đơn vị triển khai duy nhất có thể gây ra vấn đề. Tất cả các mô-đun phải
được triển khai cùng nhau. Trong một số doanh nghiệp, việc chuyển sang **microservices** có thể là một lựa chọn tốt hơn.
Vui lòng tham
khảo [bài viết](https://tanzu.vmware.com/content/blog/should-that-be-a-microservice-keep-these-six-factors-in-mind?utm_campaign=content-social&utm_medium=social-sprout&utm_source=twitter&utm_content=1516394228)
của [Nate Shutt](https://x.com/ntschutta) để tìm hiểu thêm về khi nào quyết định này là hợp lý.

Giả sử ví dụ của chúng ta phù hợp với kiến trúc microservices. Mỗi mô-đun có thể là một ứng dụng Spring Boot riêng biệt.
Chúng ta đã biết ranh giới của các mô-đun. Các phong cách kiến trúc khác nhau có thể được áp dụng cho từng mô-đun. Những
nơi chứa nhiều logic kinh doanh nhất cần được triển khai với sự chú ý cẩn thận. Mặt khác, có những mô-đun rõ ràng và đơn
giản. Làm thế nào để tìm cả hai?

- **Tìm các khu vực có nhiều ghi chú màu vàng (invariants)**. Đây là những nơi có nhiều logic giữa một command và sự
  kiện
  cuối cùng. Hệ thống cần xử lý các command tạp ở đây. Chúng ta mong đợi các thay đổi đột ngột và nơi đây có thể tạo
  ra lợi thế cạnh tranh. Chúng ta muốn chăm sóc đặc biệt cho các khu vực này, chẳng hạn như áp dụng các kỹ thuật
  Domain-Driven Design hoặc hexagonal architecture.

- **Tìm các khu vực chứa ít hoặc không có ghi chú màu vàng**. Đây là những khu vực rõ ràng và dễ triển khai. Hầu như
  không có gì giữa một command và event, hệ thống không cần làm gì phức tạp ở đây. Công việc duy nhất ở đây là tương tác
  với cơ sở dữ liệu, vì vậy chúng ta nên cẩn thận và cố gắng tránh sự phức tạp không cần thiết.

Kiến thức này là một yếu tố quan trọng trong kiến trúc, giúp chúng ta quyết định việc tách biệt **_commands exposure_
** (
ví dụ, REST resource) khỏi **_commands processing_** (domain model with invariants). Yếu tố kiến trúc này khi áp dụng
cho **Card
Operations** dẫn chúng ta đến các công nghệ sau:

![](https://i.imgur.com/LBOtKc5.png)

Hãy xem xét các lệnh và các bất biến liên quan (ghi chú xanh và vàng). Trên tường, chúng ta đã có một bộ kịch bản kiểm
thử hoàn chỉnh! Điều duy nhất còn lại là ghi chúng lại:

```java
class CreditCardTest {

    @Test
    public void cannot_withdraw_when_limit_not_assigned() {

    }

    @Test
    public void cannot_withdraw_when_not_enough_money() {

    }

    @Test
    public void cannot_withdraw_when_there_was_withdrawal_within_lastH() {

    }

    @Test
    public void can_withdraw() {

    }

    @Test
    public void cannot_assign_limit_when_it_was_already_assigned() {

    }

    @Test
    public void can_assign_limit() {

    }

    @Test
    public void can_repay() {

    }

}
```

Và theo các nguyên tắc của TDD, chúng ta có thể thiết kế mã nguồn của mình để đáp ứng các kịch bản này. Tiếp theo là
thiết kế ban đầu mà chúng ta có thể xây dựng từ các ghi chú màu xanh và vàng.

```java

@Entity
class CreditCard {

    //..fields will pop-up during TDD!

    void assignLimit(BigDecimal money) {
        if (limitAlreadyAssigned()) {
            // throw
        }
        //...
    }

    void withdraw(BigDecimal money) {
        if (limitNotAssigned()) {
            // throw
        }
        if (notEnoughMoney()) {
            // throw
        }
        if (withdrawalWithinLastHour()) {
            // throw
        }

        //...
    }

    void repay(BigDecimal money) {

    }

}
```

Vì chúng ta đã sử dụng các sticky notes, chúng ta đã thực hiện suy nghĩ của mình trong giai đoạn thiết kế. Chúng ta chỉ
việc sao chép những gì có trên các ghi chú dán và dán vào mã nguồn. Ngôn ngữ giống nhau có mặt trên các ghi chú và trong
mã nguồn, đó là một phần làm cho **event storming** trở nên mạnh mẽ. Là một nhà phát triển, quy trình này cho phép chúng
ta tập trung vào những gì chúng ta làm tốt nhất, đó là viết mã nguồn vững chắc. Language và các models chỉ là một phần
của quá trình làm việc cùng với các chuyên gia trong lĩnh vực business's domain.

Bây giờ, hãy triển khai integration layer. Để thực hiện việc trả lời cho view _list of withdrawals_ được yêu cầu bởi
mô-đun
**Statements**, chúng ta sẽ tạo tài nguyên REST **withdrawals**. Đồng thời, đây cũng sẽ là ứng cử viên tự nhiên để
exposing command **withdraw**. Như thường lệ, hãy bắt đầu với một bài test:

```java

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = RANDOM_PORT)
class WithdrawalControllerTest {

    private static final String ANY_CARD_NO = "no";

    @Autowired
    TestRestTemplate testRestTemplate;

    @Test
    public void should_show_correct_number_of_withdrawals() {
        // when
        testRestTemplate.postForEntity("/withdrawals/" + ANY_CARD_NO,
                new WithdrawRequest(TEN),
                WithdrawRequest.class);

        // then
        ResponseEntity res = testRestTemplate.getForEntity(
                "/withdrawals/" + ANY_CARD_NO,
                WithdrawRequest.class);
        assertThat(res.getStatusCode().is2xxSuccessful()).isTrue();
        assertThat(res.getBody()).hasSize(1);
    }

}

```

Và triển khai

```java

@RestController("/withdrawals")
class WithdrawalController {

    @GetMapping("/{cardNo}")
    ResponseEntity withdrawalsForCard(@PathVariable String cardNo) {
        //.. stack for query
        // - direct call to DB to Withdrawals
    }

    @PostMapping("/{cardNo}")
    ResponseEntity withdraw(@PathVariable String cardNo, @RequestBody WithdrawRequest r) {
        //.. stack for commands
        // - call to CreditCard.withdraw(r.amount)
        // - insert new Withdrawal to DB
    }

}
```

Theo context map, `Repay` command phát ra sự kiện `MoneyRepaid`. Một message broker sẽ là một ứng cử viên tự nhiên
để vận chuyển không đồng bộ các domain events. Để thực hiện messaging, chúng ta sẽ tiết kiệm thời gian bằng cách sử dụng
[Spring Cloud Stream](https://spring.io/projects/spring-cloud-stream). Hãy tạo một bài kiểm tra end-to-end:

```java

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = RANDOM_PORT)
class RepaymentsTest {

    private static final String ANY_CARD_NO = "no";

    @Autowired
    TestRestTemplate testRestTemplate;

    @Autowired
    MessageCollector messageCollector;

    @Autowired
    Source source;

    BlockingQueue<Message<?>> outputEvents;

    @BeforeClass
    public void setup() {
        outputEvents = messageCollector.forChannel(source.output());
    }

    @Test
    public void should_show_correct_number_of_withdrawals_after_1st_withdrawal() {
        // given
        testRestTemplate.postForEntity("/withdrawals/" + ANY_CARD_NR,
                new WithdrawRequest(TEN),
                WithdrawRequest.class);

        // when
        testRestTemplate.postForEntity("/repayments/" + ANY_CARD_NR,
                new RepaymentRequest(TEN),
                RepaymentRequest.class);

        // then
        assertThat(
                outputEvents.poll()
                        .getPayload() instanceof MoneyRepaid)
                .isTrue();
    }

}
```

And the implementation:

```java

@RestController("/repayments")
class RepaymentController {

    private final Source source;

    RepaymentController(Source source) {
        this.source = source;
    }

    @PostMapping("/{cardNr}")
    ResponseEntity repay(@PathVariable String cardNo, @RequestBody RepaymentRequest r) {
        //.. stack for commands
        // - call to CreditCard.repay(r)
        // - source.output().send(... new MoneyRepaid(...));
    }

}

class RepaymentRequest {

    final BigDecimal amount;

    RepaymentRequest(BigDecimal amount) {
        this.amount = amount;
    }
}
```

Module **PlasticCards** rất đơn giản. Không có các điều kiện bất biến và trách nhiệm duy nhất là giao tiếp với cơ sở dữ
liệu và/hoặc bộ trung gian tin nhắn. Để tránh làm vấn đề trở nên phức tạp, hãy lưu ý rằng module này có bốn chức năng
chính: tạo, cập nhật, đọc và xóa. **Spring Data REST** là một dự án tuyệt vời để dễ dàng tạo ra một kho lưu trữ CRUD cơ
bản mà không cần phải lo lắng nhiều về cấu trúc hay các vấn đề kết nối phức tạp.

![](https://i.imgur.com/FUTqyMX.png)
Spring Data cho phép chúng ta thực hiện một repository từ thiết kế trên chỉ trong vài dòng code. Người ta có thể lập
luận rằng một thử nghiệm đơn giản để test xem các contexts và entity mappings có ổn hay không, có vẻ như là một ý
tưởng hay. Để ngắn gọn, chúng ta hãy bỏ qua điều đó và chuyển trực tiếp đến việc triển khai:

```java

@RepositoryRestResource(path = "plastic-cards",
        collectionResourceRel = "plastic-cards",
        itemResourceRel = "plastic-cards")
interface PlasticCardController extends CrudRepository<PlasticCard, Long> {

}

@Entity
class PlasticCard {

    //..
}
```

Mặc dù module **Statements** có một invariant, nhưng nó cũng rất gần với một CRUD interface đơn giản. Để xử lý
invariant, module này tương tác với module **CardOperations**. Để test hành vi này một cách độc lập (mà
không cần một instance thực của **CardOperations** trong ứng dụng Spring Boot), chúng ta nên xem xét [**Spring Cloud
Contract**](https://spring.io/projects/spring-cloud-contract#learn) và bắt đầu giới thiệu nó vào stack đề xuất của chúng
ta.

Statements là những documents đơn giản về bản chất, và **Spring Data MongoDB** cung cấp khả năng này một cách tự nhiên
với các document collections. Module **Statements** không cung cấp bất kỳ endpoint nào cho các command, nhưng nó đăng ký
với
command **MoneyRepaid** và tận dụng các khả năng messaging của **Spring Cloud Stream**.

![](https://i.imgur.com/HhawAb9.png)

Có một tình huống thú vị: đóng một statement (sao kê) do nhận được sự kiện `MoneyRepay`. Quá trình kiểm tra có thể kích
hoạt sự
kiện giả mạo bằng các công cụ kiểm tra Spring Cloud Stream:

```java

@RunWith(SpringRunner.class)
@SpringBootTest
class MoneyRepaidListenerTest {

    private static final String ANY_CARD_NR = "nr";

    @Autowired
    Sink sink;
    @Autowired
    StatementRepository statementRepository;

    @Test
    public void should_close_the_statement_when_money_repaid_event_happens() {
        // when
        sink.input()
                .send(new GenericMessage<>(new MoneyRepaid(ANY_CARD_NR, TEN)));

        // then
        assertThat(statementRepository
                .findLastByCardNr(ANY_CARD_NR).isClosed()).isTrue();
    }

}
```

And the implementation:

```java

@Component
class MoneyRepaidListener {

    @StreamListener("card-operations")
    public void handle(MoneyRepaid moneyRepaid) {
        //..close statement
    }
}

class MoneyRepaid {

    final String cardNo;
    final BigDecimal amount;

    MoneyRepaid(String cardNo, BigDecimal amount) {
        this.cardNo = cardNo;
        this.amount = amount;
    }
}
```

Mặt khác, quá trình tạo Statement (bản sao kê) yêu cầu một truy vấn đến module **CardOperations** để kiểm tra
`withdrawals`
hiện có. Như đã đề cập, điều này nên được kiểm thử một cách độc lập. Để làm điều đó, có thể đề xuất một contract (hợp
đồng) với
nhóm chịu trách nhiệm về module **CardOperations**. Do đó, phiên bản giả lập của module đó có thể được khởi chạy cho mục
đích thử nghiệm. Mẫu giả lập **[WireMock](https://wiremock.org/)** được tạo từ hợp đồng có thể trông như sau...

```json
{
  "request": {
    "url": "/withdrawals/123",
    "method": "GET"
  },
  "response": {
    "status": 200,
    "body": "{\"withdrawals\":\"["
    first
    ", "
    second
    ", "
    third
    "]\"}"
  }
}

{
  "request": {
    "url": "/withdrawals/456",
    "method": "GET"
  },
  "response": {
    "status": 204,
    "body": "{}"
  }
}
```

Và đây là các thử nghiệm, nhờ vào hợp đồng, sẽ hoạt động mà không cần bất kỳ phiên bản thực tế nào của `CardOperations`:

```java

@RunWith(SpringRunner.class)
class StatementGeneratorTest {

    private static final String USED_CARD = "123";
    private static final String NOT_USED_CARD = "456";

    @Autowired
    StatementGenerator statementGenerator;
    @Autowired
    StatementRepository statementRepository;

    @Test
    public void should_create_statement_only_if_there_are_withdrawals() {
        // when
        statementGenerator.generateStatements();

        // then
        assertThat(statementRepository
                .findOpenByCardNr(USED_CARD)).hasSize(1);
        assertThat(statementRepository
                .findOpenByCardNr(NOT_USED_CARD)).hasSize(0);

    }

}
```

The last thing is the implementation:

```java

@Component
class StatementGenerator {

    @Scheduled
    public void generateStatements() {
        allCardNumbers()
                .forEach(this::generateIfNeeded);
    }

    private void generateIfNeeded(CardNr cardNo) {
        //query to card-operations
        //if 200 OK - generate and statement
    }

    private List<CardNr> allCardNumbers() {
        return callToCardRepository();
    }
}
```

**Sử dụng Spring Cloud Pipelines, chúng ta có thể dễ dàng giới thiệu CI/CD và hoàn tất phần triển khai.**

> Nếu bạn quan tâm, đừng bỏ
> lỡ [buổi nói chuyện của Cora Iberkleid và Marcin Grzejszczak về Spring Cloud Pipelines](https://tanzu.vmware.com/content/springone-platform-2017/continuous-deployment-to-the-cloud-marcin-grzejszczak-cora-iberkleid).

**Kết luận**

Event Storming giúp chúng ta nhanh chóng hiểu rõ về lĩnh vực của mình. Theo các nguyên tắc của DDD, chúng ta có thể chia
doanh nghiệp thành những vấn đề nhỏ gọn và ít phụ thuộc lẫn nhau. Biết được độ phức tạp của từng module và cách chúng
cần giao tiếp với nhau, chúng ta có thể lựa chọn từ một loạt công cụ trong hệ sinh thái Spring để triển khai và thực
hiện rất nhanh chóng.

**Lời cảm ơn đặc biệt**

Tôi muốn cảm ơn Kenny Bastani vì nhiều nhận xét hữu ích về bản thảo ban đầu của bài viết này. Nhưng trước hết, tôi muốn
cảm ơn anh ấy vì đã có rất nhiều ý tưởng tuyệt vời khi chúng tôi cùng nhau tạo và diễn tập bài thuyết trình tại
SpringOne.

Ngoài ra, tôi cũng muốn cảm ơn Marcin Grzejszczak vì những cuộc thảo luận bất tận về microservices và testing. Tôi có
thể nói rằng bạn hiếm khi thấy được sự đam mê và nhiệt huyết nhiều như vậy ở một người.
