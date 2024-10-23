# 사업체 수준 AIOE 계산

### 기본 수식

사업체 수준의 AIOE는 다음 수식으로 계산할 수 있습니다(Acemoglu 2020; Eisfeldt, Schubert and Zhang 2023).

$AIOE_i = \sum_{j} AIOE_{ij} \times OCC_{ij}$

여기서:
- $AIOE_i$: 사업체 i의 AI 노출도
- $AIOE_{ij}$: 직종 j의 AI 노출도
- $OCC_{ij}$: 직종 j의 구성비

따라서, 사업체별 AIOE 계산을 위해서는 두 가지 요소가 필요합니다.

1. 사업체별 직종구성비 ($OCC_{ij}$)
2. 직종별 AIOE ($\overline{AIOE_j}$)

### 문제

- 한국사업체패널조사자료(WPS)에서 128개 직종의 구성비를 모두 제공하지 않습니다. 
- 자료에서 제공하는 것은 관리직, 전문직(기술직 포함), 사무직, 서비스직, 판매직, 생산직, 단순직 7개 직종의 직종 구성비 뿐입니다. 

## 해결법 1. 대분류 직종 AIOE 평균을 이용한 단순 병합

직종비를 알고 있는 7개 직종의 인공지능직업노출도 대표값을 이용하면 인공지능직업노출도 정보를 사업체 패널 자료에 연결할 수 있습니다.  

이 경우 사업체 i 전체의 인공지능직업노출도는 다음과 같이 계산됩니다:

$AIOE_i = \sum_{j=1}^7 \overline{AIOE_j} \times OCC_{ij}$

이는 어떤 업종에서도 관리직, 전문직, 사무직, 서비스직, 생산직, 단순직의 인공지능 직업노출도가 동일하다고 가정하는 것과 같습니다. 

### 1. 직종 구성비 계산

먼저, 직종 구성비는 다음과 같이 계산됩니다:

$OCC_{ij} = \frac{L_{ij}}{L_i}$

여기서:
- $L_{ij}$: 기업 i의 직종 j 고용
- $L_i$: 기업 i의 전체 고용

#### 데이터 처리

한국사업체패널조사자료(WPS)에서는 각 직종의 근로자 수를 변수로 제공합니다:
- 관리직: epq3001
- 전문직: epq3002
- ...
- 총 근로자 수: epq3008

이를 통해 각 사업체의 직종별 구성비를 계산하고, AIOE 대표값과 곱하여 사업체별 AIOE를 산출할 수 있습니다:

$AIOE_i = \sum_{j=1}^7 AIOE_j \times \frac{epq300j}{epq3008}$

#### 사업체 패널 데이터의 직종별 구성비

| 직종 구분 | 평균 구성비(%) |
|-----------|----------------|
| 관리직 | 10.99 |
| 전문직 | 14.89 |
| 사무직 | 20.59 |
| 서비스직 | 8.05 |
| 판매직 | 4.80 |
| 생산직 | 29.65 |
| 단순직 | 10.92 |


### 2. 직종별 AIOE 계산

대분류 직종의 AIOE 평균을 구하면 사업체별 AIOE를 계산할 수 있습니다. 

7개 대분류 직종의 AIOE 평균값 ($\overline{AIOE_j}$)는 아래 그림에서처럼 소분류 직종 세 자리 수준에서 생성한 직종별 인공지능직업노출도 점수를 대분류 직종 한 자리 수준에서 집계하여 구할 수 있습니다.

#### 직종별 AI 노출도 원자료의 사업체 패널 병합 개요

<table style="width: 100%; border-collapse: collapse;">
<tr>
    <th colspan="1" style="text-align: center; border: 1px solid black; padding: 10px;">직종별 수준</th>
    <th style="text-align: center; border: none; padding: 10px;"></th>
    <th colspan="1" style="text-align: center; border: 1px solid black; padding: 10px;">사업체 수준</th>
</tr>
<tr>
    <th style="text-align: center; border: 1px solid black; padding: 10px;">한국표준직업분류(소분류)</th>
    <th style="text-align: center; border: none; padding: 10px;"></th>
    <th style="text-align: center; border: 1px solid black; padding: 10px;">근로자직종정보(대분류)</th>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 10px;">111 의원·고위 공무원 및 공공단체 임원<br>...<br>139 기타 전문 서비스 관리자</td>
    <td style="text-align: center; padding: 10px;">→</td>
    <td style="border: 1px solid black; padding: 10px;">1 관리직</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 10px;">211 생명 및 자연과학 관련 전문가<br>...<br>288 문화·예술 관련 기획자 및 매니저</td>
    <td style="text-align: center; padding: 10px;">→</td>
    <td style="border: 1px solid black; padding: 10px;">2 전문직 (기술직 포함)</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 10px;">311 행정 사무직<br>...<br>399 고객 상담 및 기타 사무원</td>
    <td style="text-align: center; padding: 10px;">→</td>
    <td style="border: 1px solid black; padding: 10px;">3 사무직</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 10px;">411 경찰·소방 및 교도 관련 종사자<br>...<br>442 식음료 서비스 종사자</td>
    <td style="text-align: center; padding: 10px;">→</td>
    <td style="border: 1px solid black; padding: 10px;">4 서비스직</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 10px;">510 영업 종사자<br>...<br>532 방문 및 노점 판매 관련직</td>
    <td style="text-align: center; padding: 10px;">→</td>
    <td style="border: 1px solid black; padding: 10px;">5 판매직</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 10px;">710 식품가공 관련 기능 종사자<br>...<br>899 기타 기계 조작원</td>
    <td style="text-align: center; padding: 10px;">→</td>
    <td style="border: 1px solid black; padding: 10px;">6 생산직</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 10px;">910 건설 및 광업 단순 종사자<br>...<br>999 기타 서비스 관련 단순 종사자</td>
    <td style="text-align: center; padding: 10px;">→</td>
    <td style="border: 1px solid black; padding: 10px;">7 단순직</td>
</tr>
<tr>
    <td style="text-align: center; padding: 10px; border: none;">⟨직종별 AIOE 원자료⟩</td>
    <td></td>
    <td style="text-align: center; padding: 10px; border: none;">⟨사업체 패널⟩</td>
</tr>
</table>

#### 직종별 AIOE 계산 과정

1. 대분류별 AIOE 평균값 ($\overline{AIOE_j}$) 계산:

$\overline{AIOE_j} = \frac{1}{n_j}\sum_{k \in j} AIOE_k$

여기서:
- $j$: 7개 대분류 직종
- $k$: 각 대분류에 속한 소분류 직종
- $n_j$: 대분류 $j$에 속한 소분류 직종의 수

#### 직종별 AIOE 대표값

| 직종 | AIOE |
|------|-------|
| 관리직 | 0.618367 |
| 전문직 | 0.621769 |
| 사무직 | 0.636765 |
| 서비스직 | 0.578409 |
| 판매직 | 0.617983 |
| 생산직 | 0.558010 |
| 단순직 | 0.564768 |

- Felten 방식의 AI 노출도는 인지 노동의 노출도가 높게 측정되는 경향이 있습니다.
- 사무직의 AIOE가 가장 높고, 관리직, 전문직, 판매직이 그 다음으로 높습니다.
- 생산직, 서비스직, 단순직은 상대적으로 낮은 AIOE를 보입니다.

## 사업체별 AIOE 계산 (단순 병합)

각 사업체의 직종별 구성비와 직종별 AIOE 평균을 알았으므로, 다음 식을 계산할 수 있습니다. 

$AIOE_i = \sum_{j=1}^7 \overline{AIOE_j} \times OCC_{ij}$

계산을 위해서는 $\overline{AIOE_j}$에 위에서 구한 직종별 AIOE 대표값을 대입하면 됩니다. 

### 구체적인 계산 예시

특정 사업체의 직종 구성이 다음과 같다고 가정합시다. 

```python
example_firm = {
    'management_ratio': 0.15,    # 관리직
    'professional_ratio': 0.20,  # 전문직
    'office_ratio': 0.30,       # 사무직
    'service_ratio': 0.10,      # 서비스직
    'sales_ratio': 0.05,        # 판매직
    'production_ratio': 0.15,   # 생산직
    'simple_ratio': 0.05        # 단순직
}
```

이 사업체의 AIOE는 다음과 같이 계산됩니다:

$AIOE_i$ 
<br>$= (0.618367 \times 0.15) + (0.621769 \times 0.20) + (0.636765 \times 0.30) + (0.578409 \times 0.10)+ (0.617983 \times 0.05)+ (0.558010 \times 0.15) + (0.564768 \times 0.05)$
<br>$= 0.092755 + 0.124354 + 0.191030 + 0.057841 + 0.030899 + 0.083702 + 0.028238$
<br>$= 0.608819$

## 해결법 1의 한계

### 단순 병합의 한계와 업종별 차이

그러나, 본 연구에서는 몇 가지 이유에서 이러한 단순 병합 방법을 보다 정교화하였습니다. 

가장 큰 이유는 이 방법이 직종 내에서 나타나는 업종 간 차이를 무시하므로 값의 변이가 지나치게 감소하기 때문입니다. 예를 들어 아래는 업종별 관리직의 AIOE 평균을 나타낸 표입니다. 

#### 업종별 관리직 AIOE 평균 분포

| 업종 | 평균 AIOE |
|------|-----------|
| 공공 행정, 국방 및 사회보장 행정 | 0.657 |
| 우편 및 통신업 | 0.650 |
| 컴퓨터 프로그래밍, 시스템 통합 및 관리업 | 0.650 |
| 개인 및 소비용품 수리업 | 0.650 |
| 금융 및 보험관련 서비스업 | 0.648 |
| 금융업 | 0.648 |
| 보험 및 연금업 | 0.648 |
| 출판업 | 0.647 |
| 전문서비스업 | 0.643 |
| 기타 전문, 과학 및 기술 서비스업 | 0.640 |
| 사업시설 관리 및 조경 서비스업 | 0.600 |
| ... | ... |
| 전문직별 공사업 | 0.597 |
| 종합 건설업 | 0.594 |
| 화학물질 및 화학제품 제조업; 의약품 제외 | 0.594 |
| 기타 기계 및 장비 제조업 | 0.592 |
| 담배 제조업 | 0.591 |
| 인쇄 및 기록매체 복제업 | 0.591 |
| 비금속 광물제품 제조업 | 0.591 |
| 1차 금속 제조업 | 0.591 |
| 기타 운송장비 제조업 | 0.591 |
| 수도사업 | 0.591 |
| 하수, 폐수 및 분뇨 처리업 | 0.585 |

표를 보면 같은 관리직 안에서도 업종마다 AIOE 값이 크게 차이난다는 사실을 알 수 있습니다.  
- 업종마다 관리직의 AIOE 평균은 최소 0.585에서 최대 0.657까지 분포
- 단순 병합 방법은 이를 0.618 단일값으로 평균화해서 사업체별 AIOE 노출도를 계산
- 따라서, 직종 내의 업종 간 차이를 충분히 반영할 수 없음

#### 업종별 관리직 소분류 구성과 AIOE

한국노동패널조사에서 나타나는 관리직의 직업 소분류 빈도를 업종별로 살펴보면, 같은 직종 안에서도 업종별로 왜 이러한 차이가 나타나는지를 알 수 있습니다. 

<table>
<tr><th>업종</th><th>직업 소분류</th><th>빈도</th><th>AIOE</th></tr>
<tr>
<td rowspan="4">의복, 의복 액세서리 및<br>모피제품 제조업</td>
<td>121 행정 및 경영 지원 관리자</td><td>9</td><td>0.646</td>
</tr>
<tr><td>141 건설, 전기 및 생산 관련 관리자</td><td>6</td><td>0.590</td></tr>
<tr><td>151 판매 및 운송 관리자</td><td>13</td><td>0.633</td></tr>
<tr><td><i>총계</i></td><td>28</td><td>0.627</td></tr>
<tr>
<td rowspan="4">보험 및 연금업</td>
<td>112 기업 고위 임원</td><td>4</td><td>0.649</td>
</tr>
<tr><td>121 행정 및 경영 지원 관리자</td><td>7</td><td>0.646</td></tr>
<tr><td>132 보험 및 금융 관리자</td><td>52</td><td>0.648</td></tr>
<tr><td><i>총계</i></td><td>63</td><td>0.647</td></tr>
<tr>
<td rowspan="3">사업 시설 관리 및<br>조경 서비스업</td>
<td>139 기타 전문 서비스 관리자</td><td>6</td><td>0.639</td>
</tr>
<tr><td>153 환경, 청소 및 경비 관련 관리자</td><td>15</td><td>0.584</td></tr>
<tr><td><i>총계</i></td><td>21</td><td>0.599</td></tr>
<tr>
<td rowspan="2">컴퓨터 프로그래밍,<br>시스템 통합 및 관리업</td>
<td>135 정보 통신 관련 관리자</td><td>8</td><td>0.649</td>
</tr>
<tr><td><i>총계</i></td><td>8</td><td>0.649</td></tr>
</table>

*Note.업종별 '총계' 항목은 소분류 직업의 표본 빈도를 반영하여 계산한 업종별 인공지능직업노출도의 평균입니다.*

표를 보면 같은 관리직 대분류에 속하고 있더라도 업종마다 종사하고 있는 관리 직종의 직업 소분류 목록 구성은 무척 상이합니다. 

즉, 직종 내 업종 간 인공지능직업노출도 평균 차이는 주로 **업종-대분류 직종 교차셀에 포함되는 소분류 직업의 목록 구성 차이**에 의해 나타난 것이라고 할 수 있습니다. 

다음 문서에서는 이러한 업종별 차이를 반영하기 위한 **해결법 2**를 검토합니다. 
