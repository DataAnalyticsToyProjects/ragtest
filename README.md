## RAG Test

* Source: https://github.com/ollama/ollama/blob/main/examples/langchain-python-rag-document/main.py

* Better tutorial: https://python.langchain.com/v0.2/docs/tutorials/rag/


### Prerequistie: 

* Ollama installed and running


### test run

* 환경 : OS X, ollama installed, python 3.9.6 (installed with Brew)

  
```
# python3 -m venv .venv

# pip3 install -r requirements.txt

# python3 main.py
```

### Result

아래 라인에서 실제 답변이 생성된 근거를 확인할 수 있다. 

https://github.com/seungwooketi/ragtest/blob/e4c5a514033c6e62215c13af7da376a47a98f325/main.py#L72

```
    print(query)
    relavant_docs = retriever.invoke(query)

    print(relavant_docs)
```

실제 돌아온 답변은, 

```
Query: how much the Company pledged in December 2022?

how much the Company pledged in December 2022?

[Document(page_content='2023, as compared to $127 million for the six months ended June 30, 2022, and included the following components:', metadata={'source': '/var/folders/1k/lspjtgrx3vs91146w6m86bwh0000gp/T/tmpk7hvnz92/tmp.pdf'}),
Document(page_content='approximately $38 million in such outstanding construction commitments. As of June 30, 2023, we also had a total of', metadata={'source': '/var/folders/1k/lspjtgrx3vs91146w6m86bwh0000gp/T/tmpk7hvnz92/tmp.pdf'}),
Document(page_content='In December 2022, the Company pledged 8,467,347 of its common shares of IndiaCo, representing 14.7% of the securities issued by IndiaCo on a fully diluted basis, as collateral for IndiaCo to enter into a debenture trust deed to borrow up to INR 5.5 billion (approximately $67 million as of June 30, 2023). The Company has recognized this share pledge at a fair value of $0.3 million and is included as a component of other liabilities on the accompanying Condensed Consolidated Balance Sheets. See', metadata={'source': '/var/folders/1k/lspjtgrx3vs91146w6m86bwh0000gp/T/tmpk7hvnz92/tmp.pdf'}),
Document(page_content='compared to $36 million for the three months ended June 30, 2022, and included the following components:', metadata={'source': '/var/folders/1k/lspjtgrx3vs91146w6m86bwh0000gp/T/tmpk7hvnz92/tmp.pdf'})]

The Company pledged approximately 8,467,347 of its common shares of IndiaCo in December 2022.0.05266799999999705

```

위와 같다. 

## Remarks

### Retrieval QA
```
Query: what is the revenue of wework?

what is the revenue of wework?

[Document(page_content='financial condition and results of operations of WeWork Inc.', metadata={'source': '/var/folders/1k/lspjtgrx3vs91146w6m86bwh0000gp/T/tmpk7hvnz92/tmp.pdf'}),
Document(page_content='financial condition and results of operations of WeWork Inc.', metadata={'source': '/var/folders/1k/lspjtgrx3vs91146w6m86bwh0000gp/T/tmpk7hvnz92/tmp.pdf'}),
Document(page_content='assets of WeWork Inc.', metadata={'source': '/var/folders/1k/lspjtgrx3vs91146w6m86bwh0000gp/T/tmpk7hvnz92/tmp.pdf'}),
Document(page_content='WeWork Inc. (Standalone)\n\nOther Subsidiaries (Combined)\n\nEliminations\n\nWeWork Inc. Consolidated\n\nAssets\n\nCurrent assets:\n\nCash and cash equivalents\n\n$\n\n277 $\n\n— $\n\n10 $\n\n— $\n\n287\n\n109\n\n—\n\n—\n\n—\n\n109\n\nAccounts receivable and accrued revenue, net Prepaid expenses\n\n134\n\n1\n\n3\n\n—\n\n138\n\nOther current assets\n\n155\n\n—\n\n—\n\n—\n\n155\n\nTotal current assets\n\n675\n\n1\n\n13\n\n—\n\n689\n\nInvestments in and advances to/(from) consolidated subsidiaries Property and equipment, net\n\n(20) 4,391\n\n(3,667) —\n\n(3,521) —\n\n7,208 —', metadata={'source': '/var/folders/1k/lspjtgrx3vs91146w6m86bwh0000gp/T/tmpk7hvnz92/tmp.pdf'})]

I don't know. The provided financial information does not include revenue data for WeWork Inc. It only shows assets and eliminations. To determine revenue, you would need to access a different part of WeWork's financial statements or reports that includes income statement data.0.06847400000000192
```

위와 같은 답변이 오는 것 볼 수 있는 데, 재미있는 점이 몇 가지 있다. 

* I don't know 라는 답변이 돌아온다. 이 때 그냥 모른다 정도가 아니라, **당신이 준 문서에는 그 정보가 없다고 명확하고 구체적으로 얘기한다. **

* 아마도 langchain의 RetrievalQA 체인이 이 기능을 하는 것 같다. 즉, langchain에서는 명시적으로 retrieval을 기반으로 한 질의 응답 기능을 제공하고 있다.

https://github.com/seungwooketi/ragtest/blob/e4c5a514033c6e62215c13af7da376a47a98f325/main.py#L8

### Chroma

위의 답변을 확인해 보면, Chroma가 생성된 벡터스토어는 임시 폴더 (/var/folders/1k/...)에 있음을 확인할 수 있다. 
