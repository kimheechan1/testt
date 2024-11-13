📖 README: FastAPI 기반 ChromaDB RAG 응용 프로그램
목차
프로젝트 개요
기능
설치 및 실행
필수 요구사항
설치 절차
실행 방법
API 설명
1. 문서 임베딩 엔드포인트
2. 문서 삭제 엔드포인트
3. 멀티플 메타데이터 검색 엔드포인트
디렉토리 구조
로깅 시스템
기술 스택
문제 해결
참고 자료
(-)
프로젝트 개요
이 프로젝트는 FastAPI, ChromaDB, LangChain, 및 HuggingFaceEmbeddings를 활용하여 질문-응답 시스템을 구축한 것입니다. 다양한 형식의 문서(PDF, DOCX, PPTX 등)를 파싱 및 청킹하여 ChromaDB에 저장하고, 이를 통해 효율적인 검색을 지원합니다. 검색된 컨텍스트를 바탕으로 GPT-4.0 기반 모델을 사용하여 질문에 답변을 제공합니다.

기능
문서 파싱 및 청킹: 다양한 파일 형식의 문서(PDF, DOCX, PPTX, TXT)를 읽어 텍스트로 변환하고 청크 단위로 나눔.
ChromaDB에 임베딩 저장 및 검색: 청크된 텍스트를 HuggingFaceEmbeddings 모델로 임베딩하여 ChromaDB에 저장하고 검색.
질문-응답 시스템: 검색된 컨텍스트를 기반으로 GPT-4.0을 통해 질문에 대한 답변 생성.
문서 삭제: 기존에 저장된 임베딩을 삭제하여 데이터 관리.
설치 및 실행
필수 요구사항
Python 3.11 이상
Anaconda (가상 환경 설정 권장)
Jupyter (추가 분석 및 테스트용)
LibreOffice (DOCX, PPTX 파일을 PDF로 변환하기 위함)
설치 절차
프로젝트 클론

git clone <repository-url>
cd <repository-folder>
가상 환경 설정 및 활성화

conda create -n myenv python=3.11
conda activate myenv
필수 패키지 설치

pip install -r requirements.txt
.env 파일 설정 .env 파일을 프로젝트 루트에 생성하고 필요한 환경 변수 설정:

OPENAI_API_KEY=your_openai_key
실행 방법
애플리케이션 실행

uvicorn RAG:app --host 0.0.0.0 --port 5005 --reload
API 문서 접근 브라우저에서 http://localhost:5005/docs를 열어 Swagger UI로 API 테스트 가능.

API 설명
1. 문서 임베딩 엔드포인트
URL: /api-kgpt/grpc/document-embedder
메서드: POST
기능: 문서를 임베딩하여 ChromaDB에 저장합니다.
요청 형식:
{
  "kgptFileSaveName": "example.pdf",
  "kgptFileName": "example_file",
  "category": "data_folder"
}
응답 예시:
{
  "documentResult": {
    "result": true,
    "status_code": "success",
    "description": "Document embedded successfully"
  }
}
2. 문서 삭제 엔드포인트
URL: /api-kgpt/grpc/document-delete
메서드: POST
기능: ChromaDB에서 특정 문서의 임베딩을 삭제합니다.
요청 형식:
{
  "kgptFileSaveName": "example.pdf",
  "kgptFileName": "example_file",
  "category": "data_folder"
}
응답 예시:
{
  "documentResult": {
    "result": true,
    "status_code": "success",
    "description": "Document vectors deleted successfully"
  }
}
3. 멀티플 메타데이터 검색 엔드포인트
URL: /api-kgpt/grpc/multiple_metadata
메서드: POST
기능: 프롬프트에 따라 ChromaDB에서 관련 문서를 검색하고 질문에 답변을 생성합니다.
요청 형식:
{
  "prompt": "질문 내용",
  "categoryList": ["document1", "document2"]
}
응답 예시:
[
  {
    "pageContent": "검색된 문서 내용",
    "source": "document1.pdf 3page",
    "file_path": "data_folder",
    "file_save_name": "example.pdf",
    "file_name": "example_file",
    "chunk_number": 2
  }
]
디렉토리 구조
project-root/
│
├── RAG.py                 # FastAPI 애플리케이션 메인 파일
├── requirements.txt       # 필요한 Python 패키지 목록
├── .env                   # 환경 변수 설정 파일
├── data_<rag_name>/       # 문서 저장 디렉토리
├── logs/                  # 로그 파일 디렉토리
└── sbl_api.log            # 로그 파일
로깅 시스템
파일 핸들러와 콘솔 핸들러를 사용하여 로깅 설정.
로그 파일은 sbl_api.log에 기록되며 최대 100MB 크기로 제한, 최대 2개의 백업 파일 유지.
로그 메시지는 호출 파일명과 라인 번호를 포함하여 기록.
기술 스택
FastAPI: 웹 서버 및 API 엔드포인트 구축
ChromaDB: 임베딩 데이터베이스 관리
LangChain: 검색 및 LLM 기반 질문-응답 시스템
HuggingFaceEmbeddings: 텍스트 임베딩 생성
PyMuPDF, PyPDF2: PDF 파일 텍스트 추출
LibreOffice: DOCX, PPTX 파일을 PDF로 변환
문제 해결
PDF 파일 파싱 문제: fitz와 convert_single_pdf 함수를 사용하여 PDF에서 텍스트를 정확히 추출.
임베딩 성능: MMR 기반 검색을 사용하여 검색 결과의 다양성 향상.
참고 자료
FastAPI Documentation
ChromaDB Documentation
LangChain Documentation
Hugging Face Embeddings
