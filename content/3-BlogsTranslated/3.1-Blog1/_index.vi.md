---
title: "Blog 1"
date: 2025-09-23
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Trí tuệ Nhân tạo (Artificial Intelligence): Tích hợp mã hóa token (tokenization) với Amazon Bedrock Guardrails để xử lý dữ liệu an toàn

Viết bởi **Nizar Kheir** và **Mark Warner** | Vào ngày **23 Tháng 09 năm 2025** | Trên trang **Amazon API Gateway, Amazon Bedrock Guardrails, AWS Lambda, AWS Step Functions, Partner solutions, Technical How-to** |

Bài đăng này được đồng viết bởi Mark Warner, Principal Solutions Architect cho Thales, Cyber Security Products.

Khi các ứng dụng AI tạo sinh tiến vào môi trường sản xuất, chúng tích hợp với một phạm vi rộng hơn các hệ thống kinh doanh xử lý **dữ liệu khách hàng nhạy cảm**. Sự tích hợp này mang lại những thách thức mới xung quanh việc bảo vệ **thông tin nhận dạng cá nhân (PII)** đồng thời duy trì **khả năng phục hồi dữ liệu gốc** khi các ứng dụng hạ nguồn (downstream applications) cần một cách hợp pháp. Hãy xem xét một công ty dịch vụ tài chính triển khai AI tạo sinh trên các phòng ban khác nhau. Đội ngũ dịch vụ khách hàng cần một trợ lý AI có thể truy cập hồ sơ khách hàng và cung cấp các phản hồi cá nhân hóa bao gồm thông tin liên hệ. Trong khi đó, đội ngũ phân tích gian lận (fraud analysis team) yêu cầu cùng dữ liệu khách hàng đó nhưng phải phân tích các mẫu hình (patterns) mà không để lộ PII thực tế, chỉ làm việc với các biểu diễn được bảo vệ của thông tin nhạy cảm.

**Amazon Bedrock Guardrails** giúp phát hiện thông tin nhạy cảm, chẳng hạn như PII, ở định dạng tiêu chuẩn trong các input prompts hoặc phản hồi của mô hình. Các bộ lọc thông tin nhạy cảm cung cấp cho các tổ chức quyền kiểm soát cách dữ liệu nhạy cảm được xử lý, với các tùy chọn để chặn các yêu cầu chứa PII hoặc **che giấu (mask)** thông tin nhạy cảm bằng các generic placeholders như `{NAME}` hoặc `{EMAIL}`.

Mặc dù việc che giấu bảo vệ hiệu quả thông tin nhạy cảm, nó lại tạo ra một thách thức mới: **sự mất khả năng đảo ngược dữ liệu (loss of data reversibility)**. Khi guardrails thay thế dữ liệu nhạy cảm bằng các mặt nạ chung chung, thông tin gốc trở nên không thể truy cập được đối với các ứng dụng hạ nguồn có thể cần nó cho các quy trình kinh doanh hợp pháp.

**Mã hóa token (Tokenization)** đưa ra một phương pháp tiếp cận bổ sung cho thách thức này. Không giống như che giấu, mã hóa token thay thế dữ liệu nhạy cảm bằng các **token giữ nguyên định dạng (format-preserving tokens)** không liên quan về mặt toán học với thông tin gốc nhưng vẫn duy trì cấu trúc và khả năng sử dụng của nó. Các token này có thể được **đảo ngược trở lại** một cách an toàn về giá trị gốc của chúng khi cần bởi các hệ thống được ủy quyền.

Trong bài đăng này, chúng tôi chỉ cho bạn cách tích hợp Amazon Bedrock Guardrails với các dịch vụ mã hóa token của bên thứ ba để bảo vệ dữ liệu nhạy cảm trong khi vẫn duy trì khả năng đảo ngược dữ liệu. Giải pháp được mô tả chứng minh cách kết hợp Amazon Bedrock Guardrails với các dịch vụ mã hóa token từ **Thales CipherTrust Data Security Platform** để tạo ra một kiến trúc bảo vệ dữ liệu nhạy cảm mà không phải hy sinh khả năng xử lý dữ liệu đó một cách an toàn khi cần.

---

## API của Amazon Bedrock Guardrails

Amazon Bedrock Guardrails cung cấp hai cách tiếp cận để triển khai các biện pháp kiểm soát an toàn nội dung (content safety controls):

* **Tích hợp trực tiếp** với lời gọi mô hình thông qua các API như `InvokeModel` và `Converse`.
* **Đánh giá độc lập** thông qua **API ApplyGuardrail**, API này tách biệt việc đánh giá guardrails khỏi lời gọi mô hình.

Bài đăng này sử dụng **API ApplyGuardrail** để tích hợp mã hóa token vì nó tách biệt việc đánh giá nội dung khỏi lời gọi mô hình, cho phép chèn quá trình xử lý mã hóa token giữa các bước này.

Giải pháp mở rộng việc triển khai API `ApplyGuardrail` điển hình bằng cách chèn quá trình xử lý mã hóa token giữa đánh giá guardrail và lời gọi mô hình, như sau:

1.  Ứng dụng gọi **API ApplyGuardrail** để đánh giá đầu vào của người dùng về thông tin nhạy cảm.
2.  Nếu thông tin nhạy cảm được phát hiện (`action = "ANONYMIZED"`):
    * Ứng dụng nắm bắt PII đã phát hiện và vị trí của nó.
    * Nó gọi một dịch vụ mã hóa token để chuyển đổi các entities này thành các **token giữ nguyên định dạng**.
    * Nó thay thế các mặt nạ guardrail chung chung bằng các token này.
3.  Sau đó, ứng dụng gọi mô hình nền tảng (foundation model) với nội dung đã được mã hóa token.

---

## Tổng quan giải pháp

Để minh họa cách quy trình công việc này mang lại giá trị trong thực tế, hãy xem xét một ứng dụng tư vấn tài chính. Ba thành phần ứng dụng riêng biệt hoạt động cùng nhau để cung cấp các thông tin chi tiết tài chính an toàn:

* **Dịch vụ cổng khách hàng (Customer gateway service):** Nhận các truy vấn của khách hàng chứa thông tin nhạy cảm.
* **Công cụ phân tích tài chính (Financial analysis engine):** Thành phần hỗ trợ AI này phân tích các mẫu hình tài chính và tạo ra các đề xuất, hoạt động với thông tin được ẩn danh hoặc được mã hóa token.
* **Dịch vụ xử lý phản hồi (Response processing service):** Xử lý giao tiếp cuối cùng với khách hàng, bao gồm việc giải mã token (detokenizing) thông tin nhạy cảm.



Quy trình công việc được điều phối bởi **AWS Step Functions** và sử dụng các hàm **AWS Lambda** để tuần tự phát hiện PII, mã hóa token, lời gọi mô hình AI và giải mã token trên ba thành phần chính:

1.  **Dịch vụ cổng khách hàng** nhận đầu vào $\rightarrow$ Gọi **API ApplyGuardrail**.
2.  Guardrail phát hiện PII $\rightarrow$ Dịch vụ cổng gọi **dịch vụ mã hóa token** (ví dụ: Thales CipherTrust) để tạo **token giữ nguyên định dạng**.
3.  Đầu vào với các giá trị được mã hóa token (ví dụ: `[[TOKEN_123]]`) được chuyển đến **Công cụ phân tích tài chính**.
4.  Công cụ phân tích gọi một **LLM trên Amazon Bedrock** để tạo lời khuyên tài chính bằng dữ liệu được mã hóa token.
5.  Phản hồi của mô hình (có thể chứa token) được gửi đến **Dịch vụ xử lý phản hồi**.
6.  Dịch vụ xử lý gọi dịch vụ mã hóa token để **giải mã token** (detokenize), khôi phục các giá trị nhạy cảm gốc.
7.  Phản hồi cuối cùng, đã được giải mã token, được gửi đến khách hàng.

Kiến trúc này duy trì tính bảo mật dữ liệu xuyên suốt luồng xử lý trong khi vẫn giữ được tính hữu ích của thông tin.

---

## Điều kiện tiên quyết (Prerequisites)

Để triển khai giải pháp này, bạn cần:

* **Một tài khoản AWS** với **Amazon Bedrock** được bật.
* Các quyền **AWS IAM** thích hợp: `bedrock:CreateGuardrail`, `bedrock:ApplyGuardrail`, và `bedrock-runtime:InvokeModel`.
* Môi trường **Python 3.7+** với thư viện `boto3` và thông tin xác thực AWS đã được cấu hình.
* Một **dịch vụ mã hóa token đã được triển khai** và có thể truy cập thông qua các điểm cuối REST API (ví dụ: Thales CipherTrust Data Security Platform).

---

## Cấu hình Amazon Bedrock Guardrails

Sử dụng AWS SDK để cấu hình Guardrail với chính sách thông tin nhạy cảm để **ẩn danh (ANONYMIZE)** các loại PII như `URL`, `EMAIL`, và `NAME` cho cả đầu vào và đầu ra:

```python
import boto3
def create_bedrock_guardrail():
    """
    Tạo một guardrail trong Amazon Bedrock cho các ứng dụng tài chính với bảo vệ PII.
    """
    bedrock = boto3.client('bedrock')
    
    response = bedrock.create_guardrail(
        name="FinancialServiceGuardrail",
        description="Guardrail for financial applications with PII protection",
        sensitiveInformationPolicyConfig={
            'piiEntitiesConfig': [
                {'type': 'URL', 'action': 'ANONYMIZE', 'inputAction': 'ANONYMIZE', 'outputAction': 'ANONYMIZE', 'inputEnabled': True, 'outputEnabled': True},
                {'type': 'EMAIL', 'action': 'ANONYMIZE', 'inputAction': 'ANONYMIZE', 'outputAction': 'ANONYMIZE', 'inputEnabled': True, 'outputEnabled': True},
                {'type': 'NAME', 'action': 'ANONYMIZE', 'inputAction': 'ANONYMIZE', 'outputAction': 'ANONYMIZE', 'inputEnabled': True, 'outputEnabled': True}
            ]
        },
        blockedInputMessaging="I can't provide information with PII data.",
        blockedOutputsMessaging="I can't generate content with PII data."
    )
    return response
```

---

## Tích hợp Quy trình Công việc Mã hóa Token

### 1. Áp dụng guardrails để phát hiện các PII entities

Sử dụng **API ApplyGuardrail** để xác thực văn bản đầu vào:

```python
def invoke_guardrail(user_query):
    #... (logic gọi API ApplyGuardrail)
    bedrock_runtime = boto3.client('bedrock-runtime')
    response = bedrock_runtime.apply_guardrail(
        guardrailIdentifier='your-guardrail-id', 
        guardrailVersion='your-guardrail-version', 
        source="INPUT",
        content=[{"text": {"text": user_query}}]
    )
    return response
```

### 2. Gọi dịch vụ mã hóa token

Phân tích cú pháp phản hồi từ Guardrail và gọi dịch vụ mã hóa token bên thứ ba (ví dụ: Thales CipherTrust) để chuyển đổi các giá trị nhạy cảm thành token.

```python
import requests
def thales_ciphertrust_tokenizer(guardrail_response):
    #... (logic trích xuất PII entities)
    #... (logic gọi điểm cuối /v1/protect của Thales CipherTrust)
    # Lấy sensitive_value và gọi API
    # response = requests.post(url_str, headers=headers, data=json.dumps(crdp_payload))
    # protected_results.append({...})
    #...
    pass
```

### 3. Thay thế mặt nạ guardrail bằng token

Thay thế các mặt nạ chung chung (`{EMAIL}`, `{URL}`, v.v.) trong đầu ra của guardrail bằng các token giữ nguyên định dạng đã được tạo.

```python
def process_guardrail_output(protected_results, guardrail_response):
    #... (logic thay thế)
    protection_map = {res['type'].upper(): res['protection_response']['protected_data'] for res in protected_results}
    modified_outputs = []
    for output_item in guardrail_response.get('outputs', []):
        if 'text' in output_item:
            modified_text = output_item['text']
            for pii_type, protected_value in protection_map.items():
                modified_text = modified_text.replace(f"{{{pii_type}}}", protected_value) # Thay thế {PII_TYPE} bằng token
            modified_outputs.append({"text": modified_text})
    return modified_outputs
```

**Ví dụ về Đầu vào được làm sạch:**
* **Đầu vào gốc:** `"Hi, this is john.smith@example.com. Based on my last five transactions on acme.com..."`
* **Đầu vào đã được mã hóa token (Sanitized):** `"Hi, this is 1001000GC5gDh1.D8eK71@EjaWV.lhC. Based on my last five transactions on 1001000WcFzawG.Jc9Tfc..."`

### 4. Xử lý Ứng dụng Hạ nguồn

Công cụ phân tích tài chính có thể gọi LLM với dữ liệu được mã hóa token để tạo ra phản hồi có liên quan. Sau đó, **dịch vụ xử lý phản hồi** sẽ gọi **Detokenize API** của dịch vụ mã hóa token (ví dụ: Thales CipherTrust) để **giải mã token** và khôi phục các giá trị gốc trước khi gửi phản hồi cuối cùng đến khách hàng.

---

## Dọn dẹp

Để tránh phát sinh phí, hãy xóa các tài nguyên AWS bạn đã tạo sau khi kiểm tra:

1.  Xóa guardrails bạn đã tạo (tham khảo mục Xóa guardrail của Amazon Bedrock).
2.  Xóa các tài nguyên AWS Lambda, API Gateway hoặc Step Functions nếu bạn đã triển khai quy trình công việc đầy đủ.
3.  Tham khảo tài liệu của giải pháp mã hóa token bên thứ ba (ví dụ: Thales CipherTrust) để gỡ bỏ chúng.

---

## Kết luận

Việc kết hợp **Amazon Bedrock Guardrails** với **mã hóa token** (ví dụ: Thales CipherTrust) cung cấp một giải pháp mạnh mẽ để bảo vệ **PII** trong các quy trình công việc AI tạo sinh, đồng thời giữ được **tính hữu dụng và khả năng đảo ngược dữ liệu** cần thiết cho các ứng dụng hạ nguồn được ủy quyền. Cách tiếp cận này giúp các tổ chức trong các ngành được quản lý chặt chẽ cân bằng giữa đổi mới AI tạo sinh và các yêu cầu tuân thủ.