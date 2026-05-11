# Final Project

## 파일 구조
```
final_project/
├── .gitignore
├── README.md
├── requirements.txt
│
├── data/                # 데이터 폴더 
│   ├── raw/             # 원본 
│   ├── processed/       # 전처리된 데이터
│   └── external/        # 외부 데이터
│
├── src/                 # 모듈화된 .py 코드
│   ├── __init__.py
│   ├── crawler.py
│   ├── preprocess.py
│   └── nli_pipeline.py
│
├── notebooks/           # Jupyter 노트북
│   └── exploration.ipynb
│
├── models/              # 학습된 모델 가중치 (gitignore)
│
└── outputs/             # 결과물, 시각화
    └── figures/
```