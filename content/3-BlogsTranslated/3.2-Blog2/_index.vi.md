---
title: "Blog 2"
date: 2025-09-23
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Trí tuệ Nhân tạo (AI): Chạy các agent AI nghiên cứu chuyên sâu trên Amazon Bedrock AgentCore

Amazon Bedrock Blog
Viết bởi **Vadim Omeltchenko**, **Eashan Kaushik**, **Mark Roy**, và **Shreyas Subramanian** | **23 Tháng 09, 2025** | Category: **Amazon Bedrock, Agents, Generative AI, Technical How-to**

---

Các AI Agent đang phát triển vượt ra ngoài những công cụ hỗ trợ cơ bản thực hiện một nhiệm vụ đơn lẻ để trở thành các hệ thống mạnh mẽ hơn có thể lập kế hoạch, phê bình và cộng tác với các Agent khác để giải quyết các vấn đề phức tạp. **Deep Agents** —một khuôn khổ làm việc (framework) mới được giới thiệu được xây dựng trên LangGraph—mang những khả năng này vào thực tế, cho phép các quy trình công việc đa tác nhân (multi-agent workflows) mô phỏng lại động lực đội nhóm trong thế giới thực.

Tuy nhiên, thách thức không chỉ là xây dựng các agent như vậy mà còn là chạy chúng một cách đáng tin cậy và an toàn trong **môi trường sản xuất (production)**. Đây là lúc **Amazon Bedrock AgentCore Runtime** phát huy tác dụng. Bằng cách cung cấp một môi trường **máy chủ phi tập trung (serverless)** an toàn, được xây dựng có mục đích cho các AI Agent và các công cụ, Runtime giúp triển khai Deep Agents ở quy mô doanh nghiệp mà không cần phải thực hiện các công việc quản lý cơ sở hạ tầng nặng nhọc.

Trong bài đăng này, chúng tôi sẽ trình bày cách triển khai Deep Agents trên AgentCore Runtime. Như được thể hiện trong sơ đồ dưới đây, AgentCore Runtime mở rộng quy mô bất kỳ agent nào và cung cấp **sự cách ly phiên (session isolation)** bằng cách cấp phát một máy ảo siêu nhỏ (microVM) mới cho mỗi phiên mới.


---

## Amazon Bedrock AgentCore là gì?

Amazon Bedrock AgentCore vừa **không phụ thuộc vào framework (framework-agnostic)** vừa **không phụ thuộc vào mô hình (model-agnostic)**, mang lại cho bạn sự linh hoạt để triển khai và vận hành các AI agent tiên tiến một cách an toàn và ở quy mô lớn. Cho dù bạn đang xây dựng với **Strands Agents**, **CrewAI**, **LangGraph**, **LlamaIndex** hay một framework khác—và chạy chúng trên mô hình ngôn ngữ lớn (LLM)—AgentCore đều cung cấp cơ sở hạ tầng để hỗ trợ chúng.

Các dịch vụ mô-đun của nó được xây dựng có mục đích cho các **khối lượng công việc tác nhân động**, với các công cụ để mở rộng khả năng agent và các biện pháp kiểm soát cần thiết cho việc sử dụng trong môi trường sản xuất. Bằng cách giảm bớt gánh nặng công việc nặng nhọc không tạo ra sự khác biệt trong việc xây dựng và quản lý cơ sở hạ tầng tác nhân chuyên biệt, AgentCore cho phép bạn mang theo framework và mô hình ưa thích của mình và triển khai mà không cần viết lại mã.

Amazon Bedrock AgentCore cung cấp một bộ khả năng toàn diện được thiết kế để chuyển đổi các **nguyên mẫu tác nhân cục bộ** thành các hệ thống sẵn sàng cho môi trường sản xuất, bao gồm:

* **Bộ nhớ ổn định (persistent memory)** để duy trì ngữ cảnh trong và giữa các cuộc hội thoại.
* Quyền truy cập vào các API hiện có bằng cách sử dụng **Giao thức Ngữ cảnh Mô hình (Model Context Protocol - MCP)**.
* Tích hợp liền mạch với các hệ thống xác thực của công ty.
* Các công cụ chuyên biệt cho việc duyệt web và thực thi mã.
* **Khả năng quan sát sâu (deep observability)** vào các quy trình lập luận của agent.

---

## Các khả năng cốt lõi của AgentCore Runtime

AgentCore Runtime cung cấp một môi trường lưu trữ **serverless** và an toàn, được thiết kế đặc biệt cho các khối lượng công việc dạng tác nhân. Nó đóng gói mã vào một **container nhẹ** với giao diện đơn giản, nhất quán, khiến nó phù hợp để chạy các agent, công cụ, máy chủ MCP hoặc các khối lượng công việc khác được hưởng lợi từ việc mở rộng quy mô liền mạch và quản lý danh tính tích hợp.

AgentCore Runtime cung cấp:

* Thời gian thực thi mở rộng lên tới **8 giờ** cho các tác vụ lập luận phức tạp.
* Xử lý các payload lớn cho **nội dung đa phương thức (multimodal content)**.
* Triển khai mô hình **định giá dựa trên mức tiêu thụ** chỉ tính phí trong quá trình xử lý đang hoạt động.
* Mỗi phiên người dùng chạy trong sự cách ly hoàn toàn bên trong các **máy ảo siêu nhỏ chuyên dụng (microVMs)**.

Runtime hoạt động với nhiều framework (ví dụ: LangGraph, CrewAI, Strands...) và nhiều nhà cung cấp mô hình nền tảng, đồng thời cung cấp xác thực tích hợp của công ty, khả năng quan sát tác nhân chuyên biệt, và quyền truy cập hợp nhất thông qua một SDK duy nhất.

---

## Ví dụ thực tế: Tích hợp Deep Agents

Trong bài đăng này, chúng ta sẽ triển khai ví dụ Deep Agents mới được phát hành trên AgentCore Runtime.

Ví dụ triển khai bao gồm:
1.  Một **research agent (tác nhân nghiên cứu)** thực hiện tìm kiếm internet chuyên sâu bằng cách sử dụng API Tavily.
2.  Một **critique agent (tác nhân phê bình)** xem xét và cung cấp phản hồi về các báo cáo đã được tạo.
3.  Một **main orchestrator (bộ điều phối chính)** quản lý quy trình công việc và xử lý các hoạt động của file.

Deep Agents sử dụng quản lý trạng thái của LangGraph để tạo ra một hệ thống Multi-Agent với:
* **Lập kế hoạch tác vụ tích hợp** thông qua công cụ `write_todos` giúp chia nhỏ các yêu cầu phức tạp.
* **Hệ thống tệp ảo** nơi các agent có thể đọc/ghi tệp để duy trì ngữ cảnh.
* **Kiến trúc tác nhân phụ** cho phép các agent chuyên biệt được gọi ra cho các tác vụ cụ thể trong khi vẫn duy trì sự cách ly ngữ cảnh.
* **Lập luận đệ quy** với giới hạn đệ quy cao (hơn 1.000) để xử lý các quy trình công việc phức tạp.

### Mã tích hợp

Vẻ đẹp nằm ở sự đơn giản của nó—chúng ta chỉ cần thêm một vài dòng mã để làm cho một agent tương thích với AgentCore:

```python
# 1. Import the AgentCore runtime
from bedrock_agentcore.runtime import BedrockAgentCoreApp

app = BedrockAgentCoreApp()

# 2. Decorate your agent function with @app.entrypoint
@app.entrypoint
async def langgraph_bedrock(payload):
    # Your existing agent logic remains unchanged
    user_input = payload.get("prompt")
    
    # Call your agent as before
    stream = agent.astream(
        {"messages": [HumanMessage(content=user_input)]},
        stream_mode="values"
    )
    
    # Stream responses back
    async for chunk in stream:
        yield(chunk)

# 3. Add the runtime starter at the bottom
if __name__ == "__main__":
    app.run()
```

Chỉ vậy thôi! Phần còn lại của mã—khởi tạo mô hình, tích hợp API và logic agent—vẫn giữ nguyên.

---

## Từng bước triển khai lên AgentCore Runtime

Hãy xem xét quy trình triển khai thực tế bằng cách sử dụng **AgentCore Starter ToolKit**.

### Điều kiện tiên quyết
* Python 3.10 trở lên.
* Thông tin xác thực AWS (AWS credentials) đã được cấu hình.
* Amazon Bedrock AgentCore SDK đã được cài đặt.

### Bước 1: Quyền IAM
Cần hai quyền AWS IAM:
1.  Vai trò cho nhà phát triển để tạo tài nguyên AgentCore.
2.  Vai trò thực thi (execution role) để agent chạy trong Runtime.

*Lưu ý: Vai trò thứ hai có thể được tự động tạo bởi AgentCore Starter Toolkit (`auto_create_execution_role=True`).*

### Bước 2: Thêm trình bao bọc (wrapper) vào agent
Thêm các đoạn import và decorator của AgentCore vào mã agent hiện có của bạn như ví dụ trên.

### Bước 3: Triển khai bằng bộ công cụ khởi động

```python
from bedrock_agentcore_starter_toolkit import Runtime

# Step 1: Configure
agentcore_runtime = Runtime()
config_response = agentcore_runtime.configure(
    entrypoint="hello.py", # contains the code we showed earlier in the post
    execution_role=role_arn, # or auto-create
    auto_create_ecr=True,
    requirements_file="requirements.txt",
    region="us-west-2",
    agent_name="deepagents-research"
)

# Step 2: Launch
launch_result = agentcore_runtime.launch()
print(f"Agent deployed! ARN: {launch_result['agent_arn']}")

# Step 3: Invoke
response = agentcore_runtime.invoke({
    "prompt": "Research the latest developments in quantum computing"
})
```

### Bước 4: Điều gì xảy ra đằng sau hậu trường
Khi bạn chạy quá trình triển khai, bộ công cụ khởi động tự động:
1.  Tạo Docker file tối ưu hóa.
2.  Xây dựng container với các dependencies từ `requirements.txt`.
3.  Tạo repository **Amazon ECR** và push hình ảnh lên.
4.  Triển khai lên AgentCore Runtime.
5.  Cấu hình mạng và **khả năng quan sát** với Amazon CloudWatch và AWS X-Ray.

Mỗi phiên mới được khởi chạy trong microVM AgentCore Runtime riêng biệt.

---

## Gọi Agent đã triển khai

**Tùy chọn 1: Sử dụng bộ công cụ khởi động**

```python
response = agentcore_runtime.invoke({
    "prompt": "Research the latest developments in quantum computing"
})
```

**Tùy chọn 2: Sử dụng trực tiếp boto3 SDK**

```python
import boto3
import json

agentcore_client = boto3.client('bedrock-agentcore', region_name='us-west-2')

response = agentcore_client.invoke_agent_runtime(
    agentRuntimeArn=agent_arn,
    qualifier="DEFAULT",
    payload=json.dumps({
        "prompt": "Analyze the impact of AI on healthcare in 2024"
    })
)

# Handle streaming response
for event in response['completion']:
    if 'chunk' in event:
        print(event['chunk']['bytes'].decode('utf-8'))
```

---

## Deep Agents đang hoạt động

Khi mã thực thi, agent chính sẽ điều phối (orchestrates) các agent phụ chuyên biệt. Trong trường hợp này, prompt của bộ điều phối đặt ra kế hoạch:

1.  Viết câu hỏi vào `question.txt`.
2.  Phân tán thành các lệnh gọi agent nghiên cứu sử dụng công cụ `internet_search`.
3.  Tổng hợp các phát hiện thành `final_report.md`.
4.  Gọi **critique-agent** để đánh giá các khoảng trống (gaps).
5.  Tùy chọn lặp lại (loop back) để nghiên cứu thêm.


*(Click vào bản vẽ để chạy video)*

---

## Dọn dẹp

Khi hoàn thành, hãy giải phóng AgentCore Runtime và repository container:

```python
agentcore_control_client = boto3.client(
    'bedrock-agentcore-control', region_name=region
)
ecr_client = boto3.client('ecr', region_name=region)

runtime_delete_response = agentcore_control_client.delete_agent_runtime(
    agentRuntimeId=launch_result.agent_id
)

response = ecr_client.delete_repository(
    repositoryName=launch_result.ecr_uri.split('/')[1],
    force=True
)
```

---

## Kết luận

Amazon Bedrock AgentCore đại diện cho một **sự thay đổi mô hình** trong cách triển khai các agent AI. Bằng cách trừu tượng hóa sự phức tạp của cơ sở hạ tầng, AgentCore cho phép các nhà phát triển tập trung vào logic agent thay vì quản lý quy trình triển khai.

Đặc biệt đối với các agent nghiên cứu chuyên sâu, AgentCore cung cấp các khả năng độc đáo:
* **Xử lý không đồng bộ:** Cho phép agent chạy dài (lên đến 8 giờ) mà không chặn phản hồi.
* **AgentCore Memory:** Duy trì ngữ cảnh điều tra phức tạp giữa các phiên.
* **AgentCore Gateway:** Mở rộng nghiên cứu bao gồm thông tin độc quyền từ doanh nghiệp qua các công cụ MCP.

**Sẵn sàng triển khai?**
1.  Cài đặt: `pip install bedrock-agentcore-starter-toolkit`
2.  Thử nghiệm: Triển khai mã của bạn theo hướng dẫn.

Kỷ nguyên của các agent AI sẵn sàng cho sản xuất đã đến.

---

### Về các tác giả

**Vadim Omeltchenko** là Sr. AI/ML Solutions Architect, người đam mê giúp khách hàng AWS đổi mới trên đám mây.

**Eashan Kaushik** là Specialist Solutions Architect AI/ML tại AWS, tập trung vào các giải pháp AI tạo sinh tiên tiến và lấy khách hàng làm trung tâm.

**Shreyas Subramanian** là Principal Data Scientist, chuyên giải quyết các thách thức kinh doanh bằng Machine Learning và tối ưu hóa quy mô lớn.

**Mark Roy** là Principal Machine Learning Architect cho AWS, dẫn đầu các nỗ lực kiến trúc giải pháp cho Amazon Bedrock.