# FW_Graph

## preprocess
각 ip의 빈도수를 기반으로 해당 ip가 내부에 있는지 외부에 있는지를 결정합니다. 이때, country가 KR이거나 None인 ip만 내부 ip로 분류되도록 합니다. 

## grouping
inbound, outbound를 구분한 데이터셋으로 ip를 hashing하여 bitset을 만듭니다. 만들어진 bitset으로 jaccard distance matrix를 계산한 후, 계층적 클러스터링의 기법중 하나인 single linkage clustering을 진행합니다. 그 결과 총 70개의 group이 생성되고 이를 기존 데이터셋에 추가하여 저장합니다. 

## pagerank
생성된 그룹마다 ip를 기준으로 pagerank 알고리즘을 적용합니다. 그 후, 계산한 pagerank score를 통해 이상치를 탐지하였습니다. 이상탐지 기법으로는 전체 데이터 평균을 기준으로 ± 6sigma 범위 내에 있는 데이터를 정상, 범위 밖에 있는 데이터를 이상으로 판단하는 6-sigma를 사용하였습니다. 
마지막으로 group 번호와 ip를 key로 하여 이상탐지 라벨링을 완료합니다. 

## drawgraph
실제로 pagerank score와 이상치의 분포를 확인하기 위해 히스토그램을 그렸습니다. 
히스토그램 그래프에서 파랑막대는 정상 ip의 pagerank score 분포를 나타내고, 빨강막대는 이상 ip의 pagerank score 분포를 나타냅니다. 이상탐지의 기준인 6-sigma 범위는 회색 점선막대로 표시하였습니다. 
Group 45, 47의 그래프 결과를 보시면 pagerank score가 돋보이게 높은 ip를 이상 ip로 분류하는 모습을 쉽게 확인할 수 있습니다. 