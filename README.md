# Documentcodegit - Chatbot tương tác với Kho lưu trữ GitHub

## Giới thiệu

Documentcodegit là một dự án chatbot tương tác được thiết kế để tham gia các cuộc hội thoại về các kho lưu trữ GitHub bằng cách sử dụng Mô hình Ngôn ngữ Lớn (LLM). Nó cho phép người dùng có các cuộc thảo luận ý nghĩa, đặt câu hỏi và truy xuất thông tin liên quan từ kho lưu trữ GitHub.

 
## Mục lục  

- [Installation](#installation)
- [Usage](#usage)
- [Chatbot Functionality](#chatbot-functionality)



## Installation

Các bước cài đặt:

1. Create a virtual environment and activate on your local machine to isolate the project's dependencies.
   ```bash
   python -m venv repochat-env
   source repochat-env/bin/activate
   ```

2. Clone the Documentcodegit repository and navigate to the project directory.
   ```bash
   git clone https://github.com/Catopham1702/Document_code.git
   cd repochat
   ```

3. Install the required Python packages using `pip`.
   ```bash
   pip install -r requirements.txt
   ```

4. Install the "llama-cpp-python" library.
    ### Cài đặt với không có tăng tốc phần cứng
   ```bash
   pip install llama-cpp-python
   ```

   ### Cài đặt với tăng tốc phần cứng. Hướng dẫn tăng tốc cụ thể ở [TheBloke/phi-2-GGUF](https://huggingface.co/TheBloke/phi-2-GGUF)

    `llama.cpp` supports multiple BLAS backends for faster processing.

    To install with OpenBLAS, set the `LLAMA_BLAS and LLAMA_BLAS_VENDOR` environment variables before installing:

    ```bash
    CMAKE_ARGS="-DLLAMA_BLAS=ON -DLLAMA_BLAS_VENDOR=OpenBLAS" pip install llama-cpp-python
    ```

    To install with cuBLAS, set the `LLAMA_CUBLAS=1` environment variable before installing:

    ```bash
    CMAKE_ARGS="-DLLAMA_CUBLAS=on" pip install llama-cpp-python
    ```

    To install with CLBlast, set the `LLAMA_CLBLAST=1` environment variable before installing:

    ```bash
    CMAKE_ARGS="-DLLAMA_CLBLAST=on" pip install llama-cpp-python
    ```

    To install with Metal (MPS), set the `LLAMA_METAL=on` environment variable before installing:

    ```bash
    CMAKE_ARGS="-DLLAMA_METAL=on" pip install llama-cpp-python
    ```

    To install with hipBLAS / ROCm support for AMD cards, set the `LLAMA_HIPBLAS=on` environment variable before installing:

    ```bash
    CMAKE_ARGS="-DLLAMA_HIPBLAS=on" pip install llama-cpp-python
    ```

    To get to know more about Hardware Acceleration, refer to official README from [llama-cpp-python](https://github.com/abetlen/llama-cpp-python)

5. Tạo một thư mục có tên là `models` trong thư mục dự án.

6. Tải một Mô Hình Ngôn Ngữ từ Hugging Face Model Hub dựa trên khả năng của máy tính của bạn. Đề nghị sử dụng mô hình sau: [TheBloke/CodeLlama-7B-GGUF](https://huggingface.co/TheBloke/CodeLlama-7B-GGUF/blob/main/codellama-7b.Q4_K_M.gguf). Nếu bạn muốn tối ưu hóa một mô hình có sẵn trên Hugging Face, làm theo hướng dẫn từ [llama.cpp](https://github.com/ggerganov/llama.cpp)

7. Sao chép tệp mô hình đã tải vào thư mục "models".

8. Mở tệp `models.py` nằm trong thư mục "repochat" và thiết lập vị trí tệp mô hình trong hàm `code_llama()` như sau:
   ```python
   def code_llama():
       callbackmanager = CallbackManager([StreamingStdOutCallbackHandler()])
       llm = LlamaCpp(
           model_path="./models/codellama-7b.Q4_K_M.gguf",
           n_ctx=2048,
           max_tokens=200,
           n_gpu_layers=1,
           f16_kv=True,
           callback_manager=callbackmanager,
           verbose=True,
           use_mlock=True
       )
       return llm
   ```

## Usage

1. Mở terminal và chạy lệnh sau để khởi động ứng dụng Document_code:
   ```bash
   streamlit run app.py
   ```

2. Bây giờ bạn có thể nhập liên kết kho lưu trữ GitHub.

3. Documentcodegit sẽ truy xuất tất cả các tệp từ kho lưu trữ và lưu chúng trong một thư mục có tên "cloned_repo". Sau đó, nó sẽ chia các tệp thành các phần nhỏ hơn và tính toán embedding của chú [sentence-transformers/all-mpnet-base-v2](https://huggingface.co/sentence-transformers/all-mpnet-base-v2).

4. Các phần nhúng được lưu trữ cục bộ trong cơ sở dữ liệu vectơ có tên là ChromaDB.

## Chatbot Functionality

Documentcodegit cho phép bạn tham gia vào các cuộc trò chuyện với chatbot. Bạn có thể đặt câu hỏi hoặc cung cấp thông tin đầu vào và chatbot sẽ lấy các tài liệu liên quan từ cơ sở dữ liệu vectơ. Sau đó, nó sẽ gửi thông tin đầu vào của bạn cùng với các tài liệu được truy xuất đến mô hình ngôn ngữ để tạo phản hồi. Theo mặc định, tôi đã đặt mô hình thành "codellama-7b-instruct", nhưng bạn có thể thay đổi mô hình dựa trên tốc độ máy tính của mình và thậm chí bạn có thể thử mô hình lượng tử hóa 13b để biết phản hồi.

Chatbot lưu giữ bộ nhớ trong suốt cuộc trò chuyện để cung cấp các phản hồi phù hợp với ngữ cảnh.


