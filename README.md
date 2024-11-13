
# 📚 **KGPT Document Processing API** 

## 📋 **목차 (Table of Contents)**
1. [프로젝트 개요](#프로젝트-개요)
2. [사전 요구 사항](#사전-요구-사항)
3. [설치 및 실행 방법](#설치-및-실행-방법)
4. [API 설명](#api-설명)
   - [문서 임베딩 (Document Embedder)](#문서-임베딩-document-embedder)
   - [문서 삭제 (Document Delete)](#문서-삭제-document-delete)
   - [검색 및 메타데이터 검색 (Multiple Metadata)](#검색-및-메타데이터-검색-multiple-metadata)
5. [프로젝트 구조](#프로젝트-구조)
6. [기타 참고 사항](#기타-참고-사항)

---

## **프로젝트 개요**

이 프로젝트는 PDF, DOCX, PPT 등 다양한 문서 파일을 분석하고, 텍스트를 추출하여 Chroma 데이터베이스에 저장한 후, 해당 데이터를 사용하여 검색 및 질문 응답 기능을 제공하는 FastAPI 기반의 애플리케이션입니다. 

주요 기능:
- **문서 임베딩 및 캐싱**: 새로운 문서를 임베딩하여 Chroma 데이터베이스에 저장.
- **문서 삭제**: 특정 문서를 데이터베이스에서 삭제.
- **질문 기반 검색**: 여러 문서에서 검색된 텍스트를 바탕으로 질문에 대한 답변 제공.

---

## **사전 요구 사항**

### 필수 설치 프로그램
- **Python** (>= 3.11.5)
- **Jupyter 및 FastAPI 관련 라이브러리**
- **LibreOffice** (문서 변환용)
- **ChromaDB 및 LangChain** 관련 라이브러리
- **CUDA** (GPU 사용 시 필수)

### 필수 라이브러리 설치
```bash
pip install -r requirements.txt
```

`requirements.txt` 파일의 예시:
```
fastapi
uvicorn
pandas
numpy
pymupdf
fitz
PyPDF2
requests
python-dotenv
langchain
langchain-chroma
langchain-huggingface
langchain-openai
Chroma
nest_asyncio
```

---

## **설치 및 실행 방법**

### 1. **프로젝트 클론**
```bash
git clone <your-repository-url>
cd <your-repository-directory>
```

### 2. **환경 변수 설정**
`.env` 파일을 생성하고 아래와 같이 설정합니다:
```
MODEL_PATH="BAAI/bge-m3"
DB_PATH="Chroma_test"
COLLECTION_NAME="my_db"
DEVICE="cpu"  # GPU 사용 시 'cuda'
OPENAI_API_KEY="보유하고 있는 OPENAI 키를 입력해주세요."
```

### 3. **FastAPI 서버 실행**
```bash
python gen_fastapi.py
```
서버는 기본적으로 `http://localhost:5005`에서 실행됩니다.

---

## **API 설명**

### **문서 임베딩 (Document Embedder)**

**Endpoint**: `/api-kgpt/grpc/document-embedder`  
**Method**: `POST`

- **설명**: 문서를 임베딩하고 ChromaDB에 저장합니다.
- **요청 Body 예제**:
    ```json
    {
        "kgptFileSaveName": "생활폐기물_2022년.pdf",
        "kgptFileName": "생활폐기물_2022년.pdf",
        "category": "test_file/환경공단/..."
    }
    ```
- **응답 예제**:
    ```json
    {
        "documentResult": {
            "result": true,
            "status_code": "success",
            "description": "Document embedded successfully"
        }
    }
    ```

### **문서 삭제 (Document Delete)**

**Endpoint**: `/api-kgpt/grpc/document-delete`  
**Method**: `POST`

- **설명**: ChromaDB에서 특정 문서를 삭제합니다.
- **요청 Body 예제**:
    ```json
    {
        "kgptFileSaveName": "sample.pdf",
        "kgptFileName": "Sample Document",
        "category": "data"
    }
    ```
- **응답 예제**:
    ```json
    {
        "documentResult": {
            "result": true,
            "status_code": "success",
            "description": "Document vectors deleted successfully"
        }
    }
    ```

### **검색 및 메타데이터 검색 (Multiple Metadata)**

**Endpoint**: `/api-kgpt/grpc/multiple_metadata`  
**Method**: `POST`

- **설명**: 검색 쿼리를 기반으로 ChromaDB에서 관련 문서를 검색합니다.
- **요청 Body 예제**:
    ```json
    {
        "prompt": "인공지능의 정의는?",
        "categoryList": ["data/sample.pdf", "data/example.pdf"]
    }
    ```
- **응답 예제**:
    ```json
    [
        {
            "pageContent": "인공지능은 ...",
            "page": 3,
            "source": "sample.pdf 3page",
            "file_path": "data",
            "file_save_name": "sample.pdf",
            "file_name": "Sample Document",
            "chunk_number": 1
        }
    ]
    ```

---

## **프로젝트 구조**

```
project/
│
├── test_file                # 환경공단 문서  
    ├──환경공단
    ├── 건설
    └── ...
├── 환경공단_chroma_Markdown  # Chroma_DB
├── gen_fastapi.py           # FastAPI 애플리케이션 메인 파일
├── requirements.txt         # 필수 라이브러리 목록
├── .env                     # 환경 변수 파일
└── README.md                
```

---

## **기타 참고 사항**


---
