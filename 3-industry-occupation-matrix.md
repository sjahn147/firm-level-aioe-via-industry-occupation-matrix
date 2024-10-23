## 해결법 2 : 업종-직종 매트릭스를 이용한 매칭

아래에서는 앞선 문서에서 확인한 업종별 소분류 직업 구성 차이를 반영하기 위한 방법을 검토합니다. 

### 해결법 1의 문제

- AI노출도 정보를 단순 병합 방법으로 사업체패널조사자료에 연결하면 척도의 변이가 크게 감소합니다. 
- 이는 모든 업종에서 직종별 AIOE가 동일하다고 가정함에 따라 동일 직종 내에서 나타나는 업종 간 차이가 모두 제거되었기 때문입니다. 

### 해결법 2의 대안

- 이 문서에서는 단순 병합으로 인한 정보 손실을 최소화하기 위해, 업종마다 대분류 직종의 집계된 평균(aggregate mean)을 각각 구합니다. 
- 업종별로 각 직종의 AIOE 대표값인 $\overline{AIOE_{ij}}$ 을 계산한 뒤, 사업체패널조사자료에서 이 값을 업종마다 각 사업체의 직종별 근로자수 정보에 연결합니다.

$\forall i: \text{AIOE} \quad \{i\} = \sum_{j=1}^{7} \sum_{k=1}^{64} (s_{ij} \times \overline{\text{AIOE}_{jk}})$

### 해결법 2의 문제

- AI 직업노출도는 직종 수준에서 계산되는 자료이므로 사업체패널자료의 업종 정보인 한국표준산업분류(KSIC)와 연계된 정보는 원자료에 존재하지 않습니다. 
- 업종별로 포함되는 소분류 직종들의 분포를 선험적으로 알 수도 없습니다. 예를 들어, 아래의 표는 “컴퓨터 프로그래밍, 시스템 통합 및 관리업” 업종 안에서도 다양한 직종이 포함되어 있음을 보여줍니다 : 

#### 표 1. 컴퓨터 프로그래밍, 시스템 통합 및 관리업의 직종
(원자료: 한국노동패널조사)

<table style="border-collapse: collapse; width: 100%;">
<tr>
    <th style="border: 1px solid black; padding: 8px;">직종</th>
    <th style="border: 1px solid black; padding: 8px;">직업 소분류</th>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">관리직</td>
    <td style="border: 1px solid black; padding: 8px;">135 정보 통신 관련 관리자</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">전문직</td>
    <td style="border: 1px solid black; padding: 8px;">221 컴퓨터 하드웨어 및 통신공학 전문가</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">전문직</td>
    <td style="border: 1px solid black; padding: 8px;">222 컴퓨터 시스템 및 소프트웨어 전문가</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">전문직</td>
    <td style="border: 1px solid black; padding: 8px;">223 데이터 및 네트워크 관련 전문가</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">전문직</td>
    <td style="border: 1px solid black; padding: 8px;">224 정보 시스템 및 웹 운영자</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">전문직</td>
    <td style="border: 1px solid black; padding: 8px;">274 감정·기술 영업 및 중개 관련 종사자</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">전문직</td>
    <td style="border: 1px solid black; padding: 8px;">285 디자이너</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">사무직</td>
    <td style="border: 1px solid black; padding: 8px;">312 경영 관련 사무원</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">사무직</td>
    <td style="border: 1px solid black; padding: 8px;">313 회계 및 경리 사무원</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">사무직</td>
    <td style="border: 1px solid black; padding: 8px;">399 고객 상담 및 기타 사무원</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">서비스직</td>
    <td style="border: 1px solid black; padding: 8px;">431 운송 서비스 종사자</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">생산직</td>
    <td style="border: 1px solid black; padding: 8px;">761 전기·전자기기 설치 및 수리원</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">생산직</td>
    <td style="border: 1px solid black; padding: 8px;">771 정보 통신기기 설치 및 수리원</td>
</tr>
</table>

이처럼 직업 소분류의 이름만을 보고 직종의 업종 포함 여부를 결정하는 것은 지나치게 임의적일 우려가 있습니다. 

## 구현 방법

- **목표** : 업종-직종 교차셀에 포함되는 직종들의 인공지능직업노출도 대표값을 계산해야합니다.
- **문제** : 각 셀에 어떤 직종들이 포함되는지 알 수 없습니다.  
- **해결** : 업종별로 포함되는 소분류 직종들의 분포를 알기 위한 실제 표본으로 한국노동패널조사를 활용합니다. 

### 프로세스

한국노동패널자료에서 업종마다 포함되는 직종들의 목록을 추출한 뒤, 해당 직종들의 AIOE 값의 평균을 계산합니다. 

예를 들어,  창고 및 운송관련 서비스 업종에서의 AI 노출도는 다음과 같이 계산됩니다. 

#### 표 2. 창고 및 운송관련 서비스업의 직종별 AIOE값
<table style="border-collapse: collapse; width: 100%;">
<tr>
    <th style="border: 1px solid black; padding: 8px;">대분류명</th>
    <th style="border: 1px solid black; padding: 8px;">소분류명</th>
    <th style="border: 1px solid black; padding: 8px;">AIOE</th>
    <th style="border: 1px solid black; padding: 8px;">셀 평균</th>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;" rowspan="1">관리직</td>
    <td style="border: 1px solid black; padding: 8px;">151 판매 및 운송 관리자</td>
    <td style="border: 1px solid black; padding: 8px;">0.6333</td>
    <td style="border: 1px solid black; padding: 8px;" rowspan="1">0.6333</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;" rowspan="8">전문직</td>
    <td style="border: 1px solid black; padding: 8px;">224 정보 시스템 및 웹 운영자</td>
    <td style="border: 1px solid black; padding: 8px;">0.6296</td>
    <td style="border: 1px solid black; padding: 8px;" rowspan="8">0.6209</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">234 전기·전자공학 기술자 및 시험원</td>
    <td style="border: 1px solid black; padding: 8px;">0.6123</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">236 소방·방재 기술자 및 안전 관리원</td>
    <td style="border: 1px solid black; padding: 8px;">0.6170</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">238 항공기·선박 기관사 및 관제사</td>
    <td style="border: 1px solid black; padding: 8px;">0.6052</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">271 인사 및 경영 전문가</td>
    <td style="border: 1px solid black; padding: 8px;">0.6417</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">274 감정·기술 영업 및 중개 관련 종사자</td>
    <td style="border: 1px solid black; padding: 8px;">0.6250</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">285 디자이너</td>
    <td style="border: 1px solid black; padding: 8px;">0.6158</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">311 행정 사무원</td>
    <td style="border: 1px solid black; padding: 8px;">0.6400</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;" rowspan="7">사무직</td>
    <td style="border: 1px solid black; padding: 8px;">312 경영 관련 사무원</td>
    <td style="border: 1px solid black; padding: 8px;">0.6331</td>
    <td style="border: 1px solid black; padding: 8px;" rowspan="7">0.6407</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">313 회계 및 경리 사무원</td>
    <td style="border: 1px solid black; padding: 8px;">0.6435</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">314 비서 및 사무 보조원</td>
    <td style="border: 1px solid black; padding: 8px;">0.6450</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">330 법률 및 감사 사무 종사자</td>
    <td style="border: 1px solid black; padding: 8px;">0.6502</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">392 여행·안내 및 접수 사무원</td>
    <td style="border: 1px solid black; padding: 8px;">0.6357</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">399 고객 상담 및 기타 사무원</td>
    <td style="border: 1px solid black; padding: 8px;">0.6375</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">412 경호 및 보안 관련 종사자</td>
    <td style="border: 1px solid black; padding: 8px;">0.5708</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;" rowspan="2">서비스직</td>
    <td style="border: 1px solid black; padding: 8px;">431 운송 서비스 종사자</td>
    <td style="border: 1px solid black; padding: 8px;">0.5883</td>
    <td style="border: 1px solid black; padding: 8px;" rowspan="2">0.5796</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">510 영업 종사자</td>
    <td style="border: 1px solid black; padding: 8px;">0.6386</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;" rowspan="3">판매직</td>
    <td style="border: 1px solid black; padding: 8px;">521 매장 판매 종사자</td>
    <td style="border: 1px solid black; padding: 8px;">0.6112</td>
    <td style="border: 1px solid black; padding: 8px;" rowspan="3">0.6281</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">531 통신 관련 판매직</td>
    <td style="border: 1px solid black; padding: 8px;">0.6346</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">743 용접원</td>
    <td style="border: 1px solid black; padding: 8px;">0.5561</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;" rowspan="11">생산직</td>
    <td style="border: 1px solid black; padding: 8px;">752 운송장비 정비원</td>
    <td style="border: 1px solid black; padding: 8px;">0.5752</td>
    <td style="border: 1px solid black; padding: 8px;" rowspan="11">0.5708</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">753 기계장비 설치 및 정비원</td>
    <td style="border: 1px solid black; padding: 8px;">0.5585</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">761 전기·전자기기 설치 및 수리원</td>
    <td style="border: 1px solid black; padding: 8px;">0.5784</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">762 전기공</td>
    <td style="border: 1px solid black; padding: 8px;">0.5685</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">771 정보 통신기기 설치 및 수리원</td>
    <td style="border: 1px solid black; padding: 8px;">0.5831</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">799 기타 기능 관련 종사자</td>
    <td style="border: 1px solid black; padding: 8px;">0.5717</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">862 전기 및 전자설비 조작원</td>
    <td style="border: 1px solid black; padding: 8px;">0.5771</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">873 자동차 운전원</td>
    <td style="border: 1px solid black; padding: 8px;">0.5769</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">874 물품 이동 장비 조작원</td>
    <td style="border: 1px solid black; padding: 8px;">0.5656</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">875 건설 및 채굴기계 운전원</td>
    <td style="border: 1px solid black; padding: 8px;">0.5577</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">921 하역 및 적재 단순 종사자</td>
    <td style="border: 1px solid black; padding: 8px;">0.5552</td>
</tr>

<tr>
    <td style="border: 1px solid black; padding: 8px;" rowspan="6">단순직</td>
    <td style="border: 1px solid black; padding: 8px;">922 배달원</td>
    <td style="border: 1px solid black; padding: 8px;">0.5631</td>
    <td style="border: 1px solid black; padding: 8px;" rowspan="6">0.5780</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">941 청소원 및 환경미화원</td>
    <td style="border: 1px solid black; padding: 8px;">0.5501</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">942 건물 관리원 및 검표원</td>
    <td style="border: 1px solid black; padding: 8px;">0.6103</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">953 판매 관련 단순 종사자</td>
    <td style="border: 1px solid black; padding: 8px;">0.5866</td>
</tr>
<tr>
    <td style="border: 1px solid black; padding: 8px;">992 계기·자판기 및 주차 관리 종사자</td>
    <td style="border: 1px solid black; padding: 8px;">0.6027</td>
</tr>
</table>

사업체패널조사자료에서 '창고및운송관련서비스' 업종에 속한 어떤 기업 'i'의 21년도 AI 노출도를 예시적으로 계산하면 아래와 같습니다. 

```
창고및운송관련서비스업 사업체 'i'의 AI 노출도
= 창고및운송관련서비스업 관리직 셀평균×기업 i관리직 직종구성비 +…+ 창고및운송관련서비스업 단순직 셀평균×기업 i의 단순직 직종구성비 
= 0.6333×0.0088594+0.6209×0.0188261+0.5796×0.1838317+0.5796×0+0.6281×0+0.5708×0+0.5780×0.7884828 
= 0.579592
```

### 결과 요약

- 사업체패널조사자료에 포함된 업종 69개`전체 75개 중 사업체 패널 조사 자료에 포함되지 않는 농업, 임업, 어업, 석탄 원유 및 천연가스 광업, 비금속광물 광업, 광업 지원서비스업 6개 업종을 제외한 갯수`와 대분류 직종 7개를 교차하여 업종-직종 교체셀 483개를 만들 수 있습니다. 
- 이 중 관측치가 없는 셀 65개를 제외한 418개 셀의 대표값을 식 (3)에 대입하여 개별 사업체의 AI 노출도를 계산하였습니다. 



| 척도 | N | Mean | SD | Min | Max |
| --- | --- | --- | --- | --- | --- |
| 사업체 AIOE | 11,524 | 0.600002 | 0.020378 | 0.564783 | 0.647254 |
| AIOE 원자료 | 141 | 0.596586 | 0.032784 | 0.52005 | 0.671398 |

차이가 있지만 원자료의 분포를 비슷하게 재현하고 있는 것을 확인할 수 있습니다. 

- 직종 대분류 수준에서 인공지능 직업노출도를 단순 병합하지 않고, 업종과 직종 교차셀의 대표값을 이용하여 업종별 차이를 함께 반영하였습니다. 
- 개별 사업체의 직종별 고용인원에 따라 기업 수준에서 변이합니다. 
- 단순히 대분류 직종의 평균 AI 노출도가 아니라 업종마다 관찰되는 실제 소분류 직종들의 AI 노출도 평균이므로 해당 업종에서 주로 나타나는 세부 직종들의 AI 노출도 수준을 대표합니다. 

전체 데이터는 [csv 파일](industry_occ_matrix.csv)을 참조하세요.
